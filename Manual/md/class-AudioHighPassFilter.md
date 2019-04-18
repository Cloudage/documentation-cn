#音频高通滤波器 (Audio High Pass Filter)

__Audio High Pass Filter__ 传递音频源的高频率，并对频率低于__截止频率 (Cutoff Frequency)__ 的信号进行截止。


##属性

![](../uploads/Main/AudioHighPassFilter.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Cutoff Frequency__ |高通截止频率，单位为赫兹（范围从 10.0 至 22000.0，默认值为 5000.0）。|
|__Highpass Resonance Q__ |高通共振品质值（范围从 1.0 至 10.0，默认值为 1.0）。|


##详细信息

__Highpass resonance Q__（Highpass Resonance Quality Factor 的缩写，表示高通共振品质因数）决定了滤波器自共振的衰减程度。高通谐振品质越高表明能量损失速度越慢，即振荡消失得越慢。
