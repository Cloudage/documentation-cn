# 字符串和文本

字符串和文本的处理不当是 Unity 项目中性能问题的常见原因。在 C# 中，所有字符串均不可变。对字符串的任何操作均会导致分配一个完整的新字符串。这种操作的代价相对比较高，而且在大型字符串上、大型数据集上或紧凑循环中执行时，接连不断的重复的字符串可能发展成性能问题。

此外，由于 N 个字符串连接需要分配 N-1 个中间字符串，串行连接也可能成为托管内存压力的主要原因。

如果必须在紧凑循环中或每帧期间对字符串进行连接，请使用 StringBuilder 执行实际连接操作。为最大限度减少不必要的内存分配，可重复使用 StringBuilder 实例。

Microsoft 整理了一份处理 C# 中的字符串的最佳做法清单，可在这里的 MSDN 网站上找到该清单：[msdn.microsoft.com](https://msdn.microsoft.com/en-us/library/dd465121(v=vs.110).aspx)。

## 区域约束与序数比对

在与字符串相关的代码中经常出现的核心性能问题之一是无意间使用了缓慢的默认字符串 API。这些 API 是为商业应用程序构建的，可根据与文本字符有关的多种不同区域性和语言规则来处理字符串。

例如，在美国英语区域设置下运行时，以下示例代码将返回 true，但在许多欧洲区域设置下，将返回 false (1)

注意：
从 Unity 5.3 和 5.4 开始，Unity 的脚本运行时始终在美国英语 (en-US) 区域设置下运行：

```
    String.Equals("encyclopedia", “encyclopædia”);
```

对于大多数 Unity 项目，上述代码完全没有必要。使用序数比对可将速度提高大约十倍，这种比较类型以 C 和 C++ 工程师熟悉的方式比较字符串：简单地比较字符串的每个连续字节，不考虑该字节所表示的字符。

切换至序数比对的方式非常简单，只需将 `StringComparison.Ordinal` 作为最终参数提供给 `String.Equals`：

```
myString.Equals(otherString, StringComparison.Ordinal);

```

## 低效的内置字符串 API

除了切换至序数比对以外，目前已知某些 C# `String` API 的效率极低，其中包括 `String.Format`、`String.StartsWith` 和 `String.EndsWith`。尽管 `String.Format` 难以替换，但低效率字符串比较方法很容易优化掉。

尽管 Microsoft 建议将 `StringComparison.Ordinal` 传递给任何不需要为本地化做调整的字符串比较，但 Unity 基准测试表明，相比自定义实现，该方法对性能的提升效果有限。

| 方法| 100k 短字符串的时间（毫秒） |
|:---|:---| 
| `String.StartsWith`，默认区域性| 137 |
| `String.EndsWit`h，默认区域性| 542 |
| `String.StartsWith`，序数| 115 |
| `String.EndsWith`，序数| 34 |
| 自定义 `StartsWith` 替换| 4.5 |
| 自定义 `EndsWith` 替换| 4.5 |



`String.StartsWith` 和 `String.EndsWith` 均可以替换为类似于以下示例的简单的手工编码版本。

```

    public static bool CustomEndsWith(string a, string b) {
            int ap = a.Length - 1;
            int bp = b.Length - 1;

            while (ap >= 0 && bp >= 0 && a [ap] == b [bp]) {
                ap--;
                bp--;
            }
            return (bp < 0 && a.Length >= b.Length) || 

                    (ap < 0 && b.Length >= a.Length);
            }

        public static bool CustomStartsWith(string a, string b) {
            int aLen = a.Length;
            int bLen = b.Length;
            int ap = 0; int bp = 0;

            while (ap < aLen && bp < bLen && a [ap] == b [bp]) {
            ap++;
            bp++;
            }

            return (bp == bLen && aLen >= bLen) || 

                    (ap == aLen && bLen >= aLen);
        } 

```

## 正则表达式

尽管正则表达式是匹配和操作字符串的强大方法，但它们可能对性能的影响极大。此外，由于 C# 库的正则表达式实现方式，即使简单的布尔值 `IsMatch` 查询也需要在底层分配大型瞬态数据结构。除非在初始化期间，否则这种瞬态托管内存波动都是不可接受的。

如果必须使用正则表达式，强烈建议不要使用静态 `Regex.Match` 或 `Regex.Replace` 方法，这些方法会将正则表达式视为字符串参数。这些方法即时编译正则表达式，并且不缓存生成的对象。


以下示例代码为无害的单行代码。

```

Regex.Match(myString, "foo");

```

但是，该代码每次执行时会产生 5 KB 的垃圾。通过简单的重构即可消除其中的大部分垃圾：

```

var myRegExp = new Regex("foo");

myRegExp.Match(myString);

```

在本示例中，每次调用 `myRegExp.Match`“只”产生 320 字节的垃圾。尽管这对于简单的匹配操作仍然代价高昂，但比前面的示例有了相当大的改进。

因此，如果正则表达式是不变的字符串字面值，通过将正则表达式传递为正则表达式对象构造函数的第一个参数来预编译它们，可显著提高效率。这些预编译的正则表达式之后会被重用。

## XML、JSON 和其他长格式文本解析

解析文本通常是加载期间所发生的最繁重的操作之一。在某些情况下，解析文本所花费的时间可能超过加载和实例化资源所花费的时间。

此问题背后的原因取决于所使用的具体解析器。C# 的内置 XML 解析器极为灵活，但因此无法针对具体数据布局进行优化。

许多第三方解析器都是基于反射构建的。尽管反射在开发过程中是绝佳选择（因为它能让解析器快速适应不断变化的数据布局），但众所周知，它的速度非常慢。

Unity 引入了采用其内置 [JSONUtility](../ScriptReference/JsonUtility.html) API 的部分解决方案，该解决方案提供了读取/发出 JSON 的 Unity 序列化系统接口。在大多数基准测试中，它比纯 C# JSON 解析器快，但它与 Unity 序列化系统的其他接口具有相同的限制：没有额外代码的情况下，无法对许多复杂的数据类型（如字典）进行序列化(2)（注意：
	 请参阅 [ISerializationCallbackReceiver](../ScriptReference/ISerializationCallbackReceiver.html) 接口，了解如何通过一种方法轻松添加必要的额外处理以便在 Unity 序列化过程中来回转换复杂数据类型）。

当遇到文本数据解析所引起的性能问题时，请考虑三种替代解决方案。

### 方案 1：在构建时解析

避免文本解析成本的最佳方法是完全取消运行时文本解析。通常，这意味着通过某种构建步骤将文本数据“烘焙”成二进制格式。

大多数选择使用该方法的开发者会将其数据移动到某种 ScriptableObject 衍生的类层级视图中，然后通过 AssetBundle 分配数据。有关使用 ScriptableObjects 的精彩讨论，请参阅 youtube 上 [Richard Fine 的 Unite 2016 讲座](https://www.youtube.com/watch?v=VBA1QCoEAX4)。

该策略可实现尽可能高的性能，但只适用于不需要动态生成的数据。它适用于游戏设计参数和其他内容。

### 方案 2：拆分和延迟加载

第二种可行的方法是将必须解析的数据拆分为较小的数据块。拆分后，解析数据的成本可分摊到多个帧。在理想的情况下，可识别出为用户提供所需体验而需要的特定数据部分，然后只加载这些部分。

举一个简单的例子：如果项目为平台游戏，则没必要将所有关卡的数据一起序列。如果将数据拆分为每个关卡的独立资源，并且将关卡划分到区域中，则可以在玩家闯关到相应位置时再解析数据。

虽然这听起来不难，但实际上需要在工具编码方面投入大量精力，并可能需要重组数据结构。

### 方案 3：线程

如果数据完全解析成纯 C# 对象，并且不需要与 Unity API 进行任何交互，则可以将解析操作移至工作线程。

该方案在具有大量核心的平台上非常强大(3)（注意：
iOS 设备最多有 2 个核心。大多数 Android 设备具有 2-4 个核心。该技术适用于针对电脑平台和游戏主机发布的项目。）但是，该方案需要仔细编程，以免产生死锁和竞态条件。

选择实现线程的项目通常使用内置的 C# [Thread](https://msdn.microsoft.com/en-us/library/system.threading.thread(v=vs.110).aspx) 和 [ThreadPool](https://msdn.microsoft.com/en-us/library/system.threading.threadpool(v=vs.110).aspx) 类（请参阅 [msdn.microsoft.com](https://msdn.microsoft.com/en-us/library/system.threading.thread(v=vs.110).aspx)）来管理其工作线程以及标准 C# 同步类。

__脚注__

* (1) 请注意，从 Unity 5.3 和 5.4 开始，Unity 的脚本运行时始终在美国英语 (en-US) 区域设置下运行。

* (2) 请参阅 [ISerializationCallbackReceiver](../ScriptReference/ISerializationCallbackReceiver.html) 接口，了解如何通过一种方法轻松添加必要的额外处理以便在 Unity 序列化过程中来回转换复杂数据类型。

* (3) 请注意，iOS 设备最多有 2 个核心。大多数 Android 设备具有 2-4 个核心。该技术适用于针对电脑平台和游戏主机发布的项目。

