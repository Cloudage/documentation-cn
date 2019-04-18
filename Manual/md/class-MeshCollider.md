#Mesh Collider

The __Mesh Collider__ takes a [Mesh Asset](class-Mesh.html) and builds its Collider based on that Mesh. It is far more accurate for collision detection than using primitives for complicated Meshes. Mesh Colliders that are marked as __Convex__ can collide with other Mesh Colliders.


![](../uploads/Main/Inspector-MeshCollider.png) 


##属性

|**_Property_** ||**_Function_** |
|:---|:---|
|__Convex__ ||Tick the checkbox to enable __Convex__. If enabled, this Mesh Collider collides with other Mesh Colliders. __Convex__ Mesh Colliders are limited to 255 triangles. |
|__Is Trigger__ ||If enabled, Unity uses this Collider for triggering events, and the physics engine ignores it. |
|__Cooking Options__ ||Enable or disable the [Mesh cooking](#cooking) options that affect how the physics engine processes Meshes.|
||__None__|Disable all of the __Cooking Options__ listed below.|
||__Everything__|Enable all of the __Cooking Options__ listed below.|
||__Inflate Convex Mesh__|Allow the physics engine to increase the volume of the input Mesh, to generate a valid convex mesh.|
||__Cook for Faster Simulation__|Make the physics engine cook Meshes for faster simulation. When enabled, this runs some extra steps to guarantee the resulting Mesh is optimal for run-time performance. This affects the performance of the physics queries and contacts generation. When this setting is disabled, the physics engine uses a faster cooking time instead, and produces results as fast as possible. Consequently, the cooked Mesh Collider might not be optimal.|
||__Enable Mesh Cleaning__|Make the physics engine clean Meshes. When enabled, the cooking process tries to eliminate [degenerate triangles](https://en.wikipedia.org/wiki/Degeneracy_(mathematics)#Triangle) of the Mesh, as well as other geometrical artifacts. This results in a Mesh that is better suited for use in collision detection and tends to produce more accurate hit points.|
||__Weld Colocated Vertices__|Make the physics engine remove equal vertices in the Meshes. When enabled, the physics engine combines the vertices that have the same position. This is important for the collision feedback that happens at run time.|
|__Material__ ||Reference to the [Physics Material](class-PhysicMaterial.html) that determines how this Collider interacts with others. |
|__Mesh__ ||Reference to the Mesh to use for collisions. |


##详细信息

The Mesh Collider builds its collision representation from the [Mesh](class-Mesh.html) attached to the GameObject, and reads the properties of the attached [Transform](class-Transform.html) to set its position and scale correctly. The benefit of this is that you can make the shape of the Collider exactly the same as the shape of the visible Mesh for the GameObject, resulting in more precise and authentic collisions. However, this precision comes with a higher processing overhead than collisions involving primitive colliders (such as Sphere, Box, and Capsule) and so it is best to use Mesh Colliders sparingly.

Faces in collision meshes are one-sided. This means objects can pass through them from one direction, but collide with them from the other.

<a name="cooking"></a>
## Mesh cooking

Mesh cooking changes a normal Mesh into a Mesh that you can use in the physics engine. Cooking builds the spatial search structures for the physics queries, such as [Physics.Raycast](../ScriptReference/Physics.Raycast.html), as well as supporting structures for the contacts generation. Unity cooks all Meshes before using them in collision detection. This can happen at import time (__Import Settings > Model > Generate Colliders__) or at run time.

When generating Meshes at run time (for example, for procedural surfaces), it's useful to set the __Cooking Options__ to produce results faster, and disable the additional data cleaning steps of cleaning. The downside is that you need to generate no degenerate triangles and no co-located vertices, but the cooking works faster. 

If you disable __Enable Mesh Cleaning__ or __Weld Colocated Vertices__, you need to ensure you aren't using data that those algorithms would otherwise filter. Make sure you don't have any co-located vertices if __Weld Colocated Vertices__ is disabled, and when __Enable Mesh Cleaning__ is enabled, make sure there are no tiny triangles whose area is close to zero, no thin triangles, and no huge triangles whose area is close to infinity.

**Note**: Setting __Cooking Options__ to any other value than the default settings means the Mesh Collider must use a Mesh that has an [isReadable](../ScriptReference/Mesh-isReadable.html) value of true.

## 限制

There are some limitations when using the Mesh Collider:

* Mesh Colliders that do not have __Convex__ enabled are only supported on GameObjects without a Rigidbody component. To apply a Mesh Collider to a Rigidbody component, tick the __Convex__ checkbox.
* In certain cases, for a Mesh Collider to work properly, you need to enable the __Read/Write Enabled__ setting on the [Model tab](FBXImporter-Model.html) of the Mesh's [Model Import Settings](class-FBXImporter.html) window. These cases include:
    * Negative scaling (for example, (-1, 1, 1)).
    * Shear transform (for example, when a rotated Mesh has a scaled parent transform).

__Optimization tip:__ If a Mesh is used only by a Mesh Collider, you can disable __Normals__ in __Import Settings__, because the physics system doesn’t need them.

---
* <span class="page-edit">2018-06-07 Page amended with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Mesh Cooking Options added in [2017.3](https://docs.unity3d.com/2017.3/Documentation/Manual/30_search.html?q=newin20173) <span class="search-words">NewIn20173</span></span>

* <span class="page-history">Updated functionality in 2018.1</span>

