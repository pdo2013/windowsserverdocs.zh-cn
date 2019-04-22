1. 具有目录服务管理员将新节点的计算机帐户添加到包含所有 HGS 服务器的请求权限以允许这些服务器以使用 HGS gMSA 帐户的安全组。

2. 重新启动要获取新的 Kerberos 票证，其中包含该安全组中的计算机的成员身份的新节点。 在重新启动完成后，使用属于本地管理员组的计算机上的域标识登录。

3. 在节点上安装 HGS 组托管服务帐户。

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
