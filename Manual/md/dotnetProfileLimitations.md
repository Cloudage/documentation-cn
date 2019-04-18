# 稳定脚本运行时：已知限制

Unity 支持新版的 .NET 运行时。使用 .NET 运行时的时候，可能会遇到以下问题：

## 代码大小

稳定脚本运行时随附比旧版脚本运行时更大的 .NET 类库 API。这意味着代码大小通常更大。这种大小增加可能很明显，特别是在大小受限和提前 (AOT) 平台上。

要减少代码大小的增加幅度，请执行以下操作：

1.尽可能选择最小的 .NET 配置文件（请参阅 [.NET 配置文件支持](dotnetProfileSupport.html)）。.NET Standard 2.0 配置文件大小大约只有 .NET 4.x 配置文件的一半，因此请尽可能使用 .NET Standard 2.0 配置文件。
1.在 Unity Editor 的 Player Settings（选择 __Edit__ > __Project Settings__ > __Player__）中，启用 __Strip Engine Code__。此选项会静态分析项目中的托管代码，并删除所有未使用的代码。**注意**：只有 IL2CPP 脚本后端才附带此选项。

## 传输层安全性 (TLS)

新版 Mono 具有针对若干平台的 TLS 1.2 支持。Unity 支持 .NET 类库中的 TLS 1.0，而 TLS 仅对 Mac 独立平台播放器有效。在需要全面 TLS 支持的情况下，请使用 [UnityWebRequest](UnityWebRequest.html) 或特定于平台的本机解决方案。Unity 正积极致力于在 Unity 支持的所有平台上为所有 .NET 类库 API 添加 TLS 1.2 支持。

---

* <span class="page-edit">2018-03-15  Page amended with [editorial review](DocumentationEditorialReview.html)
</span>
