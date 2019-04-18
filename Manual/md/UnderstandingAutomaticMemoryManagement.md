了解自动内存管理
=========================================


创建对象、字符串或数组时，用于存储它的内存是从称为**堆**的中央池分配的。当此项不再使用时，其先前占用的内存可被回收并用于其他目的。在过去，通常由程序员通过适当的函数调用显式地分配和释放这些堆内存块。如今，Unity 的 Mono 引擎等运行时系统会自动为您管理内存。自动内存管理比显式分配/释放的做法需要更少的编码工作，并且大大降低了内存泄漏的可能性（即分配了内存但后续从未释放的情况）。


价值和引用类型
-------------------------


调用某个函数时，其参数的值将复制到为该特定调用保留的内存区域。只占几个字节的数据类型可以非常快速和轻松地完成复制。但是，对象、字符串和数组通常要大得多，如果需要经常复制这些类型的数据，效率会非常低。幸运的是，并不是非要这样做；可从堆中分配大项的实际存储空间，并使用小“指针”值来记住它的位置。此后，在参数传递期间只需要复制指针。只要运行时系统能找到该指针所标识的项，就可以根据需要频繁使用该数据的同一个副本。

在参数传递期间直接存储和复制的类型称为值类型。这些类型包括整数、浮点数、布尔值和 Unity 的结构类型（例如，__Color__ 和 __Vector3__）。在堆上分配后再通过指针访问的类型称为引用类型，因为在变量中存储的值仅“引用”实际数据。引用类型的示例包括对象、字符串和数组。


分配和垃圾收集
---------------------------------


内存管理器跟踪已知未使用的堆区域。当请求新的内存块时（例如，当实例化对象时），管理器选择一个未使用的区域来分配内存块，然后从已知的未使用空间中移除分配的内存。后续请求以相同的方式处理，直到没有足够大的可用区域来分配所需的块大小。此时极不可能从堆中分配的所有内存都仍在使用中。若要访问堆上的引用项，前提是仍有引用变量可以定位到该项。如果对内存块的所有引用都消失（即，引用变量已被重新分配，或者引用变量是局部变量但现在已超出范围），则可安全地重新分配其占用的内存。

为确定哪些堆块已不再使用，内存管理器会搜索所有当前处于活动状态的引用变量，并将它们引用的块标记为“实时”。在搜索结束时，内存管理器会认为实时块之间的任何空间都是空的并可用于后续分配。由于显而易见的原因，定位和释放未使用的内存的过程称为垃圾收集（或简称 GC）。


优化
------------


垃圾收集是自动完成的，对程序员来说不可见，但收集过程实际上在后台需要耗费大量 CPU 时间。如果使用得当，自动内存管理通常在整体性能上能达到或超过手动分配。但是，程序员必须避免错误以免导致不必要的频繁触发垃圾回收器并在执行中引起暂停。

有一些臭名昭着的算法虽然一眼看上去好像是无辜的，但可能成为 GC 的噩梦。重复的字符串连接便是一个典型的例子：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void ConcatExample(int[] intArray) {
		string line = intArray[0].ToString();
		
		for (i = 1; i < intArray.Length; i++) {
			line += ", " + intArray[i].ToString();
		}
		
		return line;
	}
}


//JS 脚本示例
function ConcatExample(intArray: int[]) {
	var line = intArray[0].ToString();
	
	for (i = 1; i < intArray.Length; i++) {
		line += ", " + intArray[i].ToString();
	}
	
	return line;
}
````

此处的关键细节是新的部分不会逐一添加到字符串。实际情况的是，每次循环时，line 变量的先前内容变为死亡状态：分配的整个新字符串将包含原始部分加上末尾的新部分。由于字符串随着 i 值的增加而变长，因此消耗的堆空间量也会增加，所以每次调用此函数时都很容易用掉数百个字节的空闲堆空间。如果需要将大量字符串连接在一起，那么 Mono 库的 [System.Text.StringBuilder](http://msdn.microsoft.com/en-gb/library/system.text.stringbuilder.aspx) 类将是更好的选择。

但是，即使重复的连接也不会造成太大麻烦，除非频繁调用，而在 Unity 中这通常意味着帧更新。类似以下脚本：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public GUIText scoreBoard;
	public int score;
	
	void Update() {
		string scoreText = "Score: " + score.ToString();
		scoreBoard.text = scoreText;
	}
}


//JS 脚本示例
var scoreBoard: GUIText;
var score: int;

function Update() {
	var scoreText: String = "Score: " + score.ToString();
	scoreBoard.text = scoreText;
}
````

...在每次调用 Update 时都会分配新的字符串，并生成源源不断的垃圾。通过仅在 score 发生变化时才更新 text，可避免大部分的垃圾：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	public GUIText scoreBoard;
	public string scoreText;
	public int score;
	public int oldScore;
	
	void Update() {
		if (score != oldScore) {
			scoreText = "Score: " + score.ToString();
			scoreBoard.text = scoreText;
			oldScore = score;
		}
	}
}


//JS 脚本示例
var scoreBoard: GUIText;
var scoreText: String;
var score: int;
var oldScore: int;

function Update() {
	if (score != oldScore) {
		scoreText = "Score: " + score.ToString();
		scoreBoard.text = scoreText;
		oldScore = score;
	}
}
````

另一个潜在问题是在函数返回数组值时出现的问题：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	float[] RandomList(int numElements) {
		var result = new float[numElements];
		
		for (int i = 0; i < numElements; i++) {
			result[i] = Random.value;
		}
		
		return result;
	}
}


//JS 脚本示例
function RandomList(numElements: int) {
	var result = new float[numElements];
	
	for (i = 0; i < numElements; i++) {
		result[i] = Random.value;
	}
	
	return result;
}
````


在新建包含值的数组时，这种类型的函数非常从容和方便。但是，如果重复调用这种函数，则每次都会分配全新的内存。由于数组可能非常大，因此空闲堆空间可能会迅速耗尽，导致频繁进行垃圾收集。避免此问题的一种方法是利用数组为引用类型这一特点。作为参数传入该函数的数组可在该函数内予以修改，且结果在函数返回后仍然保留。像上面这样的函数通常可替换为如下所示的函数：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void RandomList(float[] arrayToFill) {
		for (int i = 0; i < arrayToFill.Length; i++) {
			arrayToFill[i] = Random.value;
		}
	}
}


//JS 脚本示例
function RandomList(arrayToFill: float[]) {
	for (i = 0; i < arrayToFill.Length; i++) {
		arrayToFill[i] = Random.value;
	}
}
````


此函数仅将数组的现有内容替换为新值。虽然这需要在调用代码中完成数组的初始分配（看起来有点不方便），但在调用该函数时不会产生任何新的垃圾。


请求进行垃圾收集
-----------------------

如上所述，最好尽量避免内存分配。但是，鉴于无法完全消除这些行为，可采用两种主要策略来最小化这些行为对游戏运行过程的干扰：

###快速和频繁进行垃圾收集的小堆

这种策略通常最适合游戏运行过程很长且主要关注帧率平滑性的游戏。像这样的游戏通常会频繁分配小块，但这些块的使用时间很短暂。在 iOS 上使用此策略时的典型堆大小约为 200KB，在 iPhone 3G 上的垃圾收集时间大约需要 5ms。如果堆大小增加到 1MB，则收集时间将大约需要 7ms。因此，有时，以定期的帧间隔请求进行垃圾收集可能是有利的。这种情况下通常会使垃圾收集频率高于严格意义上的要求，但是这些行为将得到快速处理，并且对游戏运行过程的影响极小：

````
if (Time.frameCount % 30 == 0)
{
   System.GC.Collect();
}
````

但是，应谨慎使用此技术并检查性能分析器的统计信息，以确保真正减少了游戏的垃圾收集时间。

###慢速但不频繁进行垃圾收集的大堆

这种策略最适合内存分配（因此垃圾收集）相对不频繁并可在游戏运行过程的暂停期间进行处理的游戏。一种非常有用的方法是，尽可能增大堆的大小，但不至于因为系统内存不足而导致操作系统终止您的应用程序。但是，Mono 运行时会尽可能避免自动扩展堆。这种情况下，可通过在启动期间预先分配一些占位空间来手动扩展堆（即，实例化一个纯粹为了影响内存管理器而分配的“无用”对象）：

````
//C# 脚本示例
using UnityEngine;
using System.Collections;

public class ExampleScript : MonoBehaviour {
	void Start() {
		var tmp = new System.Object[1024];
		
		// 以较小的块进行分配，从而避免以适合大块的特殊方式处理它们
		for (int i = 0; i < 1024; i++)
			tmp[i] = new byte[1024];
		
		// 释放引用
		tmp = null;
	}
}


//JS 脚本示例
function Start() {
	var tmp = new System.Object[1024];

	// 以较小的块进行分配，从而避免以适合大块的特殊方式处理它们
        for (var i : int = 0; i < 1024; i++)
		tmp[i] = new byte[1024];

	// 释放引用
        tmp = null;
}
````


一个足够大的堆不应在游戏运行过程中配合进行垃圾收集的暂停期间完全耗尽。发生此类暂停时，可显式请求垃圾收集：

````
System.GC.Collect();
````

同样，在使用此策略时应谨慎，并注意性能分析器的统计信息，而不能仅仅期待其具有所需的效果。

可重用的对象池
---------------------


在许多情况下，通过减少创建和销毁的对象数量即可避免生成垃圾。游戏中存在某些类型的对象，例如飞弹，可能会多次反复遇到，但是只有少数对象会同时处于游戏中。在这种情况下，通常可以重用对象，而不是销毁旧对象并替换为新对象。


其他信息
-------------------


内存管理是一个微妙而复杂的主题，业界已投入了大量的学术努力。如果有兴趣了解这一主题，[memorymanagement.org](http://www.memorymanagement.org/) 将是极好的资源，其中列出了大量出版物和在线文章。如需了解对象池的更多信息，请访问 [Wikipedia 页面](http://en.wikipedia.org/wiki/Object_pool_pattern)以及 [Sourcemaking.com](http://sourcemaking.com/design_patterns/object_pool)。
