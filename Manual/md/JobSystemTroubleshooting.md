# C# Job System tips and troubleshooting

When using the Unity C# Job System, make sure you adhere to the following:

## Do not access static data from a job

Accessing static data from a job circumvents all safety systems. If you access the wrong data, you might crash Unity, often in unexpected ways. For example, accessing [MonoBehaviour](../ScriptReference/MonoBehaviour.html) can cause crashes on domain reloads. 

**Note**: Because of this risk, future versions of Unity will prevent global variable access from jobs using [static analysis](https://www.youtube.com/watch?v=VxeC7WFfg3Q). If you do access static data inside a job, you should expect your code to break in future versions of Unity.

## Flush scheduled batches

When you want your jobs to start executing, then you can flush the scheduled batch with [JobHandle.ScheduleBatchedJobs](../ScriptReference/Unity.Jobs.JobHandle.ScheduleBatchedJobs.html). Note that calling this method can negatively impact performance. Not flushing the batch delays the scheduling until the main thread waits for the result. In all other cases use [JobHandle.Complete](../ScriptReference/Unity.Jobs.JobHandle.Complete.html) to start the execution process.

**Note**: In the [Entity Component System](https://github.com/Unity-Technologies/EntityComponentSystemSamples) (ECS) the batch is implicitly flushed for you, so calling `JobHandle.ScheduleBatchedJobs` is not necessary.

##Don’t try to update NativeContainer contents

Due to the lack of [ref returns](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/ref-returns), it is not possible to directly change the content of a [NativeContainer](../ScriptReference/Unity.Collections.LowLevel.Unsafe.NativeContainerAttribute.html). For example, `nativeArray[0]++;` is the same as writing `var temp = nativeArray[0]; temp++;` which does not update the value in `nativeArray`.

Instead, you must copy the data from the index into a local temporary copy, modify that copy, and save it back, like so: 

```	
MyStruct temp = myNativeArray[i];
temp.memberVariable = 0;
myNativeArray[i] = temp;
```

## Call JobHandle.Complete to regain ownership

Tracing data ownership requires dependencies to complete before the main thread can use them again. It is not enough to check [JobHandle.IsCompleted](../ScriptReference/Unity.Jobs.JobHandle.IsCompleted.html). You must call the method `JobHandle.Complete` to regain ownership of the `NativeContainer` types to the main thread. Calling `Complete` also cleans up the state in the safety system. Not doing so introduces a memory leak. This process also applies if you schedule new jobs every frame that has a dependency on the previous frame's job.

## Use Schedule and Complete in the main thread

You can only call [Schedule](../ScriptReference/Unity.Jobs.IJobExtensions.Schedule.html) and `Complete` from the main thread. If one job depends on another, use `JobHandle` to manage dependencies rather than trying to schedule jobs within jobs.

## Use Schedule and Complete at the right time

Call `Schedule` on a job as soon as you have the data it needs, and don't call `Complete` on it until you need the results. It is good practice to schedule a job that you do not need to wait for when it is not competing with any other jobs that are running. For example, if you have a period between the end of one frame and the beginning of the next frame where no jobs are running, and a one frame latency is acceptable, you can schedule the job towards the end of a frame and use its results in the following frame. Alternatively, if your game is saturating that changeover period with other jobs, and there is a big under-utilized period somewhere else in the frame, it is more efficient to schedule your job there instead.

## Mark NativeContainer types as read-only

Remember that jobs have read and write access to `NativeContainer` types by default. Use the `[ReadOnly]` attribute when appropriate to improve performance.

## Check for data dependencies

In the Unity __Profiler__ window, the marker “WaitForJobGroup” on the main thread indicates that Unity is waiting for a job on a worker thread to complete. This marker could mean that you have introduced a data dependency somewhere that you should resolve. Look for `JobHandle.Complete` to track down where you have data dependencies that are forcing the main thread to wait.

## Debugging jobs
Jobs have a [Run](../ScriptReference/Unity.Jobs.IJobExtensions.Run.html) function that you can use in place of `Schedule` to immediately execute the job on the main thread. You can use this for debugging purposes.

## Do not allocate managed memory in jobs

Allocating managed memory in jobs is incredibly slow, and the job is not able to make use of the Unity [Burst compiler](https://www.youtube.com/watch?v=NF6kcNS6U80&t=2s) to improve performance. Burst is a new [LLVM](https://en.wikipedia.org/wiki/LLVM) based backend compiler technology that makes things easier for you. It takes C# jobs and produces highly-optimized machine code taking advantage of the particular capabilities of your platform.

## Further Information

* Watch the [Unity GDC 2018: C# Job System](https://www.youtube.com/playlist?list=PLX2vGYjWbI0RuXtGMYKqChoZC2b-H4tck) playlist of clips.
* For more advanced information on how the C# Job System relates to ECS, see the [ECS package documentation on GitHub](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/Documentation/index.md).

---

* <span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中公开了 C# 作业系统 <span class="search-words">NewIn20181</span></span>
