# Creating jobs

To create a job in Unity you need to implement the [IJob](../ScriptReference/Unity.Jobs.IJob.html) interface. `IJob` allows you to schedule a single job that runs in parallel to any other jobs that are running. 

**Note**: A “job” is a collective term in Unity for any struct that implements the `IJob` interface.

To create a job, you need to:

* Create a struct that implements `IJob`.
* Add the member variables that the job uses (either [blittable types](https://en.wikipedia.org/wiki/Blittable_types) or [NativeContainer](JobSystemNativeContainer.html) types).
* Create a method in your struct called [Execute](../ScriptReference/Unity.Jobs.IJob.Execute.html) with the implementation of the job inside it.

When executing the job, the `Execute` method runs once on a single core.

**Note**: When designing your job, remember that they operate on copies of data, except in the case of `NativeContainer`. So, the only way to access data from a job in the main thread is by writing to a `NativeContainer`. 

## An example of a simple job definition 

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
```

---

* <span class="page-edit">2018-06-15  Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 [2018.1](https://docs.unity3d.com/2018.1/Documentation/Manual/30_search.html?q=newin20181) 版中公开了 C# 作业系统 <span class="search-words">NewIn20181</span></span>
