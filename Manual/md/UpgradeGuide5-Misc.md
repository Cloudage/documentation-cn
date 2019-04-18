#Unity 5.0 的其他升级注意事项

有关从 Unity 4 升级到 Unity 5 时已更改并可能影响项目的功能的更多信息

## 锁定/隐藏光标

光标锁定和光标可见性现在是彼此独立的。

````
// Unity 4.x
Screen.lockCursor = true;
````
 
````
// 在 Unity 5.0 变为如下形式
Cursor.visible = false;
Cursor.lockState = CursorLockMode.Locked;
````

---

## Linux

Unity 5 的游戏手柄处理方式已经过重新设计。

Unity 现在能够“配置”游戏手柄 - 通过已知模型的数据库或者使用 SDL_GAMECONTROLLERCONFIG 环境变量（由 Steam Big Picture/SteamOS 针对使用其界面检测或配置的游戏手柄进行设置）。

配置后的游戏手柄呈现一致的布局：左摇杆使用轴 0/1，右摇杆使用轴 3/4，游戏手柄面上的按钮为 0-3 等。要确定是否已配置游戏手柄，请调用 Input.IsJoystickPreconfigured()。

---

## Windows 应用商店应用程序

在大多数 API 中，“Metro”关键字已替换为“WSA”，例如：BuildTarget.MetroPlayer 变为 BuildTarget.WSAPlayer，PlayerSettings.Metro 变为 PlayerSettings.WSA。

脚本中诸如 UNITY_METRO、UNITY_METRO_8_0、UNITY_METRO_8_1 之类的定义仍然存在，但很快就会更新为 UNITY_WSA、UNITY_WSA_8_0、UNITY_WSA_8_1 定义。

---

##其他无法自动升级的脚本 API 更改

- UnityEngine.AnimationEvent 现在是一个结构。与“null”进行比较将导致编译错误。

- AddComponent(string) 用变量调用时不能自动更新为通用版本 AddComponent&lt;T&gt;()。在这种情况下，API Updater 将通过调用 APIUpdaterRuntimeServices.AddComponent() 来替换该调用。此方法旨在支持以 Editor 模式测试游戏（会尽最大努力尝试在运行时解析类型），而并非用于生产阶段，因此构建一个调用这种方法的游戏是错误的。在支持 Type.GetType(string) 的平台上，可尝试使用 GetComponent(Type.GetType(typeName)) 作为变通方法。
 

- AssetBundle.Load、AssetBundle.LoadAsync 和 AssetBundle.LoadAll 已弃用。请改用 AssetBundle.LoadAsset、AssetBundle.LoadAssetAsync 和 AssetBundle.LoadAllAssets。脚本更新程序无法更新它们，因为加载行为已稍微改变。在 5.0 中，所有加载 API 都不再加载组件，请先使用新的加载 API 加载游戏对象，然后在对象上查找组件。

---

##Unity 资源包更改

.unityPackages 的内部资源包格式已经改变，另外，将资源包导入 Unity 的方式以及冲突解决方式等方面的一些行为也已改变。

- 现在，构建的资源包中仅包含源资源以及 .meta 文本文件（其中含有该资源的所有导入器设置）。

- 资源包现在始终需要导入资源。

- 由于导入的数据（例如纹理和音频数据）不会翻倍，因此资源包的大小会显著减小。

- 文件名已在项目中存在但具有不同 GUID 的资源包将用唯一的文件名导入这些文件。这样做是为了防止覆盖实际来自不同资源包或由用户创建的项目文件。
