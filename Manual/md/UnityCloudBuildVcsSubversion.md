# 将 Apache Subversion (SVN) 用于 Unity Cloud Build

Unity Cloud Build 支持 Apache Subversion (SVN) 代码仓库中托管的项目（请参阅 [subversion.apache.org](https://subversion.apache.org/)）。

**用户名和密码**

在 SVN 服务器上，为 Unity Cloud Build 创建新用户以及安全密码。如果 SVN 主机支持只读的用户帐户，请将此帐户设置为只读。

**公有 SSH 密钥 **

Unity Cloud Build 不支持使用公有 SSH 密钥来连接到 SVN 代码仓库。应该使用用户名和密码。

**SSL 证书**

不支持自签名 SSL 证书。Unity Cloud Build 不会与采用自签名证书的服务器进行 SSL 握手。证书中的主机名必须与 Unity Cloud Build 访问的主机名相匹配。
