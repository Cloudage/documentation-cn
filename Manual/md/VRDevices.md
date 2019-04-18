VR 设备
==========
<!-- https://trello.com/c/Qw7imxOL -->
Unity 为许多虚拟现实设备提供内置支持。

设备可用性根据每个平台而定，这意味着并非每个设备都可用于每个平台。某些平台上可使用多个设备，您可以从可用列表中选择应用程序支持的 VR 设备。

在运行时，任何给定时间只能有一个设备处于激活状态。但是，如果已选择在应用程序中支持多个设备，则可以在设备之间切换。

##VR 设备信息

[XRDevice.family](../ScriptReference/XR.XRDevice-family.html) 是一个与当前 [XRSettings.loadedDeviceName](../ScriptReference/XR.XRSettings-loadedDeviceName.html) 对应的字符串。一个系列可以拥有多个具有不同特征的型号。例如，Oculus 有 DK2、Gear VR 等。可使用 [XRDevice.model](../ScriptReference/XR.XRDevice-model.html) 来访问型号。
