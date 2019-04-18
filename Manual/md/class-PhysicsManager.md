# Physics Manager

Use the Physics Manager to apply global settings for 3D physics. To access it, go to __Edit__ > __Project Settings__ > __Physics__. To manage global settings for 2D physics, use the [Physics 2D Manager](class-Physics2DManager.html).

The settings of the PhysicsManager define limits on the accuracy of the physical simulation. Generally speaking, a more accurate simulation requires more processing overhead, so these settings offer a way to trade off accuracy against performance. See the [Physics](PhysicsSection.html) section of the manual for further information.

![The PhysicsManager, shown in the Inspector window.](../uploads/Main/PhysicsSet.png)

| **属性：** | **功能：** |
|:---|:---| 
| __Gravity__ | Use the x, y and z axes to set the amount of gravity applied to all Rigidbody components. For realistic gravity settings, apply a negative number to the y axis. Gravity is defined in world units per seconds squared. <br/>**Note**: If you increase the gravity, you might need to also increase the **Default Solver Iterations** value to maintain stable contacts. |
| __Default Material__ | Use this field to define the default Physics Material that is used if none has been assigned to an individual Collider. |
| __Bounce Threshold__ | Use this to set a velocity value. If two colliding objects have a relative velocity below this value, they do not bounce off each other. This value also reduces jitter, so it is not recommended to set it to a very low value. |
| __Sleep Threshold__ | Use this to set a global energy threshold, below which a non-kinematic Rigidbody (that is, one that is not controlled by the physics system) may go to sleep. When a Rigidbody is sleeping, it is not updated every frame, making it less resource-intensive. If a Rigidbody’s kinetic energy divided by its mass is below this threshold, it is a candidate for sleeping. |
| __Default Contact Offset__ | Use this to set the distance the collision detection system uses to generate collision contacts. The value must be positive, and if set too close to zero, it can cause jitter. This is set to 0.01 by default. Colliders only generate collision contacts if their distance is less than the sum of their contact offset values.  |
| __Default Solver Iterations__ | Solvers are small physics engine tasks which determine a number of physics interactions, such as the movements of joints or managing contact between overlapping Rigidbody components. Use **Default Solver Iterations** to define how many solver processes Unity runs on every physics frame. This affects the quality of the solver output and it’s advisable to change the property in case non-default `Time.fixedDeltaTime` is used, or the configuration is extra demanding. Typically, it’s used to reduce the jitter resulting from joints or contacts.  |
| __Default Solver Velocity Iterations__ | Use this to set how many velocity processes a solver performs in each physics frame. The more processes the solver performs, the higher the accuracy of the resulting exit velocity after a Rigidbody bounce. If you experience problems with jointed Rigidbody components or Ragdolls moving too much after collisions, try increasing this value. |
| __Queries Hit Backfaces__ | Tick the checkbox if you want physics queries (such as `Physics.Raycast`) to detect hits with the backface triangles of MeshColliders. This setting is unticked by default. |
| __Queries Hit Triggers__ | Tick the checkbox if you want physics hit tests (such as Raycasts, SphereCasts and SphereTests) to return a hit when they intersect with a Collider marked as a Trigger. Individual raycasts can override this behavior. This setting is ticked by default. |
| __Enable Adaptive Force__ | The adaptive force affects the way forces are transmitted through a pile or stack of objects, to give more realistic behaviour. Tick this box to enable the adaptive force. This setting is unticked by default. |
| __Enable PCM__ | Tick this checkbox to enable the persistent contacts manifold (PCM) contacts generation method of the physics engine. This means that fewer contacts are regenerated every physics frame, and more contact data is shared across frames. The PCM contacts generation path is also more accurate, and usually produces better collision feedback in most of the cases. See [Nvidia documentation on Persistent Contact Manifold](http://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/Manual/AdvancedCollisionDetection.html#persistent-contact-manifold-pcm) for more information. <br/>**Note**: Before Unity 5.5, Unity used a contacts generation method called SAT, based on the separating axis theorem (see [dyn4j.org’s guide to SAT](http://www.dyn4j.org/2010/01/sat/)). PCM is more efficient, but for older projects, you might find it easier to continue using SAT, to avoid needing to retweak physics slightly. PCM can result in a slightly different bounce, and fewer useless contacts end up in the contacts buffers (that is, the arrays you get in the Collision instance passed to `OnCollisionEnter`, `OnCollisionStay`, `OnCollisionExit`). |
| __Auto Simulation__ | Run the physics simulation automatically or allow explicit control over it|
| __Auto Sync Transforms__ | Automatically sync transform changes with the physics system whenever a Transform component changes. |
| __Layer Collision Matrix__ | Use this to define how the [layer-based collision](LayerBasedCollision.html) detection system behaves. |


<br/> 

---
* <span class="page-edit"> 2017-05-18  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Settings added in 5.5: Default Solver Iterations, Default Solver Velocity Iterations, Queries Hit Backfaces, Enable PCM</span>
