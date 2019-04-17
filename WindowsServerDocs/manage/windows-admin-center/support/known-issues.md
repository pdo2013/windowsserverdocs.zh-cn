---
title: Windows Admin Center 已知问题
description: Windows Admin Center 已知问题 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296889"
---
# Windows Admin Center 已知问题

>适用于：Windows Admin Center、Windows Admin Center 预览版

如果遇到此页中未列出的问题，请[告诉我们](http://aka.ms/WACfeedback)。

## Lenovo XClarity 系统集成商
Windows Admin Center 和 Lenovo XClarity 集成商的特定版本组合之间存在不兼容。 如果当前使用或计划在 Windows Admin Center 中使用 Lenovo XClarity 系统集成商扩展，下面是你需要知道：

- Lenovo XClarity 系统集成商扩展版本 1.0.4 可与 Windows Admin Center 1809.5 完全兼容。
- Lenovo XClarity 系统集成商扩展版本 1.0.4 已公开 Windows Admin Center 1904 兼容性问题。 Microsoft 和 Lenovo 工程师正在积极研究在一起，并且我们希望尽快提供解决方案。 在 Windows Admin Center 文档网站上，此处发布任何更新，你还可以引用[Lenovo 的支持页面](https://support.lenovo.com/solutions/ht507549)以供参考。
- 如果你是 Lenovo 的 XClarity 功能频繁用户，你可以在 Windows Admin Center 1809.5 才能继续使用 XClarity 系统集成商 1.0.4，任一持续或可以升级到 Windows Admin Center 1904 并单独使用独立的 XClarity 管理员作为解决方法现在的软件。


## 安装程序

- 使用你自己的证书安装 Windows Admin Center 时，请注意，如果你从证书管理器 MMC 工具中复制指纹，[指纹开头将包含无效字符。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 解决方法是键入指纹的第一个字符，然后复制/粘贴其余字符。

- 不支持使用端口 1024。 在服务模式下，你可能会 （可选） 配置端口 80 重定向到你指定的端口。

- 如果 Windows 更新服务 (wuauserv) 停止并禁用，安装程序将失败。 [19100629]

### 升级

- 升级 Windows Admin Center 在从以前版本的服务模式下，如果你使用 msiexec 在安静模式下，你可能会遇到的问题位置删除 Windows Admin Center 端口的入站的防火墙规则。
  - 若要重新创建规则，请执行以下命令从提升的 PowerShell 控制台，\<port> 替换为 Windows Admin Center （默认值 443。） 配置的端口

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## 概要

- 如果你有 Windows Admin Center 安装为**Windows Server 2016**上下大量使用网关，该服务可能会崩溃包含在事件日志中出现错误```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```。 这是由于 Windows Server 2019 中已修复此 bug。 Windows Server 2016 的修补程序已包含 2 月 2019 累积更新， [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)。

- 如果你有 Windows Admin Center 安装为网关，并且你的连接列表似乎已损坏，请执行以下步骤：

>[!WARNING]
>这将删除的连接列表和网关的所有 Windows Admin Center 用户的设置。

  1. 卸载 Windows Admin Center
  2. 删除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下面的 **Server Management Experience** 文件夹
  3. 重新安装 Windows Admin Center

- 如果长期让工具保持打开和空闲状态，则可能会出现一些**错误：运行空间状态对此操作无效**错误。 如果出现这种情况，请刷新浏览器。 如果你遇到此问题，[向我们发送反馈](http://aka.ms/WACfeedback)。

- 刷新 URL 较长的页面时，你可能会遇到 **500 错误**。 [12443710]

- 在某些工具中，浏览器的拼写检查器可能会将某些字段值标记为拼写错误。 [12425477]

- 在某些工具中，命令按钮可能无法在单击后立即反映状态更改，并且工具 UI 可能不会自动反映对某些属性所做的更改。 你可以单击**刷新**以从目标服务器中检索最新状态。 [11445790]

- 如果你选择连接使用多选复选框，然后连接按筛选列表，标记筛选连接列表的标记，原始选择仍然存在，因此你选择的任何操作将应用到所有先前选定的计算机。 [18099259]

- 可能存在细微差异的 Windows Admin Center 模块和第三方软件通知中列出的内容中运行的操作系统的版本号。

### 扩展管理器

- 更新 Windows Admin Center 时，你必须重新安装你的扩展。
- 如果你添加扩展源无法访问，没有任何警告。 [14412861]

## 特定于浏览器的问题

### Microsoft Edge

- 在某些情况下，使用 Microsoft Edge 通过 Internet 访问 Windows Admin Center 网关时，可能会遇到加载时间较长的问题。 在 Windows Admin Center 网关使用自签名证书的 Azure 虚拟机上可能会出现这种情况。 [13819912]

- 使用 Azure Active Directory 作为标识提供者并且使用自签名证书或其他不受信任的证书配置 Windows Admin Center 时，无法在 Microsoft Edge 中完成 AAD 身份验证。  [15968377]

- 如果你有 Windows Admin Center 作为服务部署，并且你使用的 Microsoft Edge 作为浏览器，连接到 Azure 网关生成一个新的浏览器窗口后可能会失败。 尝试解决此问题，通过添加https://login.microsoftonline.com， https://login.live.com，你的网关的 URL 是受信任的站点和允许客户端浏览器上的弹出窗口阻止程序设置的站点。 有关修复此问题的[疑难解答指南](troubleshooting.md#azlogin)中的更多指南。 [17990376]

- 如果你有 Windows Admin Center 安装在桌面模式下，在 Microsoft Edge 中的浏览器选项卡不会显示 favicon。 [17665801]

### Google Chrome

- 版本 70 （释放后期，2018 年 10 月） 之前的 Chrome 具有一个关于 websocket 协议和 NTLM 身份验证的[bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) 。 这会影响以下工具：事件、PowerShell、远程桌面。

- Chrome 可能会弹出多个凭据提示，尤其是在**工作组**（非域）环境中添加连接体验期间。

- 如果你有 Windows Admin Center 部署为一项服务，从网关的 URL 的弹出窗口需要为任何 Azure 集成功能才能启用。 这些服务包括 Azure 网络适配器、 Azure 更新管理和 Azure Site Recovery。

### Mozilla Firefox

未用 Mozilla Firefox 对 Windows Admin Center 进行测试，但大多数功能应该会正常工作。

- Windows 10 安装：Mozilla Firefox 有其自己的证书存储，因此你必须将 ```Windows Admin Center Client``` 证书导入到 Firefox 中才能在 Windows 10 上使用 Windows Admin Center。

<a id="websockets"></a>

## 使用代理服务时的 WebSocket 兼容性

Windows Admin Center 中的远程桌面、PowerShell 和事件模块使用 WebSocket 协议，使用代理服务时通常不支持该协议。 [预览版](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)内的 Azure AD 应用程序代理兼容性中提供了 WebSocket 支持，我们正在寻求有关兼容性的反馈。

## 适用于 Windows Server 2016 (2012 R2、 2012、 2008 R2) 之前的版本的支持

> [!NOTE]
> Windows Admin Center 需要的 PowerShell 功能不包括在 Windows Server 2012 R2、 2012年或 2008 R2 中。 如果你将使用 Windows Admin Center 管理 Windows Server，你将需要在这些服务器上安装 WMF 5.1 或更高版本。

在 PowerShell 中键入 `$PSVersiontable` 以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。

如果未安装，则可以[下载并安装 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

<a id="rbacknownissues"></a>

## 基于角色的访问控制 (RBAC)

- 在配置为使用 Windows Defender 应用程序控制（WDAC，以前称为“代码完整性”）的计算机上，RBAC 部署将不会成功 [16568455]

- 若要在群集中使用 RBAC，必须将配置分别部署到每个成员节点。

- 部署 RBAC 后，你可能会遇到未授权错误，这些错误被错误地归因于 RBAC 配置。 [16369238]

## 服务器管理器解决方案

### 服务器设置

- 如果修改了设置，然后尝试导航而不保存，该页面将向你发出警告有关未保存的更改，但继续导航。 你可能最终在其中选择设置选项卡与页面的内容不匹配的状态。 [19905798] [19905787]

### 证书

- 无法将 .PFX 加密证书导入到当前用户存储。 [11818622]

### 设备

- 使用键盘在表格中导航时, 所选内容可能会跳转到表格组的顶部。 [16646059]

### 事件

- [使用代理服务时的 WebSocket 兼容性](#websockets)会影响事件。

- 导出大日志文件时，你可能会遇到一个引用“数据包大小”的错误。 [16630279]

  - 要解决此问题，请在网关计算机上提升的命令提示符下使用以下命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### 文件

- 仍不支持上载或下载大文件。 （\~100mb 限制）[12524234]

### PowerShell

- [使用代理服务时的 WebSocket 兼容性](#websockets)会影响 PowerShell

- 像在桌面 PowerShell 控制台中那样通过一次右键单击进行粘贴不起作用。 系统会改为向你呈现浏览器的上下文菜单，你可以在其中选择“粘贴”。 Ctrl-V 也可以正常使用。

- 用 Ctrl-C 进行复制不起作用，系统始终将向控制台发送 Ctrl-C 中断命令。 右键单击上下文菜单中的“复制”可以正常使用。

- 缩小 Windows Admin Center 窗口时，终端内容将重新排列，但是再次将其放大时，内容可能不会恢复为之前的状态。 如果内容变得杂乱，你可以尝试使用 Clear-Host，或者使用终端上面的按钮断开连接并重新连接。

### 注册表编辑器

- 未实现搜索功能。 [13820009]

### 远程桌面

- 远程桌面工具可能无法连接管理 Windows Server 2012 时。 [20258278]

- 当使用远程桌面连接到未加入域的计算机，你必须输入你的帐户```MACHINENAME\USERNAME```格式。

- 某些配置可以阻止 Windows Admin Center 的远程桌面客户端使用组策略。 如果遇到此问题，启用```Allow users to connect remotely by using Remote Desktop Services```下 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 远程桌面受[websocket 兼容性。](#websockets)

- 远程桌面工具当前不支持在本地桌面和远程会话之间复制/粘贴任何文本、图像或文件。

- 要在远程会话中进行任何复制/粘贴，你可以正常复制（右键单击并选择“复制”或按 Ctrl+C），但粘贴时需要右键单击并选择“粘贴”（Ctrl+V 不起作用）

- 你无法将以下键命令发送到远程会话
  - Alt+Tab
  - 功能键
  - Windows 键
  - PrtScn

- 远程应用 – 启用远程桌面设置中的远程应用工具后该工具可能未显示在工具列表管理具有桌面体验的服务器时。 [18906904]

### 角色和功能

- 选择安装源不可用的角色或功能时，将跳过它们。 [12946914]

- 如果你选择在安装角色后不自动重新启动，我们将不会再次询问。 [13098852]

- 如果你选择自动重新启动，则将在状态更新为 100% 之前重新启动。 [13098852]

### 存储

- 读取配额信息而无需错误通知可能会失败 （仍然会出现错误在浏览器的控制台） [18962274]

- 下层：DVD/CD/软盘驱动器在下层未显示为卷。

- 下层：卷和磁盘中的某些属性在下层不可用，因此它们在详细信息面板中显示为未知或空白。

- 下层：创建新卷时，ReFS 在 Windows 2012 和 2012 R2 计算机上仅支持 64K 的分配单元大小。 如果在下层目标上使用较小的分配单元大小创建了 ReFS 卷，则文件系统格式化将失败。 新卷将无法使用。 解决方法是删除卷并使用 64K 分配单元大小。

### 更新

- 安装更新后，安装状态可能会被缓存，并需要刷新浏览器。

- 你可能会遇到错误:"键集不存在"尝试设置 Azure 更新管理时。 在此情况下，请尝试在托管的节点的上的以下修正步骤
    1. 停止加密服务服务。
    2. 更改文件夹选项以显示隐藏文件 （如果需要）。
    3. 到达"%allusersprofile%\microsoft\crypto\rsa\s-1-5-18"文件夹并删除所有内容。
    4. 重新启动加密服务服务。
    5. 重复使用 Windows Admin Center 的更新管理设置

### 虚拟机

- 管理 Windows Server 2012 主机上的虚拟机时, 在浏览器虚拟机连接工具将无法连接到 VM。 下载.rdp 文件，以连接到虚拟机应该仍起作用。 [20258278]

- Azure Site Recovery – 如果 ASR 之外 WAC，在主机上的安装了你将无法保护 VM 内 WAC [18972276]

- 目前不支持 Hyper-V 管理器中提供的高级功能，如虚拟 SAN 管理器、移动虚拟机、导出虚拟机、虚拟机复制。

### 虚拟交换机

- 交换机嵌入式组合 (SET)：将 NIC 添加到组时，它们必须位于同一子网上。

## 计算机管理解决方案

计算机管理解决方案包含服务器管理器解决方案中的一部分工具，因此存在相同的已知问题，以及以下计算机管理解决方案特定问题：

- 如果你使用 Microsoft 帐户 ([MSA](https://account.microsoft.com/account/))，或者如果你使用 Azure Active Directory (AAD) 登录到 Windows 10 计算机上，则必须指定"管理-为"凭据来管理本地计算机 [16568455]

- 尝试管理 localhost 时，将提示你提升网关进程。 如果单击“用户帐户控制”弹出框中的**否**，Windows Admin Center 将无法再次显示它。 在这种情况下，通过右键单击系统托盘中的 Windows Admin Center 图标并选择“退出”以退出网关进程，然后从“开始”菜单重新启动 Windows Admin Center。

- 默认情况下，Windows 10 未开启 WinRM/PowerShell 远程处理
  
  - 要启用 Windows 10 客户端管理，必须利用提升的 PowerShell 提示符发出 ```Enable-PSRemoting``` 命令。

  - 你可能还需要通过 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 来更新防火墙以允许从本地子网外部的连接。 有关限制性更强的网络方案，请参阅[本文档](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## 故障转移群集管理器解决方案

- 管理群集（是超聚合还是传统？）时可能会遇到**找不到 Shell** 错误。 如果发生这种情况，请重新加载浏览器，或导航到其他工具并返回。 [13882442]

- 管理尚未完全配置的下层（Windows Server 2012 或 2012 R2）群集时，可能会出现问题。 此问题的解决方法是确保已在群集的**每个成员节点**上安装并启用了 Windows 功能 **RSAT-Clustering-PowerShell**。 要使用 PowerShell 执行此操作，请在所有群集节点上输入命令 `Install-WindowsFeature -Name RSAT-Windows-PowerShell`。 [12524664]

- 可能需要添加群集与整个 FQDN 才能正确被发现。

- 使用安装为网关的 Windows Admin Center 连接到群集并提供显式用户名/密码进行身份验证时，必须选择 **Use these credentials for all connections** 以便可使用凭据查询成员节点。

## 超聚合群集管理器解决方案

- 诸如**驱动器 - 更新固件**、**服务器 - 删除**和**卷 - 打开**之类的某些命令被禁用并且当前不受支持。