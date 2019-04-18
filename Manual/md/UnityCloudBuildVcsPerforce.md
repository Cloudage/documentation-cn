# 将 Perforce 用于 Unity Cloud Build

Unity Cloud Build 支持 Perforce 中托管的项目（请参阅 [www.perforce.com](https://www.perforce.com)）。

**用户名和密码**

在 Perforce 服务器上，为 Unity Cloud Build 创建新用户以及安全密码。如果 Perforce 主机支持只读的用户帐户，请将此帐户设置为只读。

**票据验证**

请使用 Perforce 管理工具来创建新用户。在向 Perforce 验证身份时，Cloud Build 将在登录过程中检索并使用分配的票据。

**已启用 Unicode 的服务器**

Unity Cloud Build 不支持已启用 Unicode 的 Perforce 服务器。
