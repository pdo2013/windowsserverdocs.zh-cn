---
title: Windows Admin Center 已知问题
description: Windows Admin Center 已知问题 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: a579d0274ff4b53a72c17760a6d53ef796625d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356910"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知问题

> 适用于：Windows Admin Center、Windows Admin Center 预览版

如果遇到此页中未列出的问题，请[告诉我们](http://aka.ms/WACfeedback)。

## <a name="lenovo-xclarity-integrator"></a>联想 XClarity 集成器

以前泄漏的 XClarity 集成器扩展和 Windows 管理中心版本1904的不兼容性问题现已通过 Windows 管理中心版本1904.1 进行解析。 我们强烈建议你将更新为最新的受支持版本的 Windows 管理中心。

- 联想 XClarity 集成器扩展版本1.1 与 Windows 管理中心1904.1 完全兼容。 强烈建议您更新到最新版本的 Windows 管理中心和联想扩展。
- 出于任何原因，如果你需要继续使用 Windows 管理中心1809.5，你可以使用 XClarity 集成器1.0.4，在 windows 管理中心1809.5 不再受支持的情况下，将在 windows 管理中心我们的[支持策略](../support/index.md)。

## <a name="installer"></a>安装程序

- 使用你自己的证书安装 Windows Admin Center 时，请注意，如果你从证书管理器 MMC 工具中复制指纹，[指纹开头将包含无效字符。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 解决方法是键入指纹的第一个字符，然后复制/粘贴其余字符。

- 不支持使用低于1024的端口。 在服务模式下，你可以选择配置端口80以重定向到指定的端口。

- 如果 Windows 更新服务（wuauserv）已停止并处于禁用状态，则安装程序将失败。 [19100629]

### <a name="upgrade"></a>升级

- 从以前的版本升级服务模式下的 Windows 管理中心时，如果在安静模式下使用 msiexec，则可能会遇到 Windows 管理中心端口的入站防火墙规则被删除的问题。
  - 若要重新创建规则，请从已提升权限的 PowerShell 控制台中执行\<以下命令，并将端口 > 替换为 Windows 管理中心配置的端口（默认值为443）。

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>常规

- 如果你已在**windows Server 2016**上将 windows 管理中心作为网关安装，则该服务可能会崩溃，并在包含```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```的事件日志中出现错误。 这是由 Windows Server 2019 中已修复的 bug 引起的。 Windows Server 2016 的修补程序包含在 2 2019 月的累积更新（ [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)）。

- 如果已将 Windows 管理中心作为网关安装，且连接列表似乎已损坏，请执行以下步骤：

   > [!WARNING]
   >这会删除网关上所有 Windows 管理中心用户的连接列表和设置。

  1. 卸载 Windows Admin Center
  2. 删除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下面的 **Server Management Experience** 文件夹
  3. 重新安装 Windows Admin Center

- 如果将该工具保持打开状态且空闲一段时间，则可能会出现以下**错误：运行空间状态对于此操作**错误无效。 如果出现这种情况，请刷新浏览器。 如果遇到这种情况，请[向我们发送反馈](http://aka.ms/WACfeedback)。

- 刷新 URL 较长的页面时，你可能会遇到 **500 错误**。 [12443710]

- 在某些工具中，浏览器的拼写检查器可能会将某些字段值标记为拼写错误。 [12425477]

- 在某些工具中，命令按钮可能无法在单击后立即反映状态更改，并且工具 UI 可能不会自动反映对某些属性所做的更改。 你可以单击**刷新**以从目标服务器中检索最新状态。 [11445790]

- 对连接列表进行标记筛选-如果使用多选复选框选择连接，然后按标记筛选连接列表，原始选择将继续，因此，你选择的任何操作都将应用于之前选择的所有计算机。 [18099259]

- Windows 管理中心模块中运行的 OSS 版本号与第三方软件通知中列出的版本之间可能存在细微的差异。

### <a name="extension-manager"></a>扩展管理器

- 更新 Windows 管理中心时，必须重新安装扩展。
- 如果添加不可访问的扩展源，则不会出现警告。 [14412861]

## <a name="browser-specific-issues"></a>特定于浏览器的问题

### <a name="microsoft-edge"></a>Microsoft Edge

- 在某些情况下，使用 Microsoft Edge 通过 Internet 访问 Windows Admin Center 网关时，可能会遇到加载时间较长的问题。 在 Windows Admin Center 网关使用自签名证书的 Azure 虚拟机上可能会出现这种情况。 [13819912]

- 使用 Azure Active Directory 作为标识提供者并且使用自签名证书或其他不受信任的证书配置 Windows Admin Center 时，无法在 Microsoft Edge 中完成 AAD 身份验证。  [15968377]

- 如果已将 Windows 管理中心部署为服务，并使用 Microsoft Edge 作为浏览器，则在生成新的浏览器窗口后，将网关连接到 Azure 可能会失败。 尝试添加来解决此问题 https://login.microsoftonline.com ， https://login.live.com ，为你网关的 URL 是受信任的站点并允许客户端浏览器上的弹出窗口阻止程序设置的站点。 有关解决此问题的更多指导，请查看[故障排除指南](troubleshooting.md#azure-features-dont-work-properly-in-edge)。 [17990376]

- 如果已在桌面模式下安装 Windows 管理中心，Microsoft Edge 中的 "浏览器" 选项卡不会显示 favicon。 [17665801]

### <a name="google-chrome"></a>Google Chrome

- 早于70版（发布时间为10月版，2018）的 Chrome 有关于 websocket 协议和 NTLM 身份验证的[bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) 。 这会影响以下工具：事件、PowerShell、远程桌面。

- Chrome 可能会弹出多个凭据提示，尤其是在**工作组**（非域）环境中添加连接体验期间。

- 如果已将 Windows 管理中心部署为服务，则需要为要运行的任何 Azure 集成功能启用网关 URL 中的弹出窗口。 这些服务包括 Azure 网络适配器、Azure 更新管理和 Azure Site Recovery。

### <a name="mozilla-firefox"></a>Mozilla Firefox

未用 Mozilla Firefox 对 Windows Admin Center 进行测试，但大多数功能应该会正常工作。

- Windows 10 安装：Mozilla Firefox 具有自己的证书存储区，因此必须将该```Windows Admin Center Client```证书导入到 Firefox，才能在 windows 10 上使用 windows 管理中心。

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>使用代理服务时的 WebSocket 兼容性

Windows Admin Center 中的远程桌面、PowerShell 和事件模块使用 WebSocket 协议，使用代理服务时通常不支持该协议。 [预览版](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)内的 Azure AD 应用程序代理兼容性中提供了 WebSocket 支持，我们正在寻求有关兼容性的反馈。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>支持2016之前的 Windows Server 版本（2012 R2，2012，2008 R2）

> [!NOTE]
> Windows 管理中心需要未包含在 Windows Server 2012 R2、2012或 2008 R2 中的 PowerShell 功能。 如果你将通过 Windows 管理中心管理 Windows Server，你将需要在这些服务器上安装 WMF 版本5.1 或更高版本。

在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。

如果未安装，则可以[下载并安装 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="role-based-access-control-rbac"></a>基于角色的访问控制（RBAC）

- 在配置为使用 Windows Defender 应用程序控制（WDAC，以前称为“代码完整性”）的计算机上，RBAC 部署将不会成功 [16568455]

- 若要在群集中使用 RBAC，必须将配置分别部署到每个成员节点。

- 部署 RBAC 后，你可能会遇到未授权错误，这些错误被错误地归因于 RBAC 配置。 [16369238]

## <a name="server-manager-solution"></a>服务器管理器解决方案

### <a name="server-settings"></a>服务器设置

- 如果修改设置，则尝试在不保存的情况下导航离开，此页会警告你未保存的更改，但会继续离开。 最终可能会处于所选的 "设置" 选项卡与页面的内容不匹配的状态。 [19905798] [19905787]

### <a name="certificates"></a>证书

- 无法将 .PFX 加密证书导入到当前用户存储。 [11818622]

### <a name="devices"></a>设备

- 使用键盘在表中导航时，所选内容可能跳到表组的顶部。 [16646059]

### <a name="events"></a>Events

- [使用代理服务时的 WebSocket 兼容性](#websocket-compatibility-when-using-a-proxy-service)会影响事件。

- 导出大日志文件时，你可能会遇到一个引用“数据包大小”的错误。 [16630279]

  - 若要解决此问题，请在网关计算机上的权限提升的命令提示符中使用以下命令：```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>文件

- 仍不支持上载或下载大文件。 （\~100mb 限制）[12524234]

### <a name="powershell"></a>PowerShell

- [使用代理服务时的 WebSocket 兼容性](#websocket-compatibility-when-using-a-proxy-service)会影响 PowerShell

- 像在桌面 PowerShell 控制台中那样通过一次右键单击进行粘贴不起作用。 系统会改为向你呈现浏览器的上下文菜单，你可以在其中选择“粘贴”。 Ctrl-V 也可以正常使用。

- 用 Ctrl-C 进行复制不起作用，系统始终将向控制台发送 Ctrl-C 中断命令。 右键单击上下文菜单中的“复制”可以正常使用。

- 缩小 Windows Admin Center 窗口时，终端内容将重新排列，但是再次将其放大时，内容可能不会恢复为之前的状态。 如果内容变得杂乱，你可以尝试使用 Clear-Host，或者使用终端上面的按钮断开连接并重新连接。

### <a name="registry-editor"></a>注册表编辑器

- 未实现搜索功能。 [13820009]

### <a name="remote-desktop"></a>远程桌面

- 在管理 Windows Server 2012 时，"远程桌面" 工具可能无法连接。 [20258278]

- 使用远程桌面连接到未加入域的计算机时，你必须以格式输入你的```MACHINENAME\USERNAME```帐户。

- 某些配置可以通过组策略阻止 Windows 管理中心的远程桌面客户端。 如果遇到这种情况， ```Allow users to connect remotely by using Remote Desktop Services```请在```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 远程桌面受[websocket 兼容性影响。](#websocket-compatibility-when-using-a-proxy-service)

- 远程桌面工具当前不支持在本地桌面和远程会话之间复制/粘贴任何文本、图像或文件。

- 要在远程会话中进行任何复制/粘贴，你可以正常复制（右键单击并选择“复制”或按 Ctrl+C），但粘贴时需要右键单击并选择“粘贴”（Ctrl+V 不起作用）

- 你无法将以下键命令发送到远程会话
  - Alt+Tab
  - 功能键
  - Windows 键
  - PrtScn

- 远程应用–从远程桌面设置启用远程应用工具之后，在管理具有桌面体验的服务器时，该工具可能不会显示在工具列表中。 [18906904]

### <a name="roles-and-features"></a>角色和功能

- 选择安装源不可用的角色或功能时，将跳过它们。 [12946914]

- 如果你选择在安装角色后不自动重新启动，我们将不会再次询问。 [13098852]

- 如果你选择自动重新启动，则将在状态更新为 100% 之前重新启动。 [13098852]

### <a name="storage"></a>存储

- 提取配额信息可能在没有错误通知的情况下失败（浏览器的控制台中仍有错误） [18962274]

- 下级：DVD/CD/软盘驱动器不会显示为下层的卷。

- 下级：卷和磁盘中的某些属性不会在 "详细信息" 面板中显示为 "未知" 或 "空白"。

- 下级：创建新卷时，ReFS 仅支持 Windows 2012 和 2012 R2 计算机上的 64 k 分配单元大小。 如果在下层目标上使用较小的分配单元大小创建了 ReFS 卷，则文件系统格式化将失败。 新卷将无法使用。 解决方法是删除卷并使用 64K 分配单元大小。

### <a name="updates"></a>更新

- 安装更新后，可能会缓存安装状态并需要浏览器进行刷新。

- 你可能会遇到以下错误：尝试设置 Azure 更新管理时出现 "密钥集不存在"。 在这种情况下，请在托管节点上尝试以下更正步骤-
    1. 停止 "加密服务" 服务。
    2. 更改文件夹选项以显示隐藏文件（如果需要）。
    3. 已到达 "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" 文件夹并删除其所有内容。
    4. 重新启动 "加密服务" 服务。
    5. 重复设置 Windows 管理中心更新管理

### <a name="virtual-machines"></a>虚拟机

- 在 Windows Server 2012 主机上管理虚拟机时，浏览器内 VM 连接工具将无法连接到 VM。 下载用于连接到 VM 的 .rdp 文件应该仍然有效。 [20258278]

- Azure Site Recovery –如果 ASR 是在 WAC 之外的主机上设置的，则你将无法在 WAC 中保护 VM [18972276]

- 目前不支持 Hyper-V 管理器中提供的高级功能，如虚拟 SAN 管理器、移动虚拟机、导出虚拟机、虚拟机复制。

### <a name="virtual-switches"></a>虚拟交换机

- 交换机嵌入组合（SET）：将 Nic 添加到团队时，它们必须位于同一子网中。

## <a name="computer-management-solution"></a>计算机管理解决方案

计算机管理解决方案包含服务器管理器解决方案中的一部分工具，因此存在相同的已知问题，以及以下计算机管理解决方案特定问题：

- 如果使用的是 Microsoft 帐户（[MSA](https://account.microsoft.com/account/)），或者使用 AZURE ACTIVE DIRECTORY （AAD）登录到 Windows 10 计算机，则必须指定 "管理身份" 凭据来管理本地计算机 [16568455]

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