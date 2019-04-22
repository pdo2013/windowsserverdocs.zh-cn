如果你提供任何证书到 HGS 使用指纹，将指示您授予对那些证书的私钥的 HGS 读取访问权限。 在服务器上安装桌面体验，请完成以下步骤：

1.  打开本地计算机证书管理器 (**certlm.msc**)
2.  查找证书 > 右键单击 > 所有任务 > 管理私钥
3.  单击“添加”。
4.  在对象选取器窗口中，单击**对象类型**，并启用**服务帐户**
5.  输入中的警告文本中提到的服务帐户的名称 `Initialize-HgsServer`
6.  请确保 gMSA 具有"读取"访问权限的私钥。

在 server core 上将需要下载 PowerShell 模块，以帮助设置专用密钥的权限。

1.  运行`Install-Module GuardedFabricTools`HGS 服务器，如果它具有 Internet 连接或运行上`Save-Module GuardedFabricTools`另一台计算机和复制到 HGS 服务器通过模块上。
2.  运行 `Import-Module GuardedFabricTools`。 这将添加其他属性在 PowerShell 中找到的证书对象。
3.  在 PowerShell 中查找你的证书指纹 `Get-ChildItem Cert:\LocalMachine\My`
4.  更新 ACL，请将用您自己的指纹和以下代码中使用的帐户的 gMSA 帐户的警告文本中列出`Initialize-HgsServer`。

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

如果您正在使用、 由 HSM 支持证书或证书存储在第三方密钥存储提供程序，这些步骤可能不适用于您。 请查阅密钥存储提供程序的文档，了解如何管理您的私钥的权限。 在某些情况下，没有授权，或安装证书时，授权提供给整个计算机。

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->