Editing Curves
==============

There are several different features and windows in the Unity Editor which use __Curves__ to display and edit data. The methods you can use to view and manipulate curves is largely the same across all these areas, although there are some exceptions.


* The [Animation View](AnimationEditorGuide.html) uses curves to display and edit the values of animated properties over time in an __Animation Clip__.

![The Animation View.](../uploads/Main/AnimationEditorCurve.png) 


* Script components can have member variables of type [AnimationCurve](EditingValueProperties.html) that can be used for all kinds of things. Clicking on those in the Inspector will open up the __Curve Editor__.

![The Curve Editor.](../uploads/Main/CurveEditorPopup.png) 


* The [Audio Source](class-AudioSource.html) component uses curves to control roll-off and other properties as a function of distance to the Audio Source.

![Distance function curves in the AudioSource component in the Inspector.](../uploads/Main/AudioSourceCurve.png) 


While these controls have subtle differences, the __curves__ can be edited in exactly the same way in all of them. This page explains how to navigate and edit curves in those controls. 

Adding and Moving Keys on a Curve
---------------------------------


A key can be added to a curve by double-clicking on the curve at the point where the __key__ should be placed. It is also possible to add a __key__ by right-clicking on a curve and select __Add Key__ from the context menu.

Once placed, __keys__ can be dragged around with the mouse:


* Click on a __key__ to select it. Drag the selected __key__ with the mouse.
* To snap the __key__ to the grid while dragging it around, hold down __Command__ on Mac / __Control__ on Windows while dragging.

It is also possible to select multiple __keys__ at once:


* To select multiple __keys__ at once, hold down __Shift__ while clicking the keys.
* To deselect a selected __key__, click on it again while holding down __Shift__.
* To select all __keys__ within a rectangular area, click on an empty spot and drag to form the rectangle selection.
* The rectangle selection can also be added to existing selected keys by holding down __Shift__.

__Keys__ can be deleted by selecting them and pressing __Delete__, or by right-clicking on them and selecting __Delete Key__ from the context menu.


Editing Keys
------------

Direct editing of key values in curve editors is a new feature in Unity 5.1. Use __Enter/Return__ or context menu to start editing selected keys, __Tab__ to switch between fields, __Enter/Return__ to commit, and __Escape__ to cancel editing.

Navigating the Curve View
-------------------------


When working with the __Animation View__ you can easily zoom in on details of the curves you want to work with or zoom out to get the full picture.

You can always press __F__ to frame-select the shown curves or selected keys in their entirely.

###Zooming

You can __zoom__ the Curve View using the scroll-wheel of your mouse, the zoom functionality of your trackpad, or by holding __Alt__ while right-dragging with your mouse. 

You can zoom on only the horizontal or vertical axis:


* __zoom__ while holding down __Command__ on Mac / __Control__ on Windows to zoom horizontally.
* __zoom__ while holding down __Shift__ to zoom vertically.

Furthermore, you can drag the end caps of the scrollbars to shrink or expand the area shown in the Curve View.

###Panning

You can __pan__ the Curve View by middle-dragging with your mouse or by holding __Alt__ while left-dragging with your mouse.

Editing Tangents
----------------
A key has two __tangents__ - one on the left for the incoming slope and one on the right for the outgoing slope. The tangents control the shape of the curve between the keys. You can select from a number of different tangent types to control how your curve leaves one key and arrives at the next key. Right-click a key to select the tangent type for that key.


![](../uploads/Main/AnimationCurveTangentMenu.png) 

For animated values to change smoothly when passing a key, the left and right tangent must be co-linear. The following tangent types ensure smoothness:

* __Clamped Auto__: This is the default tangent mode. The tangents are automatically set to make the curve pass smoothly through the key. When editing the key's position or time, the tangent adjusts to prevent the curve from "overshooting" the target value. If you manually adjust the tangents of a key in Clamped Auto mode, it is switched to __Free Smooth__ mode. In the example below, the tangent automatically goes into a slope and levels out while the key is being moved:

![](../uploads/Main/AnimationClampedAutoTangents.gif) 

* __Auto__: This is a [Legacy tangent mode](UpgradeGuide55.html), and remains an option to be backward compatible with older projects. Unless you have a specific reason to use this mode, use the default __Clamped Auto__. When a key is set to this mode, the tangents are automatically set to make the curve pass smoothly through the key. However, there are two differences compared with __Clamped Auto__ mode:
    1. The tangents do not adjust automatically when the key's position or time is edited; they only adjust when initially setting the key to this mode.
    2. When Unity calculates the tangent, it does not take into account avoiding "overshoot" of the target value of the key.

![](../uploads/Main/AnimationAuto.png) 

* __Free Smooth__: Drag the tangent handles to freely set the tangents. They are locked to be co-linear to ensure smoothness.

![](../uploads/Main/AnimationFreeSmooth.png) 

* __Flat__: The tangents are set to be horizontal (this is a special case of __Free Smooth__).

![](../uploads/Main/AnimationFlat.png) 

Sometimes you might not want the curve to be smooth when passing through a key. To create sharp changes in the curve, choose one of the __Broken__ tangent modes.

![](../uploads/Main/AnimationCurveTangentTypes.png) 

When using broken tangents, the left and right tangent can be set individually. Each of the left and right tangents can be set to one of the following types:

* __Broken - Free__: Drag the tangent handle to freely set the tangents.

![](../uploads/Main/AnimationBrokenFree.png) 

* __Broken - Linear__: The tangent points towards the neighboring key. To make a linear curve segment, set the tangents at both ends to be __Linear__. In the example below, all three keys have been set to __Broken - Linear__, to achieve straight lines from key to key.

![](../uploads/Main/AnimationBrokenLinear.png) 

* __Broken - Constant__: The curve retains a constant value between two keys. The value of the left key determines the value of the curve segment.

![](../uploads/Main/AnimationBrokenConstant.png) 
