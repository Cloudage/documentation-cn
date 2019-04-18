#音频滤波器

您可以通过应用__音频效果__来修改[音频源 (Audio Source)](class-AudioSource.html) 和[音频监听器 (Audio Listener)](class-AudioListener.html) 组件的输出。这些音效可以过滤声音的频率范围或应用混响和其他效果。

这些效果的实现，需要添加效果组件到带有音频源或音频监听器的对象上。组件的排序很重要，因为它代表应用于音频源的顺序。例如，在下图中，音频监听器首先由音频低通滤波器 (Audio Low Pass Filter) 修改，然后由音频合声滤波器 (Audio Chorus Filter) 修改。

![](../uploads/Main/FilterGraph1.png) 

要更改这些组件和任何其他组件的顺序，请在检视面板中打开上下文菜单，然后选择 _Move Up_ 或 _Move Down_ 命令。启用或禁用效果组件可确定是否应用该组件。

虽然进行了高度优化，但某些滤波器仍然为 CPU 密集型。可在 Audio 选项卡下面的[性能分析器](Profiler.html)中监控音频的 CPU 使用率。

有关具体可用滤波器类型的详细信息，请参阅本部分的其他页面。
