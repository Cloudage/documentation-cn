#音频低通滤波器 (Audio Low Pass Filter)

__Audio Low Pass Filter__ 传递[音频源](class-AudioSource.html)的低频或[音频监听器](class-AudioListener.html)接收的音源低频，并且移除比__截止频率 (Cutoff Frequency)__ 高的频率。


##属性

![](../uploads/Main/AudioLowPassFilter.png) 


|**_属性：_** |**_功能：_** |
|:---|:---|
|__Cutoff Frequency__ |低通滤波器的频率范围（ 0.0 HZ至 22000.0HZ，默认值为 5000.0HZ）。|
|__Lowpass Resonance Q__ |低通共振品质值（范围从 1.0 至 10.0，默认值为 1.0）。|


##详细信息

__Lowpass Resonance Q__（Lowpass Resonance Quality Factor 的缩写，表示低通滤波器共振效果因子）决定了滤波器自共振的衰减程度。低通谐振品质越高表明能量损失速度越慢，即振荡消失得越慢。

__Audio Low Pass Filter__ 带有可视化的曲线，可通过调节音频源和音频监听器之间的距离来设置 __Cutoff Frequency__。

音源的传播受环境影响例如，完成可视化的雾效，给音频监听器添加细微的低通效果。门外产生的高频率音效会被门过滤掉，不会传递到监听器。为了模拟这种效果，在开门时修改 Cutoff Frequency 值即可。
