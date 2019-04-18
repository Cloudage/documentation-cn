电影纹理
=============

__电影纹理__是从视频文件创建的动画__纹理__。通过在项目的 __Assets 文件夹__中放置视频文件，即可导入视频，随后就能就像常规[纹理](class-TextureImporter.html)一样使用视频。

应通过 Apple QuickTime 导入视频文件。支持的文件类型是 QuickTime 程序可以播放的类型（通常为 **.mov**、**.mpg**、**.mpeg**、**.mp4**、**.avi**、**.asf**）。在 Windows 上，需要安装 Quicktime 才能进行电影导入。请从 [Apple 支持网站的下载页面 (Apple Support Downloads)](https://support.apple.com/downloads/quicktime) 下载 Quicktime。


属性
----------


电影纹理 __Inspector__ 与常规的[纹理](class-TextureImporter.html) Inspector 非常相似。


![视频文件用作 Unity 中的电影纹理](../uploads/Main/MovieTextureInInspector.png)


|**_属性：_** |**_功能：_** |
|:---|:---|
|__Aniso Level__ |以大角度查看纹理时提高纹理质量。适用于地板和地面纹理 |
|__Filtering Mode__ |选择纹理在通过 3D 变换拉伸时如何进行过滤 |
|__Loop__ |如果启用此选项，电影将在完成播放时循环播放 |
|__Quality__ |压缩 Ogg Theora 视频文件。值越高意味着质量越高，但文件越大 |


详细信息
-------


将视频文件添加到项目时，该文件将自动导入并转换为 __Ogg Theora__ 格式。导入电影纹理后，即可像常规纹理一样将其附加到任何__游戏对象__或__材质__。

###播放电影

游戏开始运行时不会自动播放电影纹理。必须使用简短的脚本告诉它何时播放。



````
// 这行代码将使电影纹理开始播放
((MovieTexture)GetComponent<Renderer>().material.mainTexture).Play();


````

附加以下脚本可在按下空格键时切换电影播放：



````
public class PlayMovieOnSpace : MonoBehaviour {
    void Update () {
    	if (Input.GetButtonDown ("Jump")) {
    	    
    	    Renderer r = GetComponent<Renderer>();
    	    MovieTexture movie = (MovieTexture)r.material.mainTexture;
    		
    		if (movie.isPlaying) {
    			movie.Pause();
    		}
    		else {
    			movie.Play();
    		}
    	}
    }
}


````

有关播放电影纹理的更多信息，请参阅[电影纹理脚本参考](../ScriptReference/MovieTexture.html)页面


###电影音频

导入电影纹理时，也会导入视觉效果伴随的音频轨道。此音频显示为电影纹理的__音频剪辑 (AudioClip)__ 子级。


![视频的音频轨道在 Project 视图中显示为电影纹理的子级](../uploads/Main/MovieTextureAudio.png)

要播放此音频，必须将音频剪辑附加到游戏对象，就像任何其他音频剪辑一样。将音频剪辑从 Project 视图拖到 Scene 或 Hierarchy 视图中的任何游戏对象上。通常，此游戏对象就是显示电影的游戏对象。然后，使用 [audio.Play()](../ScriptReference/GameObject-audio.html) 使电影的音频轨道随其视频一起播放。
###iOS
在 iOS 上不支持电影纹理。取而代之的是使用 [Handheld.PlayFullScreenMovie](../ScriptReference/Handheld.PlayFullScreenMovie.html) 提供的全屏流媒体播放功能。

###需要将视频保存在项目的 _Assets_ 文件夹中的 _StreamingAssets_ 文件夹内。


Unity iOS 支持 iOS 设备上能正常播放的任何电影文件类型（即扩展名为 **.mov**、**.mp4**、**.mpv** 和 **.3gp** 的文件）并使用以下压缩标准之一：

* H.264 Baseline Profile Level 3.0 视频
* MPEG-4 Part 2 视频

有关受支持的压缩标准的更多信息，请参阅 iPhone SDK [MPMoviePlayerController 类参考](http://developer.apple.com/library/ios/#documentation/MediaPlayer/Reference/MPMoviePlayerController_Class/MPMoviePlayerController/MPMoviePlayerController.html)。

一旦调用 [Handheld.PlayFullScreenMovie](../ScriptReference/Handheld.PlayFullScreenMovie.html)，屏幕就会从当前的内容淡出并淡入到指定的背景颜色。电影准备播放的过程可能需要一些时间，但与此同时，播放器将继续显示背景颜色，还可能显示进度指示条让用户知道电影正在加载。播放完成后，屏幕将淡出并淡入先前的内容。

###视频播放器不支持在播放视频时切换为静音

如上所述，视频文件使用 Apple 的嵌入式播放器进行播放（截至 SDK 3.2 和 iPhone OS 3.1.2 及更早版本）。目前存在阻止 Unity 切换为静音的错误。

###视频播放器不遵循设备的方向
Apple 视频播放器和 iPhone SDK 未提供调整视频方向的方法。一种常见的方法是以横向和纵向方向手动创建每个电影的两个副本。然后，可在播放之前确定设备的方向，从而选择正确的电影版本。

###Android
在 Android 上不支持电影纹理。取而代之的是使用 [Handheld.PlayFullScreenMovie](../ScriptReference/Handheld.PlayFullScreenMovie.html) 提供的全屏流媒体播放功能。

###需要将视频保存在项目的 _Assets_ 文件夹中的 _StreamingAssets_ 文件夹内。


Unity Android 支持 Android 所支持的任何电影文件类型（即扩展名为 **.mp4** 和 **.3gp** 的文件），并使用以下压缩标准之一：

* H.263
* H.264 AVC
* MPEG-4 SP

但是，设备供应商热衷于扩展此列表，因此一些 Android 设备能够播放除所列格式之外的其他格式，例如高清 (HD) 视频。

有关受支持的压缩标准的更多信息，请参阅 Android SDK [核心媒体格式 (Core Media Formats) 文档](http://developer.android.com/guide/appendix/media-formats.html)。

一旦调用 [Handheld.PlayFullScreenMovie](../ScriptReference/Handheld.PlayFullScreenMovie.html)，屏幕就会从当前的内容淡出并淡入到指定的背景颜色。电影准备播放的过程可能需要一些时间，但与此同时，播放器将继续显示背景颜色，还可能显示进度指示条让用户知道电影正在加载。播放完成后，屏幕将淡出并淡入先前的内容。
