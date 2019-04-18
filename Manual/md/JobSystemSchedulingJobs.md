# Scheduling jobs

To schedule a job in the main thread, you must:

* Instantiate the job.
* Populate the job’s data.
* Call the [Schedule](../ScriptReference/Unity.Jobs.IJobExtensions.Schedule.html) method. 

Calling `Schedule` puts the job into the job queue for execution at the appropriate time. Once scheduled, you cannot interrupt a job.

**Note**: You can only call `Schedule` from the main thread.

## An example of scheduling a job

```
// Create a native array of a single float to store the result. This example waits for the job to complete for illustration purposes
NativeArray<float> result = new NativeArray<float>(1, Allocator.TempJob);

// Set up the job data
MyJob jobData = new MyJob();
jobData.a = 10;
jobData.b = 10;
jobData.result = result;

// Schedule the job
JobHandle handle = jobData.Schedule();

// Wait for the job to complete
handle.Complete();

// All copies of the NativeArray point to the same memory, you can access the result in "your" copy of the NativeArray
float aPlusB = result[0];

// Free the memory allocated by the result array
result.Dispose();
```

---

* <span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中公开了 C# 作业系统 <span class="search-words">NewIn20181</span></span>
