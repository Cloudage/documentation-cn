# HoloLens WorldAnchor 持久性
<!-- https://trello.com/c/Qw7imxOL -->
持久性 (Persistence) 是一种在同一应用程序的多次运行之间保存[世界锚点](wmr_input_types.html)状态的机制。这种机制可认为是真实世界中与物理位置相关的“保存游戏”功能。这方面的一个例子是在应用程序重新启动时记住游戏板在世界中的位置。

`WorldAnchorStore` 提供了保存和加载世界锚点的基本功能。应通过调用 `WorldAnchorStore.GetAsync` 并为其提供回调来检索 `WorldAnchorStore`。回调会保存返回的 `WorldAnchorStore` 以便可用于将来的操作。

要保存现有的世界锚点，请为其命名并在 `WorldAnchorStore` 上调用 `Save` 函数。请参阅以下示例：

````
private void SaveAnchor()
{
	if (!this.savedAnchor) // 只保存一次
	{
		this.savedAnchor = this.MyWorldAnchorStore.Save("MyAnchor", MyWorldAnchor);
		if (!this.savedAnchor)
		{
			// 锚点未能保存到存储库。
			//在此处进行错误处理。
		}
	}
}
````
加载本质上是上述情况的反映：

````
private void LoadAnchor()
{
	this.savedAnchor = this.Load("MyAnchor", MyWorldAnchor);
	if (!this.savedAnchor)
	{
		// 未将该名称的锚点保存到存储库。
		// 在此处进行错误处理。
	}
}
````

要从存储库中删除锚点，请在 __WorldAnchorStore__ 上调用 __Delete__ 方法。要从存储库中删除所有锚点，请调用 __Clear__ 方法。

