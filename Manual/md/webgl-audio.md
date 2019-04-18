#使用 WebGL 中的音频

WebGL 中的音频与所有其他平台上的音频在处理方式上有所不同。在其他平台上，我们在内部使用 FMOD 来提供音频回放和混音。由于 WebGL 平台不支持线程，我们需要采用不同的实现方案，该方案在内部基于 Web 音频 API，允许浏览器为我们处理音频回放和混音。

不幸的是，这限制了 Unity WebGL 中的音频功能，只能支持最基本的功能。本页面将介绍预期有效的功能。WebGL 目前不支持此处未列出的任何其他功能。

##AudioSource

音频源支持基本位置音频回放，具有暂停和恢复、平移、衰减、音高设置和多普勒效应支持。

支持以下 __[`AudioSource`](../ScriptReference/AudioSource.html)__ API：

属性：

* __[`clip`](../ScriptReference/AudioSource-clip.html)__
* __[`dopplerLevel`](../ScriptReference/AudioSource-dopplerLevel.html)__
* __[`ignoreListenerPause`](../ScriptReference/AudioSource-ignoreListenerPause.html)__
* __[`ignoreListenerVolume`](../ScriptReference/AudioSource-ignoreListenerVolume.html)__
* __[`isPlaying`](../ScriptReference/AudioSource-isPlaying.html)__
* __[`loop`](../ScriptReference/AudioSource-loop.html)__
* __[`maxDistance`](../ScriptReference/AudioSource-maxDistance.html)__
* __[`minDistance`](../ScriptReference/AudioSource-minDistance.html)__
* __[`mute`](../ScriptReference/AudioSource-mute.html)__
* __[`pitch`](../ScriptReference/AudioSource-pitch.html)__（请注意，仅支持正音高值。）
* __[`playOnAwake`](../ScriptReference/AudioSource-playOnAwake.html)__
* __[`rolloffMode`](../ScriptReference/AudioSource-rolloffMode.html)__
* __[`time`](../ScriptReference/AudioSource-time.html)__
* __[`timeSamples`](../ScriptReference/AudioSource-timeSamples.html)__
* __[`velocityUpdateMode`](../ScriptReference/AudioSource-velocityUpdateMode.html)__
* __[`volume`](../ScriptReference/AudioSource-volume.html)__

方法：

* __[`Pause`](../ScriptReference/AudioSource.Pause.html)__
* __[`Play`](../ScriptReference/AudioSource.Play.html)__
* __[`PlayDelayed`](../ScriptReference/AudioSource.PlayDelayed.html)__
* __[`PlayOneShot`](../ScriptReference/AudioSource.PlayOneShot.html)__
* __[`PlayScheduled`](../ScriptReference/AudioSource.PlayScheduled.html)__
* __[`SetScheduledEndTime`](../ScriptReference/AudioSource.SetScheduledEndTime.html)__
* __[`SetScheduledStartTime`](../ScriptReference/AudioSource.SetScheduledStartTime.html)__
* __[`Stop`](../ScriptReference/AudioSource.Stop.html)__
* __[`UnPause`](../ScriptReference/AudioSource.UnPause.html)__
* __[`PlayClipAtPoint`](../ScriptReference/AudioSource.PlayClipAtPoint.html)__

##AudioListener

支持所有 __[`AudioListener`](../ScriptReference/AudioListener.html)__ API。

##AudioClip

WebGL 中的音频剪辑将始终以 AAC 格式导入，因为不同浏览器广泛支持该格式。

支持以下所有 __[`AudioClip`](../ScriptReference/AudioClip.html)__ API。
支持的 API：

属性：

* __[`length`](../ScriptReference/AudioClip-length.html)__
* __[`loadState`](../ScriptReference/AudioClip-loadState.html)__
* __[`samples`](../ScriptReference/AudioClip-samples.html)__

方法：

* __[`Create`](../ScriptReference/AudioClip.Create.html)__。部分支持 `AudioClip.Create`：仅当 streaming 参数设置为 false 时才有效，并可在调用 AudioClip.Create 时加载完整的音频样本。然后，该函数将创建剪辑并在返回控制之前加载所有样本。
* __[`SetData`](../ScriptReference/AudioClip.SetData.html)__。部分支持 `AudioClip.SetData`：只能用于替换 AudioClip 的全部内容。`offsetSamples` 参数将被忽略。

##WWW.audioClip

如果音频剪辑采用浏览器本机支持的格式，则 __[`WWW.audioClip`](../ScriptReference/WWW-audioClip.html)__ 应该能在 WebGL 中工作。请参阅[此处](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats)以获取不同浏览器中支持的格式列表。

##Microphone

WebGL 不支持 __[`Microphone`](../ScriptReference/Microphone.html)__ 类。
