---
title: Windows Admin Center 常见疑难解答步骤
description: Windows Admin Center 常见疑难解答步骤 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 8e718eda7859c5e0b6949829c225b28e882525ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811705"
---
# <a name="troubleshooting-windows-admin-center"></a>Windows Admin Center 疑难解答

> 适用于：Windows Admin Center，Windows Admin Center 预览版

> [!Important]
> 本指南将帮助你诊断和解决与 Windows Admin Center 相关的问题。 如果你的特定工具存在问题，请查看以了解你遇到的是否是[已知问题。](http://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>安装程序失败，出现消息： **_无法加载模块 Microsoft.PowerShell.LocalAccounts。_ **

如果您默认 PowerShell 模块的路径已修改或删除，则可能发生此问题。 若要解决此问题，请确保```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules```是**第一个**在 PSModulePath 环境变量中的项。 可以使用 PowerShell 的以下行来实现此目的：

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>我在 Web 浏览器中收到了**无法访问此站点/页面**错误消息

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>如果你安装了 Windows Admin Center 作为 **Windows 10 上的应用**

* 请检查 Windows Admin Center 是否正在运行。 查找 Windows Admin Center 图标![](../media/trayIcon.PNG)系统任务栏中或**Windows Admin Center 桌面 / SmeDesktop.exe**任务管理器中。 如果没有安装，请从“开始”菜单启动 **Windows Admin Center**。

> [!NOTE] 
> 重新启动后，你必须从“开始”菜单启动 Windows Admin Center。  

* [检查 Windows 版本](#check-the-windows-version)

* 请确保使用 Microsoft Edge 或 Google Chrome 作为 Web 浏览器。

* 你在[首次启动](../use/get-started.md#selecting-a-client-certificate)时是否选择了正确的证书？

  * 请尝试在私人会话中打开浏览器 - 如果可行，你将需要清除缓存。

* 未在最近将 Windows 10 升级到新的生成或版本？

  * 这可能会清除受信任的主机设置。 [按照这些说明来更新受信任的主机设置。](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>如果你安装了 Windows Admin Center 作为 **Windows Server 上的网关**

* 您是否升级从以前版本的 Windows Admin Center？ 检查以确保由于未删除防火墙规则[此已知问题](known-issues.md#upgrade)。 使用以下 PowerShell 命令以确定是否存在该规则。 如果没有，请按照[这些说明](known-issues.md#upgrade)来重新创建它。
    
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* 请检查客户端和服务器的 [Windows 版本](#check-the-windows-version)。

* 请确保使用 Microsoft Edge 或 Google Chrome 作为 Web 浏览器。

* 在服务器上，打开任务管理器 > 服务并确保**ServerManagementGateway / Windows Admin Center**正在运行。
![](../media/Service-TaskMan.PNG)

* 测试网络连接到网关 (替换\<值 > 你的部署中的信息)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>如果已在 Azure Windows Server VM 中安装 Windows Admin Center

* [检查 Windows 版本](#check-the-windows-version)
* 你是否针对 HTTPS 添加了入站端口规则？ 
* [了解有关在 Azure VM 中安装 Windows Admin Center 的详细信息](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>请检查 Windows 版本

* 打开运行对话框 (Windows 键 + R) 并启动 ```winver```。

* Windows 10 版本 1703 或更低版本中的 Microsoft Edge 不支持 Windows Admin Center。 请升级到最新版本的 Windows 10 或使用 Chrome。

* 如果使用版本 17134 和 17637 之间的内部版本使用的 insider preview 版本的 Windows 10 或 Server，Windows 将有一个 bug 会导致 Windows Admin Center 失败。 请使用当前支持的 Windows 版本。

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>请确保网关计算机和托管的节点上运行的 Windows 远程管理 (WinRM) 服务

* 使用 WindowsKey + R 打开运行对话框
* 类型```services.msc```，然后按 enter
* 在打开的窗口中，查找 Windows 远程管理 (WinRM)，请确保它正在运行，并且设置为自动启动

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>未将服务器升级从 2016年到 2019年？

* 这可能会清除受信任的主机设置。 [按照这些说明来更新受信任的主机设置。](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>我收到消息："无法安全地连接到此页。 这可能是因为该站点使用过时或不安全的 TLS 安全性设置。

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
只有 HTTP/2 连接到你的计算机。 Windows Admin Center 使用集成的 Windows 身份验证，不支持 HTTP/2。 添加以下两个注册表值下的```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters```密钥上**运行浏览器的计算机**删除 HTTP/2 限制：

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>遇到问题的远程桌面、 事件和 PowerShell 工具。

这三个工具需要 websocket 协议，通常通过代理服务器和防火墙阻止。 如果使用 Google Chrome，则[已知问题](known-issues.md#google-chrome)使用 websocket 和 NTLM 身份验证。

## <a name="i-can-connect-to-some-servers-but-not-others"></a>我只能连接到部分服务器

* 登录到本地网关计算机，并尝试```Enter-PSSession <machine name>```在 PowerShell 中，替换\<计算机名称 > 尝试在 Windows Admin Center 中管理的计算机的名称。 

* 如果你的环境使用的是工作组而不是域，请参阅[在工作组中使用 Windows Admin Center](#using-windows-admin-center-in-a-workgroup)。

* **使用本地管理员帐户：** 如果使用的不是内置管理员帐户的本地用户帐户，将需要通过运行以下命令在 PowerShell 中或在命令提示符下以管理员身份在目标计算机上启用目标计算机上的策略：

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>在工作组中使用 Windows Admin Center

### <a name="what-account-are-you-using"></a>你使用的是哪个帐户？
请确保你使用的凭据是目标服务器的本地管理员组成员。 在某些情况下，WinRM 还需要远程管理用户组成员身份。 如果你使用的是本地用户帐户而**不是内置 Administrator 帐户**，则你将需要在目标计算机上启用策略，方法是在目标计算机上以管理员身份在 PowerShell 中或在命令提示符中运行以下命令：

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>你是否通过不同子网连接到工作组计算机？

若要连接与网关不在同一个子网的工作组计算机，请确保 WinRM (TCP 5985) 的防火墙端口允许目标计算机上的入站流量。 你可以在目标计算机上以管理员身份在 PowerShell 中或在命令提示符中运行以下命令，以创建此防火墙规则：

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>配置 TrustedHosts

安装 Windows Admin Center 时，你可以选择允许 Windows Admin Center 管理网关的 TrustedHosts 设置。 在工作组环境中，或在域中使用本地管理员凭据时需要进行此设置。 如果你选择放弃此设置，则必须手动配置 TrustedHosts。

**若要修改 TrustedHosts 使用 PowerShell 命令：**

1. 打开管理员 PowerShell 会话。
2. 查看你的当前 TrustedHosts 设置：

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > 如果你当前已有 TrustedHosts 设置，则以下命令将覆盖你的设置。 我们建议你通过以下命令将当前设置保存到文本文件，以便在需要时恢复设置。
   > 
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. 将想要管理的计算机的 TrustedHosts 设置为 NetBIOS、IP 或 FQDN：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > 为简便起见，你可以利用通配符一次性设置所有 TrustedHosts。
   > 
   >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. 完成测试后，你可以从提升的 PowerShell 会话中发出以下命令来清除 TrustedHosts 设置：

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. 如果你之前导出了设置，请打开文件，复制值，然后使用此命令：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>我先前有 Windows Admin Center 安装，并且现在没有其他可以使用相同的 TCP/IP 端口

手动在提升的命令提示符中运行这两个命令：

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>在 Edge 中 azure 功能未正常运行

边缘已[已知问题](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge)到会影响 Windows Admin Center 中的 Azure 登录名的安全区域相关。 如果在使用 Edge 时使用的 Azure 功能时遇到问题，请尝试添加 https://login.microsoftonline.com， https://login.live.com并为你网关的 URL 受信任的站点和到允许的站点的客户端浏览器上的 Edge 弹出窗口阻止程序设置。 

要实现此目的，请执行以下操作：
1. 搜索**Internet 选项**在 Windows 开始菜单
2. 转到**安全**选项卡
3. 下**受信任的站点**选项，请单击**站点**按钮，然后在打开的对话框中添加 Url。 将需要添加网关 URL，以及 https://login.microsoftonline.com和 https://login.live.com。
4. 转到**隐私**选项卡
5. 下**弹出窗口阻止程序**部分中，单击**设置**按钮，然后在打开的对话框中添加 Url。 将需要添加网关 URL，以及 https://login.microsoftonline.com和 https://login.live.com。

## <a name="having-an-issue-with-an-azure-related-feature"></a>出现问题。 Azure 相关的功能？

请向我们发送电子邮件wacFeedbackAzure@microsoft.com使用以下信息：
* 中的常规问题信息[下面列出的问题](#providing-feedback-on-issues)。
* 描述你遇到的问题和重现此问题所采取的步骤。 
* 你没有以前注册到 Azure 中使用新建 AadApp.ps1 可下载脚本在网关，然后升级到版本 1807年？ 或者，也未注册网关使用网关设置从 UI 向 Azure > Azure？
* 是 Azure 帐户与多个目录/租户相关联？
    * 如果是：注册时向 Windows Admin Center 的 Azure AD 应用程序，已在 Azure 中使用默认目录的目录？ 
* 你的 Azure 帐户是否有权访问多个订阅？
* 已使用的订阅是否具有附加的计费？
* 你在登录到多个 Azure 帐户时遇到问题？
* 你的 Azure 帐户是否需要多重身份验证？
* 是尝试管理 Azure VM 的计算机？
* 在 Azure VM 上安装 Windows Admin Center？

## <a name="providing-feedback-on-issues"></a>提供有关问题的反馈

请转到“事件查看器”>“应用程序和服务”>“Microsoft-ServerManagementExperience”，查找所有错误或警告。

请在我们的 [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) 上报告错误并描述你遇到的问题。

请包含你在事件日志中找到的所有错误或警告，以及以下信息： 

* **安装** Windows Admin Center 的平台（Windows 10 或 Windows Server）：
    * 如果服务器上安装，什么是 Windows[版本](#check-the-windows-version)的**运行浏览器的计算机**访问 Windows Admin Center: 
    * 是否使用由安装程序创建的自签名的证书？
    * 如果你使用了自己的证书，那么使用者名称是否与计算机匹配？
    * 如果你使用了自己的证书，那么该证书是否指定了其他使用者名称？
* 安装时是否使用了默认端口设置？
    * 如果不是，那么你指定的是哪个端口？
* **安装** Windows Admin Center 的计算机是否加入了域？
* **安装** Windows Admin Center 的 Windows [版本](#check-the-windows-version)：
* 你**尝试管理**的计算机是否加入了域？
* 你**尝试管理**的计算机的 Windows [版本](#check-the-windows-version)：
* 你使用的是哪个浏览器？
    * 如果你使用的是 Google Chrome，那么它的版本是？ （“帮助”>“关于 Google Chrome”）

