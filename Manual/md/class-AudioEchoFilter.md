#音频回声滤波器 (Audio Echo Filter)

__Audio Echo Filter__ 在给定的__延时 (Delay)__ 之后重复声音，并根据__衰减率 (Decay Ratio)__ 衰减重复的声音。


##属性

![](../uploads/Main/AudioEchoFilter.png) 

|**_属性：_** |**_功能：_** |
|:---|:---|
|__Delay__ |回声延时，以 ms 为单位。范围从 10 到 5000。默认值为 500。|
|__Decay Ratio__ |每次延时的回声衰减。范围从 0 到 1。1.0 表示无衰减，0.0 表示完全衰减（例如，简单的 1 行延时）。默认值为 0.5。|
|__Wet Mix__ |要传递到输出的回声信号量。范围从 0.0 到 1.0。默认值为 1.0。|
|__Dry Mix__ |要传递到输出的原始信号量。范围从 0.0 到 1.0。默认值为 1.0。|



##详细信息

__Wet Mix__ 值决定了滤波信号的幅度，__Dry Mix__ 决定了未过滤声音输出的幅度。

坚硬的表面会反射声音的传播。例如，使用音频回声滤波器 (Audio Echo Filter) 可使大峡谷效果更加逼真。

根据闪电和雷声可知声音的传播速度比光慢。要模拟此情况，请为事件声音应用 Audio Echo Filter，将 Wet Mix 设置为 0.0 并将 Delay 调制为[音频源](class-AudioSource.html)到[音频监听器](class-AudioListener.html)之间的距离。
