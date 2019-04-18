## 抗锯齿

本页的效果描述是指在后期处理堆栈中找到的默认效果。

__抗锯齿 (Anti-aliasing)__ 效果提供了一组算法来防止锯齿并为图形提供更平滑的外观。锯齿是线条出现锯齿状或“楼梯”外观的效果（如下面左图所示）。如果图形输出设备没有足够高的分辨率来显示直线，则会发生这种情况。

__抗锯齿__用中间色调将这些锯齿状线条包围起来，从而降低锯齿明显程度。虽然这种做法减轻了线条的锯齿状外观，但也会使它们变得更模糊。

![左侧的场景渲染时未进行抗锯齿处理。右侧的场景显示了采用时间抗锯齿 (Temporal Anti-aliasing) 算法的效果。](../uploads/Main/PostProcessing-Antialiasing-0.jpg)

抗锯齿算法根据图像而定的。不能正常支持传统的多重采样（在 Editor 的 [Quality settings](class-QualitySettings.html) 中启用）时，这种算法非常有用，例如：

* 使用[延迟渲染](RenderTech-DeferredShading.html)时

* 在 Unity 5.5 或更早版本中的前向渲染路径中使用 HDR 时

后期处理栈中提供的算法包括：

* 快速近似抗锯齿 (Fast Approximate Anti-aliasing, FXAA)

* 时间抗锯齿 (Temporal Anti-aliasing, TAA)

## 快速近似抗锯齿

FXAA 是成本最低的技术，建议将此技术用于移动平台和其他不支持运动矢量（TAA 技术需要此类矢量）的平台。但是，此方案包含多个质量预设，因此也适合作为速度较慢的桌面平台硬件和游戏主机硬件的后备解决方案。

![选择 FXAA 后的抗锯齿 (Anti-aliasing) 效果的 UI](../uploads/Main/PostProcessing-Antialiasing-1.png)

### 属性

| __属性：__| __功能：__ |
|:---|:---| 
| __Preset__| 要使用的质量预设。提供性能和边缘质量之间的折衷。 |

### 优化

* 降低质量设置

### 要求

* Shader Model 3

请参阅[图形硬件功能和仿真](GraphicsEmulation.html)页面，查看更多详细信息和兼容硬件列表。

## 时间抗锯齿

时间抗锯齿是一种更先进的抗锯齿技术，此方案中的帧在历史缓冲区中随时间累积，用于更有效地平滑边缘。此技术在运动中的边缘平滑效果方面要好得多，但需要运动矢量并且成本高于 FXAA。因此，建议用于桌面平台和游戏主机平台。

![选择 TAA 后的抗锯齿 (Anti-aliasing) 效果的 UI](../uploads/Main/PostProcessing-Antialiasing-2.png)

### 属性

| __属性：__| __功能：__ |
|:---|:---| 
| __Jitter - Spread__| 抖动样本的扩散范围直径（以纹理像素为单位）。较小的值会产生更清晰但更锯齿更明显的输出，而较大的值会产生更平滑但更模糊的输出。 |
| __Blending - Stationary__| 静止片元的混合系数。控制历史样本混合到具有最少有效运动的片元的最终颜色中的百分比。 |
| __Blending - Motion__| 移动片元的混合系数。控制历史样本混合到具有显著有效运动的片元的最终颜色中的百分比。 |
| __Sharpen__| TAA 会在高频区域引起轻微的细节损失。锐化 (Sharpening) 缓解了这一问题。 |



### 限制

* 在 VR 中不受支持

### 要求

* 运动矢量

* 深度纹理

* Shader Model 3

请参阅[图形硬件功能和仿真](GraphicsEmulation.html)页面，查看更多详细信息和兼容硬件列表。

---

* <span class="page-edit"> 2017-05-24  Page published with no [editorial review](DocumentationEditorialReview.html)
</span>

* <span class="page-history">5.6 中的新功能</span>
