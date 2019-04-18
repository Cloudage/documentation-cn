#自定义 Android 启动画面

如果使用 Unity 个人版来开发项目，则会在游戏加载时显示默认的启动画面。该画面根据 [Player Settings](class-PlayerSettings.html) 中的 __Default Screen Orientation__ 选项进行定向。无法更改此启动画面。

Unity 专业版用户可以更改启动画面。项目中的任何纹理均可用作启动画面。可从 Android [Player Settings](class-PlayerSettings.html) 的 Splash Image 部分设置该纹理。还应从以下选项中选择 __Splash scaling__ 方法：

* __Center (only scale down)__ 以自然大小绘制图像，除非图像太大（在这种情况下会缩小图像来进行适应）。
* __Scale to fit (letter-boxed)__ 在绘制图像时让较长的尺寸精确适应屏幕大小。较短尺寸两侧的空白空间填充为黑色。
* __Scale to fill (cropped)__ 会缩放图像以使较短的尺寸精确适应屏幕大小。图像的较长尺寸将被裁剪。

