1.  运行[安装 HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver)加入域并将提升到域控制器的节点。

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  在服务器重启，当使用域管理员帐户登录。

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->