#ShaderLab：旧版 Fog 命令

通过 Fog 命令控制 Fog 参数。

雾化效果根据与摄像机的距离将所生成像素的颜色向下混合为恒定颜色。雾化不会修改混合后像素的 Alpha 值，而只是修改其 RGB 分量。



##语法

###Fog
````
	Fog {Fog 命令}
````
在花括号中指定 Fog 命令。

###Mode
````
	Mode Off | Global | Linear | Exp | Exp2
````
定义雾模式。默认值是 Global，可转换为 Off 或 Exp2，具体取决于是否在渲染设置 (Render Settings) 中开启了雾效。

###Color
````
	Color ColorValue
````
设置雾效颜色。

###Density
````
	Density FloatValue
````
设置指数雾效的强度。

###Range
````
	Range FloatValue, FloatValue
````
设置线性雾效的远近范围。



##详细信息

默认雾效设置基于 [Lighting 窗口](GlobalIllumination.html)中的设置：雾模式为 __Exp2__ 还是 __Off__；强度和颜色也是获取自这些设置。

请注意，如果使用[片元程序](SL-ShaderPrograms.html)，则仍将应用着色器的雾效设置。如果平台上不存在固定函数雾效功能，Unity 将在运行时修补着色器，以支持请求的雾模式。
