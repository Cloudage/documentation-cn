#HoloLens 锚点共享
<!-- https://trello.com/c/Qw7imxOL -->
锚点共享 (Anchor Sharing) 是一种将[世界锚点](wmr_input_types.html)保存在一台设备上之后可加载到其他设备上的机制。

例如：两个用户使用放置在桌子上的虚拟游戏板玩游戏。为了让两个用户的游戏板对齐，两个设备都需要了解虚拟游戏板相对于真实世界的空间位置。锚点共享允许从一个用户的设备保存锚点信息，然后在另一个用户的设备上重新创建该信息。

请注意，锚点共享功能不包含在设备之间传输数据所需的传输层。网络传输系统提供此传输功能。请参阅关于[网络](UNet.html)的 Unity 文档以了解更多与此相关的信息。

##导出锚点

要导出现有的世界锚点，必须具有该锚点的共享名称以及 `WorldAnchorTransferBatch`。

以下示例显示了此 API 的基本用法。请注意：

1.应该多次调用 `OnExportDataAvailable`。
2.如果将应用程序设置为以增量方式将数据发送到其他设备，则还需要处理发送了数据但在导出调用中途失败的情况。

````
private void ExportWorldAnchor()
{
	WorldAnchorTransferBatch transferBatch = new WorldAnchorTransferBatch();
	transferBatch.AddWorldAnchor("GameRootAnchor", this.MyWorldAnchor);
	WorldAnchorTransferBatch.ExportAsync(transferBatch, OnExportDataAvailable, OnExportComplete);
}

private void OnExportComplete(SerializationCompletionReason completionReason)
{
	if (completionReason != SerializationCompletionReason.Succeeded)
	{
		// 如果我们一直在传输数据但失败了，
		// 告诉客户端将数据丢弃
		SendExportFailedToClient();
	}
	else
	{
		// 告诉客户端序列化已成功。
		//一旦收到所有数据，客户端就可以开始导入。
		SendExportSucceededToClient();
	}
}

private void OnExportDataAvailable(byte[] data)
{
	//将字节发送到客户端。还可以缓冲数据。
	TransferDataToClient(data); 
}
````

##导入锚点

收到数据后，通过 `WorldAnchorTransferBatch` 将其导入，然后在另一个客户端上重新创建世界锚点。

以下示例显示了基本用法。请注意，导入有时会失败；如果发生这种情况，请重试该过程。

````
private int retryCount = 10;
private void ImportWorldAnchor(byte[] importedData)
{
	WorldAnchorTransferBatch.ImportAsync(importedData, OnImportComplete);
}
​
private void OnImportComplete(SerializationCompletionReason completionReason, WorldAnchorTransferBatch deserializedTransferBatch)
{
	if (completionReason != SerializationCompletionReason.Succeeded)
	{
		Debug.Log("Failed to import: " + completionReason.ToString());
		if (retryCount > 0)
		{
			retryCount--;
			WorldAnchorTransferBatch.ImportAsync(fileData, OnImportComplete);
		}
		return;
	}
​
	string[] ids = deserializedTransferBatch.GetAllIds();
	foreach (string id in ids)
	{
		GameObject gameObject = GetGameObjectFromAnchorId(id);
		if (gameObject != null)
		{
			transferBatch.LockObject(id, gameObject);
		}
		else
		{
			Debug.Log("Failed to find object for anchor id: " + id);
		}
	}
}
````
