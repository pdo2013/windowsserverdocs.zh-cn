主机保护者服务应安装在单独的 Active Directory 林中。
确保 HGS 机器**不**加入到域之前开始并以本地计算机管理员查看身份登录。

运行以下命令以安装主机保护者服务和配置它的域。
此处指定的密码将仅应用到目录服务修复模式密码的 Active Directory;它将*不*更改管理员帐户的登录密码。
您可以提供-HgsDomainName 所选任何的域名。

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
