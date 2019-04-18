流媒体资源 (Streaming Assets)
================


Unity 中的大多数资源在构建时都会合并到项目中。但是，将文件放入目标计算机上的普通文件系统以使其可通过路径名访问有时会很有用。这方面的一个例子是在 iOS 设备上部署电影文件；原始电影文件必须位于文件系统中的某个位置以便由 `PlayMovie` 函数进行播放。

放置在 Unity 项目中名为 __StreamingAssets__（区分大小写）的文件夹中的所有文件都将逐字复制到目标计算机上的特定文件夹。可使用 [Application.streamingAssetsPath](../ScriptReference/Application-streamingAssetsPath.html) 属性来检索此文件夹。在任何情况下，最好使用 `Application.streamingAssetsPath` 来获取 __StreamingAssets__ 文件夹的位置，因为它总是指向运行应用程序的平台上的正确位置。

此文件夹的位置因平台而异。请注意，以下名称区分大小写：

* 在桌面计算机（Mac OS 或 Windows）上，可使用以下代码获取文件的位置：


    	 path = Application.dataPath + "/StreamingAssets";

* 在 iOS 上，使用：


    	 path = Application.dataPath + "/Raw";

* 在 Android 上，使用：


    	 path = "jar:file://" + Application.dataPath + "!/assets/";


在 Android 上，这些文件包含在压缩的 .jar 文件（其格式与标准的 zip 压缩文件基本相同）中。这意味着，如果不使用 Unity 的 WWW 类来检索文件，则需要使用其他软件查看 .jar 存档内部并获取文件。
    
**注意**：位于 __StreamingAssets__ 文件夹中的 .dll 文件不参与编译。

