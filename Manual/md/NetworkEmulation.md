网络仿真
=================


作为 Unity 网络功能集的一部分，可以选择模拟较慢的互联网连接速度，从而测试低带宽地区用户的游戏体验。

要启用网络仿真 (Network Emulation)，请选择 __Edit &gt; Network Emulation__，然后选择所需的连接速度仿真。


![启用 Network Emulation](../uploads/Main/NetworkEmulationMenu.jpg)

技术细节
-----------------


网络仿真会延迟 Network 和 NetworkView 类的网络流量中的数据包发送。ping 将针对所有选项进行人为膨胀，随着模拟连接速度变慢，通胀值会增加。在 __Dial-Up__ 设置中，还引入了数据包丢弃和偏差来模拟可能最糟糕的连接。无论您是作为服务器还是客户端，仿真都将持续存在。

网络仿真仅影响 Network 和 NetworkView 类，不会改变或模拟用 .NET 套接字编写的专用网络代码。
