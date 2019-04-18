# JobHandle and dependencies

When you call the [Schedule](../ScriptReference/Unity.Jobs.IJobExtensions.Schedule.html) method of a job it returns a [JobHandle](../ScriptReference/Unity.Jobs.JobHandle.html). You can use a `JobHandle` in your code as a dependency for other jobs. If a job depends on the results of another job, you can pass the first job's `JobHandle` as a parameter to the second job's `Schedule` method, like so: 

```
JobHandle firstJobHandle = firstJob.Schedule();
secondJob.Schedule(firstJobHandle);
```

## Combining dependencies

If a job has many dependencies, you can use the method [JobHandle.CombineDependencies](../ScriptReference/Unity.Jobs.JobHandle.CombineDependencies.html) to merge them. `CombineDependencies` allows you to pass them onto the `Schedule` method.

```
NativeArray<JobHandle> handles = new NativeArray<JobHandle>(numJobs, Allocator.TempJob);

// Populate `handles` with `JobHandles` from multiple scheduled jobs...

JobHandle jh = JobHandle.CombineDependencies(handles);
```

## Waiting for jobs in the main thread

Use `JobHandle` to force your code to wait in the main thread for your job to finish executing. To do this, call the method [Complete](../ScriptReference/Unity.Jobs.JobHandle.Complete.html) on the `JobHandle`. At this point, you know the main thread can safely access the [NativeContainer](../ScriptReference/Unity.Collections.LowLevel.Unsafe.NativeContainerAttribute.html) that the job was using.

**Note**: Jobs do not start executing when you schedule them. If you are waiting for the job in the main thread, and you need access to the NativeContainer data that the job is using, you can call the method `JobHandle.Complete`. This method flushes the jobs from the memory cache and starts the process of execution. Calling `Complete` on a `JobHandle` returns ownership of that job's `NativeContainer` types to the main thread. You need to call `Complete` on a `JobHandle` to safely access those `NativeContainer` types from the main thread again. It is also possible to return ownership to the main thread by calling `Complete` on a `JobHandle` that is from a job dependency. For example, you can call `Complete` on `jobA`, or you can call `Complete` on `jobB` which depends on `jobA`. Both result in the `NativeContainer` types that are used by `jobA` being safe to access on the main thread after the call to `Complete`.

Otherwise, if you don’t need access to the data, you need to explicity flush the batch. To do this, call the static method [JobHandle.ScheduleBatchedJobs](../ScriptReference/Unity.Jobs.JobHandle.ScheduleBatchedJobs.html). Note that calling this method can negatively impact performance. 

## An example of multiple jobs and dependencies

**Job code**:

```
// Job adding two floating point values together
public struct MyJob : IJob
{
    public float a;
    public float b;
    public NativeArray<float> result;

    public void Execute()
    {
        result[0] = a + b;
    }
}

// Job adding one to a value
public struct AddOneJob : IJob
{
    public NativeArray<float> result;
    
    public void Execute()
    {
        result[0] = result[0] + 1;
    }
}
```

**Main thread code**:

```
// Create a native array of a single float to store the result in. This example waits for the job to complete
NativeArray<float> result = new NativeArray<float>(1, Allocator.TempJob);

// Setup the data for job #1
MyJob jobData = new MyJob();
jobData.a = 10;
jobData.b = 10;
jobData.result = result;

// Schedule job #1
JobHandle firstHandle = jobData.Schedule();

// Setup the data for job #2
AddOneJob incJobData = new AddOneJob();
incJobData.result = result;

// Schedule job #2
JobHandle secondHandle = incJobData.Schedule(firstHandle);

// Wait for job #2 to complete
secondHandle.Complete();

// All copies of the NativeArray point to the same memory, you can access the result in "your" copy of the NativeArray
float aPlusB = result[0];

// Free the memory allocated by the result array
result.Dispose();
```

---

* <span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中公开了 C# 作业系统 <span class="search-words">NewIn20181</span></span>
