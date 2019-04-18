#Trees

Unity terrains can be furnished with _trees_. Patches of trees can be painted onto a terrain in much the same way that heightmaps and textures are painted but the trees are solid 3D objects that grow from the surface. Unity uses optimisations (eg, billboarding for distant trees) to maintain good rendering performance, so you can have dense forests with thousands of trees and still keep an acceptable framerate.

![Terrain with trees](../uploads/Main/TerrainWithTrees.jpg)

##Enabling trees

The tree button on the toolbar enables tree painting.

![](../uploads/Main/TerrainTreePaintTool.png)

Initially, the terrain will have no trees available but if you click the _Edit Trees_ button and select _Add Tree_ you will see a window to select a tree asset from your project.

![The Add Tree window](../uploads/Main/TerrainTreeAddWindow.png)

Unity comes with several sample SpeedTree tree objects in the Standard Assets packages for prototyping purposes __Assets &gt; Import Package &gt; Terrain Assets__) but you can also create suitable objects with SpeedTree Modeler app, Unity Tree Creator tools or other 3D modelling apps. 

If the selected tree object is created by the Tree Creator, the window will show Bend factor for adjusting the wind responsiveness. See the section on _Making trees bend in the wind_ below. 

##Painting trees

With a tree selected, you can paint onto the landscape in the same way you paint textures or heightmaps. You can remove trees from an area by holding the shift key while you paint and remove just the currently selected tree type by holding down the control key. The familiar _Brush Size_ option is available for tree painting but the _Opacity_ property is replaced by _Tree Density_, which controls the average number of trees painted into a given unit of area. 

There is a ranged slider for you to control the tree's minimal height and maximal height. If you disable Random, you can specify a value for all tree's height. By default a tree's width is locked to height so that trees are always scaled uniformly. However you can disable Lock Width to Height option and specify the width separately. 

There is also a control for _Color Variation_ and _Random Tree Rotation_. The variation options help to create the impression of a random, natural-looking forest rather than an artificial plantation of identical trees.

The _Mass Place Trees_ button is a very useful way to create an overall covering of trees without painting over the whole landscape. Following a mass placement, you can still use painting to add or remove trees to create denser or sparser areas.

##SpeedTree/LOD trees

From Unity 5, you can use SpeedTree Modeler from IDV, Inc. to create trees with advanced visual effects such as smooth LOD transition, fast billboarding and natural wind animation. Please refer to their [site](http://www.speedtree.com/unity/) for detailed information. You can also freely import SpeedTree assets into your project folder from asset store packages or other third party sources. 

See [here](SpeedTree.html) for more information.

##Tree Creator trees

Unity has its own [Tree creator](class-Tree.html) that you can use to produce new tree assets but you can also use a standard 3D modelling app for the task. The tree mesh should have fewer than 2000 triangles (for performance reasons) and the anchor point should be right at the base of the tree where it emerges from the ground. The mesh should always have exactly two materials, one for the tree body and the other for the leaves.

Trees must use the __Nature/Soft Occlusion Leaves__ and __Nature/Soft Occlusion Bark__ shader. In order to use those shaders you also have to place the tree in a special folder that contains the name “Ambient-Occlusion”. When you place a model in such a folder and reimport it, Unity will calculate soft ambient occlusion in a way that is specifically designed for trees. The “Nature/Soft Occlusion” shaders rely on the folder naming convention and the tree won't render correctly if you don't follow it.

When you save a tree asset from the modelling app, you will need to click the _Refresh_ button (shown in the inspector when the tree painting tool is selected) in order to see the updated trees on your terrain.

##Using colliders with trees

You can add a Capsule Collider to a new tree asset by instantiating it in the scene, adding the collider (menu: __Component &gt; Physics &gt; Capsule Collider__) and creating a new prefab for the modified tree object. When you add the tree to the terrain for painting, make sure you select the prefab with the collider rather than the original object. You should also enable _Create Tree Colliders_ in the terrain's Terrain Collider component inspector.

##Making trees bend in the wind

The first thing you will need to do is create a wind zone. You can do this by selecting from the menu: __Game Object &gt; 3D Object &gt; Wind Zone__.

At this point, you will need to make sure that your trees are set to bend. Select your __Terrain__, click the __Place trees__ button in the __Inspector__ and then select __Edit Trees__ &gt; __Edit Tree__. Setting the __Bend Factor__ to 1 will cause the trees to adjust if you have not already done this.

You may notice that your trees are moving about quite violently. To fix this, you can change your bend value, but it is probably easier to set the values on the __Wind Zone__ directly, keeping your tree bend factor set to 1. To keep the trees from fluttering around too much, adjust the wind turbulence down to around 0.1 to 0.3 and everything will become much smoother. If you don't want the trees blowing all the way to one side, set the Wind Main value down to the same value as your turbulence.

---

* <span class="page-edit">2017-09-19  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 Unity 4.6 中更改了 GameObject 菜单</span>
