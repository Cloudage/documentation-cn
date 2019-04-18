#已知问题

本页面包含为通用 Windows 平台构建的应用程序的已知问题，这些问题由与 Unity 无关的外部因素（如驱动程序和库）引起。

|**设置**|**问题**|**原因**|**变通方案**|
|:---|:---|:---|:---|
| Nokia Lumia 630/635/1520| 着色器中的特定采样器排序导致纹理包裹模式更改为钳位模式。此问题也可能在其他 3xx Adreno 设备上重现。 | Adreno 驱动程序错误。| 在着色器代码文件中更改受影响纹理的采样器寄存器（例如，将 `sampler2D _MainTex;` 更改为 `sampler2D _MainTex:register(s0);`）。 |
|UWP| 锁定光标的情况下，滚动鼠标滚轮可能会导致鼠标移动（如果 [Cursor.lockState](../ScriptReference/Cursor-lockState.html) `!= None`）。 | Windows 操作系统问题。| 打开 __Player Settings__ (__Edit__ > __Project Settings__ > __Player__)。选择 __Publishing Settings__ 选项卡，并勾选 __Independent Input Source__ 复选框。 |
|UWP .Net Native| `Terrain.GetHeights`、`Terrain.SetHeights` 和 `Terrain.SetHeightsDelayLOD` 函数导致播放器崩溃。| 使用多维数组调用 `GCHandle.Alloc` 时的 .Net Native 错误。| 当前无变通方案。|

---
<span class="page-edit">• 2017-05-16  Page amended with no [editorial review](DocumentationEditorialReview.html)
</span><br/>
