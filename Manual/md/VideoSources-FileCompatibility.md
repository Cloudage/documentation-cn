# 视频文件兼容性

Unity 可导入许多不同格式的视频文件。导入后，视频文件将存储为[视频剪辑 (VideoClip)](class-VideoClip.html) 资源。

但是，这些文件的兼容性因您使用的平台而异，请参阅下表以获取完整的兼容性列表。

| 扩展名| Windows | OSX | Linux |
|:---|:---|:---|:---| 
| .asf| ✓ |  |  |
| .avi| ✓ |  |  |
| .dv| ✓ | ✓ |  |
| .m4v| ✓ | ✓ |  |
| .mov| ✓ | ✓ |  |
| .mp4| ✓ | ✓ |  |
| .mpg| ✓ | ✓ |  |
| .mpeg| ✓ | ✓ |  |
| .ogv| ✓ | ✓ | ✓ |
| .vp8| ✓ | ✓ | ✓ |
| .webm| ✓ | ✓ | ✓ |
| .wmv| ✓ |  |  |



这些格式中的每一种都可能包含具有许多不同 [编解码器] 的轨道。每个平台的每个版本还支持不同的编解码器列表，因此请务必查阅您正在使用的平台的官方文档。例如，Windows 和 OSX 都提供有关其各自编解码器兼容性的官方文档，请参阅 [Windows](https://msdn.microsoft.com/en-us/library/windows/desktop/dd757927(v=vs.85).aspx) 和 [OSX](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/OSX_Technology_Overview/MediaLayer/MediaLayer.html) 官方文档，了解有关这些平台的更多兼容性信息。

如果 Editor 无法读取文件中某个轨道的内容，则会生成错误消息。如果发生这种情况，必须从源转换或重新编码该轨道，以便 Editor 平台的本机库能够使用它。

H.264（通常采用 .mp4、.m4v 或 .mov 格式）是支持性最佳的视频编解码器，因为它提供最佳的跨平台兼容性。

通过取消选中视频剪辑导入器 (Video Clip Importer) 中的 __Transcode__ 复选框，也可在不进行转码的情况下使用[视频剪辑](class-VideoClip.html)（有关更多信息，请参阅 [视频源] 文档）。进行此设置后，即可直接使用视频文件而无需任何额外的转换，这样将提高速度，并防止因重新编码而导致的质量损失。

__注意：__为获得最佳效果，请务必检查每个目标平台是否支持您的视频文件。

---

* <span class="page-edit">2017-06-15 Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Unity 5.6 中的新功能</span>
