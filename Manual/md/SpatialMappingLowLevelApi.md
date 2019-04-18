# 空间映射低级 API

通过空间映射渲染器和碰撞体组件可以轻松使用空间映射的功能，而无需担心系统的小细节。如果希望更好地控制应用程序中的空间映射，请使用 Unity 为空间映射提供的低级 API。

此 API 提供了许多数据结构和对象类型，可用于访问 HoloLens 收集的空间映射信息。

## SurfaceObserver

可使用 [SurfaceObserver](../ScriptReference/XR.WSA.SurfaceObserver.html) 访问空间映射数据。`SurfaceObserver` 是一个 API 类，负责监视一个真实世界空间体积以提供应用程序所需的空间映射数据。`SurfaceObserver` 描述真实世界中的一个物理区域，并报告已由空间映射系统添加、更改或移除的与该区域相交的空间表面集合。

仅当希望通过 Unity scripting API 直接与空间映射进行交互时，才需要使用 `SurfaceObservers`。

Unity 提供了自己的空间映射渲染器和碰撞体组件，这些组件构建在 `SurfaceObserver` API 之上，可用于轻松访问空间映射功能。请参阅有关[空间映射组件](#SpatialMappingComponents)的文档以了解这些组件的更多详细信息。

通过使用 `SurfaceObserver`，应用程序可以在有或没有物理碰撞数据的情况下异步请求网格数据。请求完成后，另一个回调会通知应用程序这些数据已可供使用。

`SurfaceObserver` 为应用程序提供以下功能：

1.根据表面更改（例如添加、删除和更新）请求，发出[回调](https://en.wikipedia.org/wiki/Callback_(computer_programming))。

2.提供一个接口来请求与已知表面相对应的网格数据。

3.当请求的网格数据已可供使用时，发出[回调](https://en.wikipedia.org/wiki/Callback_(computer_programming))。

4.提供定义 `SurfaceObserver` 的位置和体积的方式。

## SurfaceData

[SurfaceData](../ScriptReference/XR.WSA.SurfaceData.html) 是一个类，其中包含空间映射系统构建和报告__表面__网格数据所需的所有信息。

必须使用 [RequestMeshAsync](../ScriptReference/XR.WSA.SurfaceObserver.RequestMeshAsync.html) 方法将填充的 `SurfaceData` 对象传递给空间映射系统。最初调用 [RequestMeshAsync](../ScriptReference/XR.WSA.SurfaceObserver.RequestMeshAsync.html) 方法时，需要将其传递给 `SurfaceDataReadyDelegate`。当网格数据准备就绪后，`SurfaceDataReadyDelegate` 会报告一个匹配的 `SurfaceData` 对象。

因此，应用程序可以精确确定数据对应的表面。

应该使用应用程序所需的信息填充 `SurfaceData` 游戏对象。此信息包括以下组件和数据：

* [WorldAnchor](../ScriptReference/XR.WSA.WorldAnchor.html) 组件

* [MeshFilter](class-MeshFilter.html) 组件

* [MeshCollider](class-MeshCollider.html) 组件（如果应用程序中需要物理数据）

* 所需生成的网格中每立方米的三角形

* 表面 ID

当使用配置有误的 [SurfaceData](../ScriptReference/XR.WSA.SurfaceData.html) 对象来调用 [RequestMeshAsync](../ScriptReference/XR.WSA.SurfaceObserver.RequestMeshAsync.html) 方法时，系统会抛出参数异常。即使 `RequestMeshAsync` 方法调用没有抛出参数异常，也没有其他办法可以检查空间映射是否正在创建并成功返回网格数据。建议通过脚本手动跟踪创建的网格数据。

## API 用法示例

下面的示例脚本显示了使用 API 重要部分的基本示例。

```

using UnityEngine;
using UnityEngine.XR;
using UnityEngine.XR.WSA;
using UnityEngine.Rendering;
using UnityEngine.Assertions;
using System;
using System.Collections;
using System.Collections.Generic;

public enum BakedState 
{
    NeverBaked = 0,
    Baked = 1,
    UpdatePostBake = 2
}

// 此类可保存由系统保留用于确定表面烘焙优先级的数据。
class SurfaceEntry 
{
    public GameObject  m_Surface; // 与此表面对应的游戏对象
    public int         m_Id; // 此表面的 ID
    public DateTime    m_UpdateTime; // 更新系统报告的时间
    public BakedState  m_BakedState;
    public const float c_Extents = 5.0f;
}

public class SMSample : MonoBehaviour 
{
    // 此观察者是了解空间映射世界的窗口。
    SurfaceObserver m_Observer;

    // 此字典包含一组已知的空间映射表面。
    //系统会定期更新、添加和删除表面。
    Dictionary<int, SurfaceEntry> m_Surfaces;

    //这是系统绘制烘焙表面时使用的材质。
    public Material m_drawMat;

    // 空间映射系统使用此标志在烘焙正在进行时推迟请求。
    // 烘焙网格数据可能需要经过多个帧。此示例基于表面数据表面
    // 确定烘焙请求顺序的优先级，并仅在
    // 系统当前未处理请求时才发出新请求。
    bool m_WaitingForBake;

    //这是系统最后一次更新 SurfaceObserver。更新间隔
    // 不超过两秒
    float m_lastUpdateTime;

    void Start () 
    {
        m_Observer = new SurfaceObserver ();
        m_Observer.SetVolumeAsAxisAlignedBox (new Vector3(0.0f, 0.0f, 0.0f), 
            new Vector3 (SurfaceEntry.c_Extents, SurfaceEntry.c_Extents, SurfaceEntry.c_Extents));
        m_Surfaces = new Dictionary<int, SurfaceEntry> ();
        m_WaitingForBake = false;
        m_lastUpdateTime = 0.0f;
    }
    
    void Update () 
    {
        // 避免在 SurfaceObserver 上过于频繁调用 Update。
        if (m_lastUpdateTime + 2.0f < Time.realtimeSinceStartup) 
        {
            // 此代码块使观察体跟随摄像机。
            Vector3 extents;
            extents.x = SurfaceEntry.c_Extents;
            extents.y = SurfaceEntry.c_Extents;
            extents.z = SurfaceEntry.c_Extents;
            m_Observer.SetVolumeAsAxisAlignedBox (Camera.main.transform.position, extents);

            try 
            {
                m_Observer.Update (SurfaceChangedHandler);
            } 
            catch 
            {
                // 如果指定的回调错误，Update 可抛出异常。
                Debug.Log ("Observer update failed unexpectedly!");
            }

            m_lastUpdateTime = Time.realtimeSinceStartup;
        }
        if (!m_WaitingForBake) 
        {
            // 设定从高到低的优先级为旧添加、其他添加和更新。
            SurfaceEntry bestSurface = null;
            foreach (KeyValuePair<int, SurfaceEntry> surface in m_Surfaces) 
            {
                if (surface.Value.m_BakedState != BakedState.Baked) 
                {
                    if (bestSurface == null) 
                    {
                        bestSurface = surface.Value;
                    } 
                    else 
                    {
                        if (surface.Value.m_BakedState < bestSurface.m_BakedState) 
                        {
                            bestSurface = surface.Value;
                        } 
                        else if (surface.Value.m_UpdateTime < bestSurface.m_UpdateTime) 
                        {
                            bestSurface = surface.Value;
                        }
                    }
                }
            }
            if (bestSurface != null) 
            {
                // 填写并分发请求。
                SurfaceData sd;
                sd.id.handle = bestSurface.m_Id;
                sd.outputMesh = bestSurface.m_Surface.GetComponent<MeshFilter> ();
                sd.outputAnchor = bestSurface.m_Surface.GetComponent<WorldAnchor> ();
                sd.outputCollider = bestSurface.m_Surface.GetComponent<MeshCollider> ();
                sd.trianglesPerCubicMeter = 300.0f;
                sd.bakeCollider = true;
                try 
                {
                    if (m_Observer.RequestMeshAsync(sd, SurfaceDataReadyHandler)) 
                    {
                        m_WaitingForBake = true;
                    } 
                    else 
                    {
                        // 请求网格时返回值 false
                        // 通常表示指定的
                        // 表面 ID 无效。
                        Debug.Log(System.String.Format ("Bake request for {0} failed.Is {0} a valid Surface ID?", bestSurface.m_Id));
                    }
                }
                catch 
                {
                    // 如果没有正确填写数据结构，请求可能会失败
                    Debug.Log (System.String.Format("Bake for id {0} failed unexpectedly!", bestSurface.m_Id));
                }
            }
        }
    }

    // 此处理程序在表面更改时接收事件，并使用 SurfaceObserver
    // 的 Update 方法传播这些事件
    void SurfaceChangedHandler (SurfaceId id, SurfaceChange changeType, Bounds bounds, DateTime updateTime) 
    {
        SurfaceEntry entry;
        switch (changeType) 
        {
            case SurfaceChange.Added:
            case SurfaceChange.Updated:
            if (m_Surfaces.TryGetValue(id.handle, out entry)) 
            {
                // 如果系统已烘焙此表面，应将其标记为
                // 在更新时间外也需要烘焙，因此“下一个要烘焙的表面”
                // 逻辑会对其进行正确排序。
                if (entry.m_BakedState == BakedState.Baked) 
                {
                    entry.m_BakedState = BakedState.UpdatePostBake;
                    entry.m_UpdateTime = updateTime;
                }
            } 
            else 
            {
                // 这是一个全新的表面，所以为其创建一个条目。
                entry = new SurfaceEntry ();
                entry.m_BakedState = BakedState.NeverBaked;
                entry.m_UpdateTime = updateTime;
                entry.m_Id = id.handle;
                entry.m_Surface = new GameObject (System.String.Format("Surface-{0}", id.handle));
                entry.m_Surface.AddComponent<MeshFilter> ();
                entry.m_Surface.AddComponent<MeshCollider> ();
                MeshRenderer mr = entry.m_Surface.AddComponent<MeshRenderer> ();
                mr.shadowCastingMode = ShadowCastingMode.Off;
                mr.receiveShadows = false;
                entry.m_Surface.AddComponent<WorldAnchor> ();
                entry.m_Surface.GetComponent<MeshRenderer> ().sharedMaterial = m_drawMat;
                m_Surfaces[id.handle] = entry;
            }
            break;

            case SurfaceChange.Removed:
            if (m_Surfaces.TryGetValue(id.handle, out entry)) 
            {
                m_Surfaces.Remove (id.handle);
                Mesh mesh = entry.m_Surface.GetComponent<MeshFilter> ().mesh;
                if (mesh) 
                {
                    Destroy (mesh);
                }
                Destroy (entry.m_Surface);
            }
            break;
        }
    }

    void SurfaceDataReadyHandler(SurfaceData sd, bool outputWritten, float elapsedBakeTimeSeconds) 
    {
        m_WaitingForBake = false;
        SurfaceEntry entry;
        if (m_Surfaces.TryGetValue(sd.id.handle, out entry)) 
        {
            // 这两个断言检查返回的过滤器和 WorldAnchor
            // 是否与系统用于请求数据的对应项相同。除非更改了代码
            // 来替换或销毁它们，否则这应该始终为 true。
            Assert.IsTrue (sd.outputMesh == entry.m_Surface.GetComponent<MeshFilter>());
            Assert.IsTrue (sd.outputAnchor == entry.m_Surface.GetComponent<WorldAnchor>());
            entry.m_BakedState = BakedState.Baked;
        } 
        else 
        {
            Debug.Log (System.String.Format("Paranoia:  Couldn't find surface {0} after a bake!", sd.id.handle));
            Assert.IsTrue (false);
        }
    }
}
```

__注意：__调用 [SurfaceObserver 的 Update](../ScriptReference/XR.WSA.SurfaceObserver.Update.html) 方法可能是资源密集型操作，因此调用次数应尽量不要超出应用程序的要求。对于大多数应用程序来说，每三秒调用一次此方法应该就足够了。

--

* <span class="page-edit">2018-05-01 Page published with [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">在 2017.3 版中更新了 Hololens 空间映射文档</span>
