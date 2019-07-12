---
title: Windows Admin Center 已知问题
description: Windows Admin Center 已知问题 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: e7cf6fc6a4fae2eee76409bd6af4ef2ff6ed35a3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811776"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知问题

> 适用于：Windows Admin Center，Windows Admin Center 预览版

如果遇到此页中未列出的问题，请[告诉我们](http://aka.ms/WACfeedback)。

## <a name="lenovo-xclarity-integrator"></a>Lenovo XClarity 系统集成商

与 Windows Admin Center 版本 1904.1 现在已解决的 Lenovo XClarity 集成器扩展插件和 Windows Admin Center 版本 1904年以前披露不兼容性问题。 我们强烈建议你更新到最新的受支持 Windows Admin Center 版本。

- Lenovo XClarity 集成器扩展版本 1.1 是与 Windows Admin Center 1904.1 完全兼容。 我们强烈建议你更新到最新版本的 Windows Admin Center 和 Lenovo 扩展。
- 出于任何原因，如果您需要继续使用 Windows Admin Center 1809.5 次，您可以使用 XClarity 集成器 1.0.4 还将在 Windows Admin Center 扩展源中提供的直到 Windows Admin Center 1809.5 不再支持基于我们[的支持策略](../support/index.md)。

## <a name="installer"></a>安装程序

- 使用你自己的证书安装 Windows Admin Center 时，请注意，如果你从证书管理器 MMC 工具中复制指纹，[指纹开头将包含无效字符。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 解决方法是键入指纹的第一个字符，然后复制/粘贴其余字符。

- 不支持使用低于 1024年的端口。 在服务模式下，你可能会选择配置的端口 80 将重定向到你指定的端口。

- 如果停止并禁用 Windows 更新服务 (wuauserv) 之后，安装程序将失败。 [19100629]

### <a name="upgrade"></a>升级

- 升级 Windows Admin Center 在从以前版本的服务模式下，如果在安静模式下使用 msiexec，可能会遇到删除 Windows Admin Center 端口的入站的防火墙规则的问题。
  - 若要重新创建该规则，请从提升的 PowerShell 控制台中，执行以下命令替换\<端口 > 替换为 Windows Admin Center （默认值 443。） 配置的端口

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>常规

- 如果必须在作为网关安装 Windows Admin Center **Windows Server 2016**大量使用，该服务可能会崩溃，包含在事件日志中错误```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```。 这是由于 Windows Server 2019 中已修复的 bug。 Windows Server 2016 的修补程序已包括在内，2019 年 2 月累积更新， [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)。

- 如果有 Windows Admin Center 作为网关安装并连接列表似乎已损坏，请执行以下步骤：

   > [!WARNING]
   >这将删除的连接列表和在网关上的所有 Windows Admin Center 用户的设置。

  1. 卸载 Windows Admin Center
  2. 删除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下面的 **Server Management Experience** 文件夹
  3. 重新安装 Windows Admin Center

- 如果使工具保持打开状态，并且很长的一段时间内处于空闲状态时，你可能会收到多个**错误：不能用于此操作的运行空间状态**错误。 如果出现这种情况，请刷新浏览器。 如果遇到此操作，请[向我们发送反馈](http://aka.ms/WACfeedback)。

- 刷新 URL 较长的页面时，你可能会遇到 **500 错误**。 [12443710]

- 在某些工具中，浏览器的拼写检查器可能会将某些字段值标记为拼写错误。 [12425477]

- 在某些工具中，命令按钮可能无法在单击后立即反映状态更改，并且工具 UI 可能不会自动反映对某些属性所做的更改。 你可以单击**刷新**以从目标服务器中检索最新状态。 [11445790]

- 如果选择连接使用可多选复选框，然后筛选连接列表的标记筛选连接列表的标记，原选定内容仍然存在，因此你选择的任何操作将应用于所有以前选择的计算机。 [18099259]

- 可能有之间的操作系统中运行 Windows Admin Center 模块，什么第三方软件通知中列出的版本号的次要变体。

### <a name="extension-manager"></a>扩展管理器

- 更新 Windows Admin Center 时，必须重新安装你的扩展。
- 如果将扩展添加源无法访问，则不会发出警告。 [14412861]

## <a name="browser-specific-issues"></a>特定于浏览器的问题

### <a name="microsoft-edge"></a>Microsoft Edge

- 在某些情况下，使用 Microsoft Edge 通过 Internet 访问 Windows Admin Center 网关时，可能会遇到加载时间较长的问题。 在 Windows Admin Center 网关使用自签名证书的 Azure 虚拟机上可能会出现这种情况。 [13819912]

- 使用 Azure Active Directory 作为标识提供者并且使用自签名证书或其他不受信任的证书配置 Windows Admin Center 时，无法在 Microsoft Edge 中完成 AAD 身份验证。  [15968377]

- 如果你有作为服务部署到 Windows Admin Center 和 Microsoft Edge 使用作为你的浏览器，连接到 Azure 网关后生成新的浏览器窗口可能会失败。 尝试添加来解决此问题 https://login.microsoftonline.com ， https://login.live.com ，为你网关的 URL 是受信任的站点并允许客户端浏览器上的弹出窗口阻止程序设置的站点。 有关更多指导修复这[故障排除指南](troubleshooting.md#azure-features-dont-work-properly-in-edge)。 [17990376]

- 如果必须在桌面模式下安装 Windows Admin Center，在 Microsoft Edge 浏览器选项卡不会显示 favicon。 [17665801]

### <a name="google-chrome"></a>Google Chrome

- 之前的版本 70 （发布于 2018 年 10 月后期日） 有 Chrome [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609)有关 websockets 协议和 NTLM 身份验证。 这将影响以下工具：事件，PowerShell，通过远程桌面。

- Chrome 可能会弹出多个凭据提示，尤其是在**工作组**（非域）环境中添加连接体验期间。

- 如果必须作为服务部署到 Windows Admin Center，从网关 URL 的弹出窗口需要启用任何 Azure 集成功能工作。 这些服务包括 Azure 网络适配器、 Azure 更新管理和 Azure Site Recovery。

### <a name="mozilla-firefox"></a>Mozilla Firefox

未用 Mozilla Firefox 对 Windows Admin Center 进行测试，但大多数功能应该会正常工作。

- Windows 10 安装：Mozilla Firefox 有其自己的证书存储，因此您必须导入```Windows Admin Center Client```入 Firefox，若要在 Windows 10 上使用 Windows Admin Center 证书。

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>WebSocket 兼容性时使用的代理服务

Windows Admin Center 中的远程桌面、PowerShell 和事件模块使用 WebSocket 协议，使用代理服务时通常不支持该协议。 [预览版](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)内的 Azure AD 应用程序代理兼容性中提供了 WebSocket 支持，我们正在寻求有关兼容性的反馈。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>对 Windows Server 2016 (2012 R2、 2012、 2008 R2) 之前的版本的支持

> [!NOTE]
> Windows Admin Center 需要的 Windows Server 2012 R2、 2012年或 2008 R2 中不包括 PowerShell 功能。 如果将它们与 Windows Admin Center 管理 Windows Server，你将需要这些服务器上安装 WMF 5.1 或更高版本。

在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。

如果未安装，则可以[下载并安装 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="role-based-access-control-rbac"></a>基于角色的访问控制 (RBAC)

- 在配置为使用 Windows Defender 应用程序控制（WDAC，以前称为“代码完整性”）的计算机上，RBAC 部署将不会成功 [16568455]

- 若要在群集中使用 RBAC，必须将配置分别部署到每个成员节点。

- 部署 RBAC 后，你可能会遇到未授权错误，这些错误被错误地归因于 RBAC 配置。 [16369238]

## <a name="server-manager-solution"></a>服务器管理器解决方案

### <a name="server-settings"></a>服务器设置

- 如果修改了设置，然后尝试离开而不保存，页面将警告您未保存的更改，但继续离开。 可能最终状态不匹配的页的内容选择设置选项卡。 [19905798] [19905787]

### <a name="certificates"></a>证书

- 无法将 .PFX 加密证书导入到当前用户存储。 [11818622]

### <a name="devices"></a>设备

- 导航时通过具有键盘的表，所选内容可能会跳转到顶部的表组。 [16646059]

### <a name="events"></a>事件

- [使用代理服务时的 WebSocket 兼容性](#websocket-compatibility-when-using-a-proxy-service)会影响事件。

- 导出大日志文件时，你可能会遇到一个引用“数据包大小”的错误。 [16630279]

  - 若要解决此问题，请在网关计算机上提升的命令提示符中使用以下命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>文件

- 仍不支持上载或下载大文件。 (\~100 mb 的限制) [12524234]

### <a name="powershell"></a>PowerShell

- [使用代理服务时的 WebSocket 兼容性](#websocket-compatibility-when-using-a-proxy-service)会影响 PowerShell

- 像在桌面 PowerShell 控制台中那样通过一次右键单击进行粘贴不起作用。 系统会改为向你呈现浏览器的上下文菜单，你可以在其中选择“粘贴”。 Ctrl-V 也可以正常使用。

- 用 Ctrl-C 进行复制不起作用，系统始终将向控制台发送 Ctrl-C 中断命令。 右键单击上下文菜单中的“复制”可以正常使用。

- 缩小 Windows Admin Center 窗口时，终端内容将重新排列，但是再次将其放大时，内容可能不会恢复为之前的状态。 如果内容变得杂乱，你可以尝试使用 Clear-Host，或者使用终端上面的按钮断开连接并重新连接。

### <a name="registry-editor"></a>注册表编辑器

- 未实现搜索功能。 [13820009]

### <a name="remote-desktop"></a>远程桌面

- 远程桌面工具可能无法进行连接时管理 Windows Server 2012。 [20258278]

- 当使用远程桌面连接到未加入域的计算机，您必须输入你的帐户```MACHINENAME\USERNAME```格式。

- 某些配置可能会阻止 Windows Admin Center 远程桌面客户端使用组策略。 如果您遇到此问题，启用```Allow users to connect remotely by using Remote Desktop Services```下 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 远程桌面受[websocket 兼容性。](#websocket-compatibility-when-using-a-proxy-service)

- 远程桌面工具当前不支持在本地桌面和远程会话之间复制/粘贴任何文本、图像或文件。

- 要在远程会话中进行任何复制/粘贴，你可以正常复制（右键单击并选择“复制”或按 Ctrl+C），但粘贴时需要右键单击并选择“粘贴”（Ctrl+V 不起作用）

- 你无法将以下键命令发送到远程会话
  - Alt+Tab
  - 功能键
  - Windows 键
  - PrtScn

- 远程应用 – 启用远程桌面设置中的远程应用工具后该工具可能不会显示在工具列表管理具有桌面体验的服务器时。 [18906904]

### <a name="roles-and-features"></a>角色和功能

- 选择安装源不可用的角色或功能时，将跳过它们。 [12946914]

- 如果你选择在安装角色后不自动重新启动，我们将不会再次询问。 [13098852]

- 如果你选择自动重新启动，则将在状态更新为 100% 之前重新启动。 [13098852]

### <a name="storage"></a>存储

- 正在提取配额信息而无需错误通知可能会失败 （将仍有错误在浏览器的控制台） [18962274]

- 低级别：DVD/CD/软盘驱动器不显示为低级别上的卷。

- 低级别：卷和磁盘中的某些属性不是可用的低级别，因此它们出现未知或空白的详细信息面板中。

- 低级别：创建新卷时，ReFS Windows 2012 和 2012 R2 计算机上仅支持 64 K 分配单元大小。 如果在下层目标上使用较小的分配单元大小创建了 ReFS 卷，则文件系统格式化将失败。 新卷将无法使用。 解决方法是删除卷并使用 64K 分配单元大小。

### <a name="updates"></a>更新

- 安装更新之后，安装状态可能会被缓存，并需要刷新浏览器。

- 您可能会遇到错误："由键集不存在"时尝试设置 Azure 更新管理。 在这种情况下，请尝试以下更正步骤上托管的节点-
    1. 停止加密服务服务。
    2. 更改文件夹选项以显示隐藏文件 （如果需要）。
    3. 到达"%allusersprofile%\microsoft\crypto\rsa\s-1-5-18"文件夹并删除其所有内容。
    4. 重新启动加密服务服务。
    5. 重复使用 Windows Admin Center 更新管理设置

### <a name="virtual-machines"></a>虚拟机

- 在管理 Windows Server 2012 主机上的虚拟机，浏览器内虚拟机连接工具将无法连接到 VM。 下载要连接到 VM 的.rdp 文件应仍正常工作。 [20258278]

- Azure Site Recovery – 如果 ASR WAC，外部主机上的安装了您将不能保护从 VM 内 WAC [18972276]

- 目前不支持 Hyper-V 管理器中提供的高级功能，如虚拟 SAN 管理器、移动虚拟机、导出虚拟机、虚拟机复制。

### <a name="virtual-switches"></a>虚拟交换机

- 切换嵌入组合 (SET):将 Nic 添加到一个团队时, 必须位于同一子网。

## <a name="computer-management-solution"></a>计算机管理解决方案

计算机管理解决方案包含服务器管理器解决方案中的一部分工具，因此存在相同的已知问题，以及以下计算机管理解决方案特定问题：

- 如果您使用 Microsoft 帐户 ([MSA](https://account.microsoft.com/account/)) 或者使用 Azure Active Directory (AAD) 登录到你的 Windows 10 计算机，您必须指定"管理-为"的凭据来管理本地计算机 [16568455]

- 尝试管理 localhost 时，将提示你提升网关进程。 如果单击“用户帐户控制”弹出框中的**否**，Windows Admin Center 将无法再次显示它。 在这种情况下，通过右键单击系统托盘中的 Windows Admin Center 图标并选择“退出”以退出网关进程，然后从“开始”菜单重新启动 Windows Admin Center。

- 默认情况下，Windows 10 未开启 WinRM/PowerShell 远程处理
  
  - 要启用 Windows 10 客户端管理，必须利用提升的 PowerShell 提示符发出 ```Enable-PSRemoting``` 命令。

  - 你可能还需要通过 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 来更新防火墙以允许从本地子网外部的连接。 有关限制性更强的网络方案，请参阅[本文档](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## <a name="failover-cluster-manager-solution"></a>故障转移群集管理器解决方案

- 管理群集（是超聚合还是传统？）时可能会遇到**找不到 Shell** 错误。 如果发生这种情况，请重新加载浏览器，或导航到其他工具并返回。 [13882442]

- 管理尚未完全配置的下层（Windows Server 2012 或 2012 R2）群集时，可能会出现问题。 此问题的解决方法是确保已在群集的**每个成员节点**上安装并启用了 Windows 功能 **RSAT-Clustering-PowerShell**。 要使用 PowerShell 执行此操作，请在所有群集节点上输入命令 `Install-WindowsFeature -Name RSAT-Windows-PowerShell`。 [12524664]

- 可能需要添加群集与整个 FQDN 才能正确被发现。

- 使用安装为网关的 Windows Admin Center 连接到群集并提供显式用户名/密码进行身份验证时，必须选择 **Use these credentials for all connections** 以便可使用凭据查询成员节点。

## <a name="hyper-converged-cluster-manager-solution"></a>超聚合群集管理器解决方案

- 诸如**驱动器 - 更新固件**、**服务器 - 删除**和**卷 - 打开**之类的某些命令被禁用并且当前不受支持。