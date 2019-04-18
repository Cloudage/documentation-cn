Animation 窗口指南
=============================


Unity 中的 __Animation 窗口__让您可以直接在 Unity 内创建和修改__动画剪辑__。它旨在充当外部 3D 动画程序的强大而直接的替代方案。除了对运动进行动画化，编辑器还允许您对材质和组件的变量进行动画化，并使用__动画事件__函数（在时间轴上的指定点调用这些函数）来充实动画剪辑。

请参阅关于[动画导入](AnimationsImport.html)和[动画脚本](AnimationScripting.html)的页面以了解关于这些主题的更多信息。

---

##What's the difference between the Animation window and the Timeline window?


###The Timeline window

The [Timeline window](TimelineEditorWindow.html) allows you to create cinematic content, game-play sequences, audio sequences and complex particle effects. You can animate many different GameObjects within the same sequence, such as a cut scene or scripted sequence where a character interacts with scenery. In the timeline window you can have multiple types of [track](TimelineTrackList.html), and each track can contain multiple [clips](TimelineClipsView.html) that can be moved, trimmed, and blended between. It is useful for creating more complex animated sequences that require many different GameObjects to be choreographed together.

The Timeline window is newer than the Animation window. It was added to Unity in version 2017.1, and supercedes some of the functionality of the Animation window. To start learning about Timeline in Unity, visit the [Timeline section](TimelineSection.html) of the user manual.

![The Timeline window, showing many different types of clips arranged in the same sequence](../uploads/Main/timeline_cinematic_example.png)

###The Animation window

The [Animation window](AnimationEditorGuide.html) allows you to create individual [animation clips](animeditor-CreatingANewAnimationClip.html) as well as viewing [imported animation clips](AnimationsImport.html). Animation clips store animation for a single GameObject or a single hierarchy of GameObjects. The Animation window is useful for animating discrete items in your game such as a swinging pendulum, a sliding door, or a spinning coin. The animation window can only show one animation clip at a time.

The Animation window was added to Unity in version 4.0. The Animation window is an older feature than the Timeline window. It provides a simple way to create animation clips and animate individual GameObjects, and the clips you create in the Animation window can be combined and blended between using anim [Animator controller](Animator.html). However, to create more complex sequences involving many disparate GameObjects you should use the Timeline window (see above).

The animation window has a "timeline" as part of its user interface (the horiontal bar with time delineations marked out), however this is separate to the Timeline window.

To start learning about animation in Unity, visit the [Animation section](AnimationSection.html) of the user manual.

![The Animation window, shown in dopesheet mode, showing a hierarchy of objects (in this case, a robot arm with numerous moving parts) animated together in a single animation clip](../uploads/Main/AnimationEditorDopeSheetView.png)


 
