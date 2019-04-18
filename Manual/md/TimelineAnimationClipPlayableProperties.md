# 动画剪辑可播放资源属性

使用 Inspector 窗口可更改动画剪辑的可播放资源属性。可播放资源属性包括用于手动变换动画剪辑的根运动偏移的控件和用于覆盖默认剪辑匹配的选项。

要查看可播放资源动画剪辑属性，请在 Timeline Editor 窗口中选择动画剪辑，然后在 Inspector 窗口中展开 **Animation Playable Asset**。

![Inspector 窗口中显示了所选动画剪辑的可播放资源属性](../uploads/Main/timeline_inspector_animation_clip_playable.png)

## Clip Root Motion Offsets

使用 **Clip Root Motion Offsets** 可将位置和旋转偏移应用于所选动画剪辑的根运动。应用剪辑根偏移有两种方法：

* 通过[匹配剪辑偏移](TimelineMatchOffsets.html)根据上一个动画剪辑的结尾或下一个动画剪辑的开头来设置根运动偏移。匹配结果取决于 **Clip Offset Match Fields**。

* 使用 **Clip Root Motion Offsets** 下的工具和属性手动设置根运动偏移的位置和旋转。

|**属性：** |**功能：** |
|:---|:---|
|__Move tool__| 启用移动工具 (Move tool) 可在 Scene 视图中显示移动辅助图标 (Move Gizmo)。使用移动辅助图标可手动定位所选动画剪辑的根运动偏移。使用移动辅助图标会更改 Position 属性。 |
|__Rotate tool__ | 启用旋转工具 (Rotate tool) 可在 Scene 视图中显示旋转辅助图标 (Rotate Gizmo)。使用旋转辅助图标可手动旋转所选动画剪辑的根运动偏移。使用旋转辅助图标会更改 Rotation 属性。 |
|__Position__ | 使用 Position 属性可手动设置剪辑在 X、Y 和 Z 坐标上的偏移。默认情况下，位置坐标设置为零并且相对于[轨道偏移](TimelineAnimationTrackProperties.html)。 |
|__Rotation__ | 使用 Rotation 属性可手动设置剪辑围绕 X、Y 和 Z 轴的旋转偏移。默认情况下，旋转坐标设置为零并且相对于[轨道偏移](TimelineAnimationTrackProperties.html)。 |

## Clip Offset Match Fields

使用 **Clip Offset Match Fields** 可选择[匹配剪辑偏移](TimelineMatchOffsets.html)时匹配的变换。默认情况下会禁用 **Override Track Match Fields**，而 **Clip Offset Match Fields** 会显示并使用在[动画轨道级别](TimelineAnimationTrackProperties.html)设置的匹配选项。

启用 **Override Track Match Fields** 可覆盖轨道匹配选项，并设置与所选动画剪辑匹配的变换。

---
* <span class="page-edit">2017-12-07  Page published with limited [editorial review](DocumentationEditorialReview.html)
</span>

