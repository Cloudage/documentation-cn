# 通用 Windows 平台：C# 脚本中的 WinRT API

可在 Unity 脚本中直接使用 WinRT API。但是，这种情况存在限制和要求：

* 必须用 C# 编写脚本，不能使用 UnityScript 的 WinRT API
* 必须使用 Microsoft 的编译器来编译脚本，不能使用 Mono。因此，必须将 Compilation Overrides 设置为 **Use .NET Core** 或 **Use .NET Core Partially**，在后一种情况中，脚本**不得**位于 **Plugins** 或 **Standard Assets** 文件夹下
* 由于 Unity Editor 也使用相同的脚本代码（但始终使用 Mono），所有使用 WinRT API 的代码都必须位于 **ENABLE_WINMD_SUPPORT** 定义之下

以下是直接使用 WinRT API 获取广告信息的示例：

```C#
using UnityEngine;
public class WinRTAPI : MonoBehaviour {
	void Update() {
		auto adId = GetAdvertisingId();
		// ...
	}

	string GetAdvertisingId() {
		#if ENABLE_WINMD_SUPPORT
			return Windows.System.UserProfile.AdvertisingManager.AdvertisingId;
		#else
			return"";
		#endif
	}
}
```

注意：使用 IL2CPP 脚本后端时，仅在使用 .NET 4.6 兼容性配置文件的情况下才支持该功能。
