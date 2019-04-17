---
title: Windows Admin Center 常见疑难解答步骤
description: Windows Admin Center 常见疑难解答步骤 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296897"
---
# Windows Admin Center 疑难解答

>适用于：Windows Admin Center、Windows Admin Center 预览版

> [!Important]
> 本指南将帮助你诊断和解决与 Windows Admin Center 相关的问题。 如果你的特定工具存在问题，请查看以了解你遇到的是否是[已知问题。](http://aka.ms/wacknownissues)

<a id="toc"></a>

## 快速链接

* [安装程序失败，带有消息：**_无法加载模块 Microsoft.PowerShell.LocalAccounts'。_**](#psmodulepath)

* 我在 Web 浏览器中收到了**无法访问此站点/页面**错误消息（请选择部署类型）
    * [安装 Windows Admin Center 作为 Windows 10 上的应用](#whitescreenw10)
    * [安装 Windows Admin Center 作为 Windows Server 上的网关](#whitescreenws)
    * [安装 Windows Admin Center 作为 Azure VM 上的网关](#whitescreenazvm)

* [Windows Admin Center 主页加载，但顿添加连接窗格中，或无法连接到任何计算机。](#winvercompat)

* [我收到了以下消息：“加载该模块时出错。 Rpc：过期重试‘Ping’。”](#winvercompat)

* [我收到以下消息:"无法安全地连接到此页面。 这可能是因为该站点使用过时或不安全的 TLS 安全设置。"](#tls)

* [遇到的问题使用远程桌面、 事件和 PowerShell 工具。](#websockets)

* [时遇到的问题，在 Edge 中使用 Azure 的功能](#azlogin)

* [我只能连接到部分服务器](#connectionissues)

* [我在**工作组**中使用 Windows Admin Center](#workgroup)

* [我之前已安装，Windows Admin Center 和任何其他内容现在可以使用相同的 TCP/IP 端口](#urlacl)

* [我的问题未在此处列出，或者此页面上的步骤没有解决我的问题。](#filebug)

<a id="psmodulepath"></a>

## 安装程序失败，带有消息：**_无法加载模块 Microsoft.PowerShell.LocalAccounts'。_**

如果已修改或删除默认 PowerShell 模块路径，则可发生此情况。 若要解决此问题，请确保```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules```是你 PSModulePath 环境变量中的**第一个**项目。 你可以达到此与下面的 PowerShell 行：

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## 我在 Web 浏览器中收到了**无法访问此站点/页面**错误消息

<a id="whitescreenw10"></a>

### 如果你安装了 Windows Admin Center 作为 **Windows 10 上的应用**

* 请检查 Windows Admin Center 是否正在运行。 查找 Windows Admin Center 图标![](../media/trayIcon.PNG)系统托盘中或**Windows Admin Center 桌面 / SmeDesktop.exe**任务管理器中。 如果没有安装，请从“开始”菜单启动 **Windows Admin Center**。

> [!NOTE] 
> 重新启动后，你必须从“开始”菜单启动 Windows Admin Center。  

* [请检查 Windows 版本](#winvercompat)

* 请确保使用 Microsoft Edge 或 Google Chrome 作为 Web 浏览器。

* 你在[首次启动](../use/get-started.md#selecting-a-client-certificate)时是否选择了正确的证书？

  * 请尝试在私人会话中打开浏览器 - 如果可行，你将需要清除缓存。

* 没有你最近升级 Windows 10 到新版本或版本？

  * 这可能已清除你的受信任的主机设置。 [请按照以下说明更新你的受信任的主机设置。](#configure-trustedhosts) 

[[返回顶部]](#toc)

<a id="whitescreenws"></a>

### 如果你安装了 Windows Admin Center 作为 **Windows Server 上的网关**

* 未从以前版本的 Windows Admin Center 升级？ 检查以确保由于[此已知的问题](known-issues.md#upgrade)而不删除的防火墙规则。 使用下面的 PowerShell 命令来确定是否存在该规则。 如果没有，请按照[以下说明](known-issues.md#upgrade)重新创建它。
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* 请检查客户端和服务器的 [Windows 版本](#winvercompat)。

* 请确保使用 Microsoft Edge 或 Google Chrome 作为 Web 浏览器。

* 在服务器上，打开任务管理器 > 服务，并确保**ServerManagementGateway / Windows Admin Center**正在运行。
![](../media/Service-TaskMan.PNG)

* 测试网关的网络连接（请将 \<values> 替换为部署信息）
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[返回顶部]](#toc)

<a id="whitescreenazvm"></a>  

### 如果你在 Azure Windows Server VM 中安装了 Windows Admin Center

* [请检查 Windows 版本](#winvercompat)
* 你是否针对 HTTPS 添加了入站端口规则？ 
* [了解有关在 Azure VM 中安装 Windows Admin Center 的详细信息](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[返回顶部]](#toc)

<a id="winvercompat"></a>

### 请检查 Windows 版本

* 打开运行对话框 (Windows 键 + R) 并启动 ```winver```。

* Windows 10 版本 1703 或更低版本中的 Microsoft Edge 不支持 Windows Admin Center。 请升级到最新版本的 Windows 10 或使用 Chrome。

* 如果你使用 insider preview 版本的 Windows 10 或服务器内部版本 17134 和 17637 之间，Windows 将拥有 bug 导致 Windows Admin Center 失败。 请使用当前的受支持的版本的 Windows。

### 请确保 Windows 远程管理 (WinRM) 服务正在运行的网关计算机和托管的节点

* 使用 WindowsKey + R 打开运行对话框
* 类型```services.msc```，然后按 enter
* 在打开的窗口中，查找 Windows 远程管理 (WinRM)，请确保它在运行并设置为自动启动

### 没有你升级你的服务器从 2016年到 2019年？

* 这可能已清除你的受信任的主机设置。 [请按照以下说明更新你的受信任的主机设置。](#configure-trustedhosts) 

[[返回顶部]](#toc)

<a id="tls"></a>

## 我收到以下消息:"无法安全地连接到此页面。 这可能是因为该站点使用过时或不安全的 TLS 安全设置。

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
你的计算机被限制为 HTTP/2 连接。 Windows Admin Center 使用集成的 Windows 身份验证，不支持 HTTP/2。 添加下的以下两个注册表值```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters```键移除 HTTP/2 限制**运行浏览器的计算机**上：

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[返回顶部]](#toc)

<a id="websockets"></a> 

## 遇到的问题使用远程桌面、 事件和 PowerShell 工具。

这三个工具需要 websocket 协议，这通常通过代理服务器和防火墙阻止。 如果你使用的 Google Chrome，则[已知问题](known-issues.md#google-chrome)与 websocket 和 NTLM 身份验证。

[[返回顶部]](#toc)


<a id="connectionissues"></a> 

## 我只能连接到部分服务器
* 请本地登录到网关计算机并尝试在 PowerShell 中 ```Enter-PSSession <machine name>```，其中 \<machine name> 应替换为你尝试在 Windows Admin Center 中管理的计算机名称。 

* 如果你的环境使用的是工作组而不是域，请参阅[在工作组中使用 Windows Admin Center](#workgroup)。

* **使用本地管理员帐户：** 如果你使用的是本地用户帐户而不是内置 Administrator 帐户，则你将需要在目标计算机上启用策略，方法是在目标计算机上以管理员身份在 PowerShell 中或在命令提示符中运行以下命令：

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[返回顶部]](#toc)

<a id="workgroup"></a>

## 在工作组中使用 Windows Admin Center 

### 你使用的是哪个帐户？
请确保你使用的凭据是目标服务器的本地管理员组成员。 在某些情况下，WinRM 还需要远程管理用户组成员身份。 如果你使用的是本地用户帐户而**不是内置 Administrator 帐户**，则你将需要在目标计算机上启用策略，方法是在目标计算机上以管理员身份在 PowerShell 中或在命令提示符中运行以下命令：

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### 你是否通过不同子网连接到工作组计算机？

若要连接与网关不在同一个子网的工作组计算机，请确保 WinRM (TCP 5985) 的防火墙端口允许目标计算机上的入站流量。 你可以在目标计算机上以管理员身份在 PowerShell 中或在命令提示符中运行以下命令，以创建此防火墙规则：

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### 配置 TrustedHosts

安装 Windows Admin Center 时，你可以选择允许 Windows Admin Center 管理网关的 TrustedHosts 设置。 在工作组环境中，或在域中使用本地管理员凭据时需要进行此设置。 如果你选择放弃此设置，则必须手动配置 TrustedHosts。

**使用 PowerShell 命令修改 TrustedHosts：**

1. 打开管理员 PowerShell 会话。
2. 查看你的当前 TrustedHosts 设置：

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

    > [!WARNING]
    > 如果你当前已有 TrustedHosts 设置，则以下命令将覆盖你的设置。 我们建议你通过以下命令将当前设置保存到文本文件，以便在需要时恢复设置。

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. 将想要管理的计算机的 TrustedHosts 设置为 NetBIOS、IP 或 FQDN：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >为简便起见，你可以利用通配符一次性设置所有 TrustedHosts。

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. 完成测试后，你可以从提升的 PowerShell 会话中发出以下命令来清除 TrustedHosts 设置：

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. 如果你之前导出了设置，请打开文件，复制值，然后使用此命令：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[返回顶部]](#toc)

<a id="urlacl"></a>

## 我之前已安装，Windows Admin Center 和任何其他内容现在可以使用相同的 TCP/IP 端口

在提升的命令提示符中，手动运行这两个命令：

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[返回顶部]](#toc)

<a id="azlogin"></a>

## 时遇到的问题，在 Edge 中使用 Azure 的功能

Edge 具有[已知问题](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge)相关影响 Azure 登录 Windows Admin Center 中的安全区域。 如果你有使用 Edge 时使用 Azure 功能时遇到问题，请尝试添加https://login.microsoftonline.com，https://login.live.com为网关的 URL 是受信任的站点和允许对站点的客户端浏览器上边缘弹出窗口阻止程序设置。 

若要实现此目的，请执行以下操作：
1. **Internet 选项**Windows 开始菜单中搜索
2. 转到**安全**选项卡
3. 下的**受信任的站点**选项中，单击**站点**按钮，然后将 Url 添加打开的对话框中。 你将需要添加网关的 URL 以及https://login.microsoftonline.com和https://login.live.com。
4. 转到**隐私**选项卡
5. 在**弹出窗口阻止程序**部分中，单击**设置**按钮，然后将 Url 添加打开的对话框中。 你将需要添加网关的 URL 以及https://login.microsoftonline.com和https://login.live.com。


[[返回顶部]](#toc)

<a id="azissue"></a>

## 有一项 Azure 相关功能的问题？

请向我们发送一封电子邮件在 wacFeedbackAzure@microsoft.com 带有以下信息：
* [下面列出的问题](#filebug)的一般问题信息。 
* 描述你的问题以及你所花的时间要重现问题的步骤。 
* 你没有之前注册到 Azure 使用新 AadApp.ps1 可下载脚本网关并升级到版本 1807年？ 或者，也未注册到 Azure 使用从网关设置 > Azure UI 网关？
* 是你与多个目录/租户相关联的 Azure 帐户？
    * 如果是： 注册到 Windows Admin Center 的 Azure AD 应用程序时, 已在 Azure 中使用默认的目录的目录？ 
* 你的 Azure 帐户是否有权访问多个订阅？
* 你已使用的订阅是否具有附加的计费？
* 你在登录到多个 Azure 帐户时遇到问题？
* 你的 Azure 帐户是否需要多重身份验证？
* 是你想要管理 Azure 虚拟机的计算机？
* 在 Azure VM 上安装 Windows Admin Center？

[[返回顶部]](#toc)

<a id="filebug"></a>

## 仍然无法解决，或此处没有列出你的问题？ [疑难解答常见问题]

请转到“事件查看器”>“应用程序和服务”>“Microsoft-ServerManagementExperience”，查找所有错误或警告。

请在我们的 [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) 上报告错误并描述你遇到的问题。

请包含你在事件日志中找到的所有错误或警告，以及以下信息： 

* **安装** Windows Admin Center 的平台（Windows 10 或 Windows Server）：
    * 如果服务器上安装，什么是**运行浏览器的计算机**，以访问 Windows Admin Center 的 Windows[版本](#winvercompat)： 
    * 你使用安装程序创建的自签名的证书？
    * 如果你使用了自己的证书，那么使用者名称是否与计算机匹配？
    * 如果你使用了自己的证书，那么该证书是否指定了其他使用者名称？
* 安装时是否使用了默认端口设置？
    * 如果不是，那么你指定的是哪个端口？
* **安装** Windows Admin Center 的计算机是否加入了域？
* **安装** Windows Admin Center 的 Windows [版本](#winvercompat)：
* 你**尝试管理**的计算机是否加入了域？
* 你**尝试管理**的计算机的 Windows [版本](#winvercompat)：
* 你使用的是哪个浏览器？
    * 如果你使用的是 Google Chrome，那么它的版本是？ （“帮助”>“关于 Google Chrome”）

[[返回顶部]](#toc)