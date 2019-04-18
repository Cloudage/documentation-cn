# Mixed Reality Devices

Unity provides built-in support for a number of Mixed Reality (MR) devices.

Device availability is on a per-platform basis, meaning that not every device is available for every platform. Multiple devices are available on certain platforms, and you can choose which MR devices your app will support from the available list.

在运行时，任何给定时间只能有一个设备处于激活状态。但是，如果已选择在应用程序中支持多个设备，则可以在设备之间切换。

##MR device information

XRDevice.family is a string corresponding to the current XRSettings.loadedDeviceName. A family can have multiple models, which have different characteristics. The model can be accessed with XRDevice.model.
