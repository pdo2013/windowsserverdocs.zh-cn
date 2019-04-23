首先，获取一个 SSL 证书为 HGS 从证书颁发机构。 每台主机需要信任的 SSL 证书，所以建议您从公司的公共密钥基础结构或第三方 CA 颁发的 SSL 证书。 IIS 支持的任何 SSL 证书支持的 HGS，但是**针对证书使用者名称必须匹配的完全限定的 HGS 服务名称**（群集分布式的网络名称）。 例如，如果 HGS 域是"bastion.local"，HGS 服务名称为"hgs"，应为"hgs.bastion.local"颁发 SSL 证书。 可以将其他 DNS 名称添加到证书的使用者备用名称字段，如有必要。

SSL 证书，请打开权限提升的 PowerShelll 会话和是运行时提供的证书路径之后[集 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

或者，如果你安装了证书的本地证书存储中，您可以通过指纹引用它：

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> 配置 HGS 使用 SSL 证书不会禁用 HTTP 终结点。
> 如果你想要仅允许使用 HTTPS 终结点，Windows 防火墙配置为阻止到端口 80 的入站的连接。
> **请不要修改 IIS 绑定**的 HGS 网站删除 HTTP 终结点; 它不支持执行此操作。
