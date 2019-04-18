# Batch mode and built-in coroutine compatibility

This page describes the supported features when running the Unity Editor and Standalone Player in batch mode. 

When running Unity, the following built-in [coroutine](Coroutines.html) operators add functionality:

* [AsyncOperation](../ScriptReference/AsyncOperation.html)

* [WaitForEndOfFrame](../ScriptReference/WaitForEndOfFrame.html)

* [WaitForFixedUpdate](../ScriptReference/WaitForFixedUpdate.html)

* [WaitForSeconds](../ScriptReference/WaitForSeconds.html) 

* [WaitForSecondsRealtime](../ScriptReference/WaitForSecondsRealtime.html)

* [WaitUntil](../ScriptReference/WaitUntil.html)

* [WaitWhile](../ScriptReference/WaitWhile.html)

The following table shows which of these operators Unity supports inside the Editor and Standalone Player, and when running each of them in batch mode using the [-batchmode](CommandLineArguments.html#batchmode) command line argument:

| | Editor | Editor -batchmode | Unity Standalone Player | Unity Standalone Player -batchmode |
|:---|:---|:---|:---|:---| 
| `AsyncOperation`| 是 | 是 | 是 | 是 |
| `WaitForEndOfFrame`| 是 | No* | 是 | 是 |
| `WaitForFixedUpdate`| 是 | 是 | 是 | 是 |
| `WaitForSeconds`| 是 | 是 | 是 | 是 |
| `WaitForSecondsRealtime`| 是 | 是 | 是 | 是 |
| `WaitUntil`| 是 | 是 | 是 | 是 |
| `WaitWhile`| 是 | 是 | 是 | 是 |


&#42; You cannot use `WaitForEndOfFrame` when running the Editor with `-batchmode`, because systems like animation, physics and timeline might not work correctly in the Editor. This is because Unity does not currently update these systems when using `WaitForEndOfFrame`. 

## Running coroutines

### Inside the Editor

In the Editor, press the "Play" button to run your code with coroutines.

### Editor in batch mode

To run coroutines when launching the Editor in batch mode from the command line, enter:

C:\Program Files\Unity\Editor\Unity.exe __-projectPath__ PROJECT_PATH __-batchMode__

### Inside the Standalone Player

Launch your Standalone Player to run your code. The Player loads and then waits for your coroutines to complete.

### Standalone Player in batch mode

To run coroutines when launching the Player in batch mode from the command line, enter:

PATH_TO_STANDALONE_BUILD __-projectPath__ PROJECT_PATH __-batchMode__

For example, on Windows:

C:\projects\myproject\builds\myproject.exe __-batchMode __

On Mac:

~/UnityProjects/myproject/builds/myproject __-batchMode__

## Example coroutine scripts

### AsyncOperation

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_AsyncTests());
    }

    public IEnumerator Example_AsyncTests()
    {
        Debug.Log("Start of AsyncLoad Example");
        
        var load = UnityEngine.Resources.LoadAsync("");
        yield return load;
        yield return null;
        
        Debug.Log("End of AsyncLoad Example");
    }
}
```

### WaitForEndOfFrame

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitForEndOfFrame_Coroutine());
    }

    public IEnumerator Example_WaitForEndOfFrame_Coroutine()
    {
        Debug.Log("Start of WaitForEndOfFrame Example");
        
        yield return new WaitForEndOfFrame();
        
        Debug.Log("End of WaitForEndOfFrame Example");
    }
}
```


### WaitForFixedUpdate

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitForFixedUpdate_Coroutine());
    }

    public IEnumerator Example_WaitForFixedUpdate_Coroutine()
    {
        Debug.Log("Start of WaitForFixedUpdate Example");
        
        yield return new WaitForFixedUpdate();
        
        Debug.Log("End of WaitForFixedUpdate Example");
    }
}
```


### WaitForSeconds

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitForSeconds_Coroutine());
    }

    public IEnumerator Example_WaitForSeconds_Coroutine()
    {
        Debug.Log("Start of WaitForSeconds Example");
        
        yield return new WaitForSeconds(1.5f);
        
        Debug.Log("End of WaitForSeconds Example");
    }
}
```


### WaitForSecondsRealtime

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitForSecondsRealtime_Coroutine());
    }

    public IEnumerator Example_WaitForSecondsRealtime_Coroutine()
    {
        Debug.Log("Start of WaitForSecondsRealtime Example");
        
        yield return new WaitForSecondsRealtime(1.5f);
        
        Debug.Log("End of WaitForSecondsRealtime Example");
    }
}
```


### WaitUntil

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitUntil_Coroutine());
    }

    public IEnumerator Example_WaitUntil_Coroutine()
    {
        Debug.Log("Start of WaitUntil Example");
        
        yield return new WaitUntil(() => Time.time > 5.0f);
        
        Debug.Log("End of WaitUntil Example");
    }
}
```

### WaitWhile

```
using System.Collections;
using UnityEngine;

[ExecuteInEditMode]
public class ExampleClass : MonoBehaviour
{
    public void Start()
    {
        StartCoroutine(Example_WaitWhile_Coroutine());
    }

    public IEnumerator Example_WaitWhile_Coroutine()
    {
        Debug.Log("Start of WaitWhile Example");
        
        yield return new WaitWhile(() => Time.time < 5.0f);
        
        Debug.Log("End of WaitWhile Example");
    }
}
```

---
* <span class="page-edit">2018-06-06 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">Added advice on using batchmode and coroutines in 2017.4</span>
