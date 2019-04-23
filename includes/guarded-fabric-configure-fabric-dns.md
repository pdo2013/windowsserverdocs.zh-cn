有多种方法来配置 fabric 域的名称解析。 一种简单方式是为在构造中设置 DNS 中的条件转发器区域。 若要设置此区域，请在构造 DNS 服务器上提升的 Windows PowerShell 控制台中运行以下命令。 替换的姓名和地址作为你的环境时需要以下 Windows PowerShell 语法中。 添加其他 HGS 节点的主服务器。

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    