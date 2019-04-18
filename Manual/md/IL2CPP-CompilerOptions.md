#编译器选项

使用 [IL2CPP](IL2CPP.html) 脚本后端时，可以控制 __il2cpp.exe__ 如何生成 C++ 代码。具体来说，可使用 C# 属性启用或禁用下面列出的运行时检查。

| **_选项_** | **_描述_** | **_默认值_** | 
|:---|:---|:---| 
|__Null checks__ | 如果启用此选项，则 IL2CPP 生成的 C++ 代码将包含 null 检查，并根据需要抛出托管的 NullReferenceException 异常。如果禁用此选项，则不会将 null 检查放入生成的 C++ 代码中。对于某些项目，禁用此选项可能会提高运行时性能。<br/>但是，在生成的代码中对 null 值的任何访问都不会受到保护，并可能会导致不正确的行为。_通常情况下_，游戏将在取消引用 null 值后很快崩溃，但我们无法保证这一点。请谨慎禁用此选项。 | Enabled | 
|__Array bounds checks__ | 如果启用此选项，则 IL2CPP 生成的 C++ 代码将包含数组边界检查，并根据需要抛出托管的 IndexOutOfRangeException 异常。如果禁用此选项，则不会将数组边界检查放入生成的 C++ 代码中。<br/>对于某些项目，禁用此选项可能会提高运行时性能。但是，在生成的代码中对具有无效索引的数组的任何访问都不会受到保护，并可能导致不正确的行为，包括读取或写入任意内存位置。在大多数情况下，这些内存访问在发生时不会表现出任何直接副作用，可能会悄无声息破坏游戏状态。禁用此选项要极度谨慎。 | Enabled | 
|__Divide by zero checks__ | 如果启用此选项，则 IL2CPP 生成的 C++ 代码将包含整数除法的除以零检查，并根据需要抛出托管的 DivideByZeroException 异常。如果禁用此选项，则不会将整数除法的除以零检查放入生成的 C++ 代码中。对于大多数项目，应禁用此选项。仅在需要进行除以零检查时才启用，因为这些检查具有运行时成本。 | Disable | 

可使用 `Il2CppSetOptions` 属性在 C# 代码中启用或禁用运行时检查。要使用此属性，请在计算机上的 Unity Editor 安装中的 *IL2CPP* 目录中找到 *Il2CppSetOptionsAttribute.cs* 源文件。（在 Windows 上位于 *Data\il2cpp*，在 OS X 上位于 *Contents/Frameworks/il2cpp*）。将此源文件复制到项目中的 *Assets* 文件夹中。

现在按照以下示例中所示使用该属性。

```
[Il2CppSetOption(Option.NullChecks, false)]
public static string MethodWithNullChecksDisabled()
{
	var tmp = new object();
	return tmp.ToString();
}
```

可将 `Il2CppSetOptions` 属性应用于类型、方法和属性。Unity 在最局部的作用域内使用该属性。

```
[Il2CppSetOption(Option.NullChecks, false)]
public class TypeWithNullChecksDisabled
{
	public static string AnyMethod()
	{
		// 在此方法中将禁用 null 检查。
		var tmp = new object();
		return tmp.ToString();
	}

	[Il2CppSetOption(Option.NullChecks, true)]
	public static string MethodWithNullChecksEnabled()
	{
		// 此方法中将启用 null 检查。
		var tmp = new object();
		return tmp.ToString();
	}
}
```

```
public class SomeType
{
	[Il2CppSetOption(Option.NullChecks, false)]
	public string PropertyWithNullChecksDisabled
	{
		get
		{
			// 此处将禁用 null 检查。
			var tmp = new object();
			return tmp.ToString();
		}
		set
		{
			// 此处将禁用 null 检查。
			value.ToString();
		}
	}

	public string PropertyWithNullChecksDisabledOnGetterOnly
	{
		[Il2CppSetOption(Option.NullChecks, false)]
		get
		{
			//此处将禁用 null 检查。
			var tmp = new object();
			return tmp.ToString();
		}
		set
		{
			//此处将启用 null 检查。
			value.ToString();
		}
	}
}
```
