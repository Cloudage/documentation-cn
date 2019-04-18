#高级操作：使用 LLAPI

HLAPI 旨在最大程度减少样板代码，而低级 API (LLAPI) 旨在实现最大的灵活性。通常，使用 LLAPI 会涉及到创建 UnityWebRequest，然后创建相应的 `DownloadHandlers` 或 `UploadHandlers` 并将它们附加到 `UnityWebRequests`。

本部分将详细介绍低级 API 中提供的选项以及目标用途：

* [创建 UnityWebRequest](UnityWebRequest-CreatingUnityWebRequests.html)
* [创建 UploadHandler](UnityWebRequest-CreatingUploadHandlers.html)
* [创建 DownloadHandler](UnityWebRequest-CreatingDownloadHandlers.html)

请注意，HLAPI 和 LLAPI 并不互斥。如果需要调整某个常见方案，始终可以自定义通过 HLAPI 创建的 UnityWebRequest 对象。

有关本部分描述的每个对象的完整详细信息，请参阅 Unity Scripting API。
