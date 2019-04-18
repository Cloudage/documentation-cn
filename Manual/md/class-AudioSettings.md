#音频设置

AudioSettings 类包含与声音系统相关的各种全局信息，最重要的是它包含的 API，允许在运行时重置音频系统，以便更改扬声器模式、采样率（如果平台支持）、DSP 缓冲区大小和真实/虚拟语音计数等设置。

其中许多设置也可以在项目设置的 Audio 部分进行配置。更改设置后，这些设置将同时应用于编辑器并定义游戏的初始状态，而使用 AudioSettings API 执行的更改仅适用于游戏运行时，游戏停止运行后，项目设置仍然是编辑器中定义的初始设置。

游戏可以提供声音选项菜单，用户可以在此菜单中更改声音设置，更改也可能是来自于外设，例如，插入外部音频输入/输出设备甚至 HDMI 监视器（也可充当音频设备）。AudioConfiguration AudioSettings.GetConfiguration() / bool AudioSettings.Reset(AudioConfiguration config) API 可读取和应用对当前声音系统配置的全局更改，基本上取代了 AudioSettings.SetDSPBufferSize(...) 函数和 AudioSettings.outputSampleRate / AudioSettings.speakerMode（这些函数在属性被修改后重新初始化整个音频系统时会产生副作用）。

该 API 定义的 AudioSettings.OnAudioConfigurationChanged(bool device) 可设置一个回调，通过此回调可向脚本发出关于音频配置更改的通知。这些更改可能是由实际设备更改引起的，也可能是由脚本启动的配置引起的。

请务必注意，只要执行全局音频系统配置的运行时修改，就必须重新加载所有音频对象。此原则适用于基于磁盘的音频剪辑资源和混音器，但脚本生成或修改的任何音频剪辑都将丢失，必须重新创建。同样，任何播放状态也都会丢失，因此需要在 AudioSettings.OnAudioConfigurationChanged(...) 回调中重新生成这些状态。

有关详细信息和示例，请参阅脚本 API 参考。
