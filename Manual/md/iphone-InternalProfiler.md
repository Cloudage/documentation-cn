Measuring performance with the built-in profiler
================================================


Unity iOS and Android contain a built-in profiler. The built-in profiler emits console messages from the game running on device. These messages are written every 30 seconds and will provide insight into how the game is running. Understanding what these messages mean is not always easy, but as a minimum, you should quickly be able to determine if your game is CPU or GPU bound, and if CPU bound whether it's script code, or perhaps Mono garbage collection that is slowing you down. See later in this page to learn how to configure the built-in profiler.

What the profiler tells you
---------------------------


Here's an example of the built-in profiler's output.



````
iPhone/iPad Unity internal profiler stats:
cpu-player> min: 9.8 max: 24.0 avg: 16.3
cpu-ogles-drv> min: 1.8 max: 8.2 avg: 4.3
cpu-waits-gpu> min: 0.8 max: 1.2 avg: 0.9
cpu-present> min: 1.2 max: 3.9 avg: 1.6
frametime> min: 31.9 max: 37.8 avg: 34.1
draw-call #> min: 4 max: 9 avg: 6 | batched: 10
tris #> min: 3590 max: 4561 avg: 3871 | batched: 3572
verts #> min: 1940 max: 2487 avg: 2104 | batched: 1900
player-detail> physx: 1.2 animation: 1.2 culling: 0.5 skinning: 0.0 batching: 0.2 render: 12.0 fixed-update-count: 1 .. 2
mono-scripts> update: 0.5 fixedUpdate: 0.0 coroutines: 0.0 
mono-memory> used heap: 233472 allocated heap: 548864 max number of collections: 1 collection total duration: 5.7
````
All times are measured in milliseconds per frame. You can see the minimum, maximum and average times over the last thirty frames.

General CPU activity
--------------------


|__Property__: |__Function__:|
|:---|:---|
|__cpu-player__ |Displays the time your game spends executing code inside the Unity engine and executing scripts on the CPU.|
|__cpu-ogles-drv__ |Displays the time spent executing OpenGL ES driver code on the CPU. Many factors like the number of draw calls, number of internal rendering state changes, the rendering pipeline setup and even the number of processed vertices can have an effect on the driver stats.|
|__cpu-waits-gpu__ |Displays the time the CPU is idle while waiting for the GPU to finish rendering. If this number exceeds 2-3 milliseconds then your application is most probably fillrate/GPU processing bound. If this value is too small then the profile skips displaying the value.|
|__msaa-resolve__ |The time taken to apply anti-aliasing.|
|__cpu-present__ |The amount of time spent executing the presentRenderbuffer command in OpenGL ES.|
|__frametime__ |Represents the overall time of a game frame. Note that iOS hardware is always locked at a 60Hz refresh rate, so you will always get multiples times of 16.7ms (1000ms/60Hz = 16.7ms).|

Rendering statistics
--------------------


|__Property__: |__Function__:|
|:---|:---|
|__tris #__ |Total number of triangles sent for rendering.|
|__verts #__ |Total number of vertices sent for rendering. You should keep this number below 10000 if you use only static geometry but if you have lots of skinned geometry then you should keep it much lower.|
|__batched__ |Number of draw-calls, triangles and vertices which were automatically batched by the engine. Comparing these numbers with draw-call and triangle totals will give you an idea how well is your scene prepared for batching. Share as many materials as possible among your objects to improve batching.|

Detailed Unity Player statistics
--------------------------------

The __player-detail__ section provides a detailed breakdown of what is happening inside the engine:

|__Property__: |__Function__:|
|:---|:---|
|__physx__ |Time spent on physics.|
|__animation__ |Time spent animating bones.|
|__culling__ |Time spent culling objects outside the camera frustum.|
|__skinning__ |Time spent applying animations to skinned meshes.|
|__batching__ |Time spent batching geometry. Batching dynamic geometry is considerably more expensive than batching static geometry.|
|__render__ |Time spent rendering visible objects.|
|__fixed-update-count__ |Minimum and maximum number of FixedUpdates executed during this frame. Too many FixedUpdates will deteriorate performance considerably.|

Detailed script statistics
---------------------------

The __mono-scripts__ section provides a detailed breakdown of the time spent executing code in the Mono runtime:

|__Property__: |__Function__:|
|:---|:---|
|__update__ |Total time spent executing all Update() functions in scripts.|
|__fixedUpdate__ |Total time spent executing all FixedUpdate() functions in scripts.|
|__coroutines__ |Time spent inside script coroutines.|

Detailed statistics on memory allocated by scripts
--------------------------------------------------

The __mono-memory__ section gives you an idea of how memory is being managed by the Mono garbage collector:

|__Property__: |__Function__:|
|:---|:---|
|__allocated heap__ |Total amount of memory available for allocations. A garbage collection will be triggered if there is not enough memory left in the heap for a given allocation. If there is still not enough free memory even after the collection then the allocated heap will grow in size.|
|__used heap__ |The portion of the __allocated heap__ which is currently used up by objects. Every time you create a new class instance (not a struct) this number will grow until the next garbage collection.|
|__max number of collections__ |Number of garbage collection passes during the last 30 frames.|
|__collection total duration__ |Total time (in milliseconds) of all garbage collection passes that have happened during the last 30 frames.|


Configuration
-------------

On iOS, it's disabled by default. To enable it, open the Unity-generated XCode project, select the `InternalProfiler.h` file, and change the line

	 #define ENABLE_INTERNAL_PROFILER 0

to

	 #define ENABLE_INTERNAL_PROFILER 1

Select __View &gt; Debug Area &gt; Activate Console__ in the XCode menu to display the output console (GDB) and then run your project. Unity will output statistics to the console window every thirty frames.

To enable it on Android, click the __Enable Internal Profiler__ (Deprecated) checkbox in PlayerSettings (__Edit__ > __Project Settings__ > __Player__). Make sure __Development Build__ is checked in the __Build Settings__ when building, and the statistics should show up in logcat when run on the device. To view logcat, you need __adb__ or the Android Debug Bridge. Once you have that, simply run the shell command __adb logcat__.

* <span class="page-edit">2018-06-14  Page amended with limited [editorial review](DocumentationEditorialReview.html)
</span>
