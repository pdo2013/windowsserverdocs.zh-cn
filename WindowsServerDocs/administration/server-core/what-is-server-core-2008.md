---
title: 什么是 Server Core 2008？
description: 了解 Windows Server 2008 中的服务器核心安装选项
ms.prod: windows-server
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 0a109d0bfc4fc09b5e8097059d68b728d17752a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383379"
---
# <a name="what-is-server-core-2008"></a>什么是 Server Core 2008？
>适用于：Windows Server 2008

>[!NOTE]
>此信息适用于 Windows Server 2008。 有关 Windows Server 中的服务器核心的信息，请参阅[什么是 Windows server 中的服务器核心安装](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core)。 

"服务器核心" 选项是在部署 Windows Server 2008 的 Standard edition、Enterprise edition 或 Datacenter edition 时可用的新的最小安装选项。 服务器核心提供仅支持安装特定服务器角色的 Windows Server 2008 的最小安装，如本章稍后部分所述。 与 Windows Server 2008 的完全安装选项相比，此选项支持安装所有可用的服务器角色以及其他 Microsoft 或第三方服务器应用程序，如 Microsoft Exchange Server 或 SAP。 

在继续之前，需要解释短语 "安装选项"。 通常情况下，当你购买 Windows Server 2008 的副本时，你购买的许可证使用特定版本或库存单位（Sku）。 表1-1 列出了可用的各种版本的 Windows Server 2008。 该表还指明了每个版本都有哪些安装选项（完整、服务器核心或两者）。

**表 1-1**Windows Server 2008 版本及其对安装选项的支持

| 版本       | 完全          | 服务器核心  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard （x86 和 x64）       | X | X        |
| Windows Server 2008 企业版（x86 和 x64）       | X | X        |
| Windows Server 2008 Datacenter （x86 和 x64）        | X | X       |
| Windows Web 服务器2008（x86 和 x64）       | X | X  |
| 用于基于 Itanium 的系统的 Windows Server 2008       | X |     |
| Windows HPC Server 2008 （仅 x64）       | X |   |
| 不带 Hyper-v 的 Windows Server 2008 Standard （x86 和 x64） | X | X |
| 不带 Hyper-v 的 Windows Server 2008 企业版（x86 和 x64）  | X | X |
| 不带 Hyper-v 的 Windows Server 2008 Standard （x86 和 x64） | X | X |

要了解什么是 "安装选项"，假设你购买了允许安装 Windows Server 2008 Enterprise Edition 副本的批量许可证。 将批量许可媒体插入系统并开始安装过程时，你将看到一个屏幕，如图1-1 所示，为你提供了版本和安装选项的选择。

![选择要安装的服务器核心安装选项](../media/what-is-server-core-2008/FIg1-1.png)

**图 1-1**选择要安装的服务器核心安装选项

在图1-1 中，你的批量许可（或产品密钥，适用于零售媒体）提供了两个可供选择的安装选项：第二个选项（Windows Server 2008 Enterprise 的完整安装）和第五个选项（Windows 的服务器核心安装服务器2008企业版），在此示例中选择后者。 

## <a name="full-vs-server-core"></a>完全与服务器核心 
自 Microsoft Windows 平台早期以来，Windows server 本质上是 "所有内容" 服务器，其中包含所有类型的功能，其中一些功能在你的网络环境中永远不会实际使用。 例如，当你在系统上安装 Windows Server 2003 时，将在你的服务器上安装路由和远程访问服务（RRAS）的二进制文件，即使你不需要此服务也是如此（尽管你仍必须配置并启用 RRAS，然后才能正常运行）。 Windows Server 2008 仅在您选择在服务器上安装该特定角色时，通过安装服务器角色所需的二进制文件来改善早期版本。 但是，Windows Server 2008 的完全安装选项仍将安装许多服务和其他组件，这些组件通常不需要特定的使用方案。 

这就是 Microsoft 创建了第二个安装选项（服务器核心，适用于 Windows Server 2008）的原因：为了消除对某些常用服务器角色的支持不重要的任何服务和其他功能。 例如，域名系统（DNS）服务器实际上不需要安装 Windows Internet Explorer，因为出于安全原因，你不想从 DNS 服务器浏览 Web。 并且 DNS 服务器甚至不需要图形用户界面（GUI），因为你可以通过命令行使用强大的 Dnscmd 命令来管理几乎所有方面的 DNS，或使用 DNS Microsoft 管理控制台（MMC）管理单元远程管理。

为避免出现这种情况，Microsoft 决定从 Windows Server 2008 中去除所有内容，这些都是运行核心网络服务（如 Active Directory 域服务（AD DS）、DNS、动态主机配置协议（DHCP）、文件和打印以及其他一些服务器角色。 结果为 "新服务器核心" 安装选项，该选项可用于创建仅支持有限数量的角色和功能的服务器。 

## <a name="the-server-core-gui"></a>服务器核心 GUI
在系统上完成安装 Server Core 并首次登录时，你会感到惊讶。 图1-2 显示第一次登录后的服务器核心用户界面。

![服务器核心用户界面](../media/what-is-server-core-2008/Fig1-2.png)

**图 1-2**服务器核心用户界面

没有桌面！ 也就是说，没有 Windows 资源管理器 shell，其 "开始" 菜单、任务栏以及您可能会看到的其他功能。 你只是一个命令提示符，这意味着，你必须使用一次键入一个命令（速度较慢，或使用脚本和批处理文件）来配置服务器核心安装，这可以帮助你加速和简化 configurati通过自动执行任务。 你还可以在执行服务器核心的无人参与安装时使用应答文件来执行某些初始配置任务。 

对于在使用诸如 dism.exe、Dfscmd 和 Dnscmd 之类的命令行工具的专家，使用命令行工具的管理员，配置和管理服务器核心安装可能很容易，甚至很有趣。 但对于不是专家的用户，所有这些都不会丢失。 你仍可以使用标准 Windows Server 2008 MMC 工具来管理服务器核心安装。 你只需在运行完整安装的 Windows Server 2008 或带有 Service Pack 1 的 Windows Vista 的其他系统上使用它们。 

你将在本书的第3章到第6章中了解有关配置和管理服务器核心安装的详细信息，但后面的章节介绍如何管理特定服务器角色和其他组件。 若要了解有关各种 Windows 命令行工具以及如何使用它们的详细信息，请参阅以下两个有用资源：
* Windows Server 2008 技术库（）的 "命令参考" 部分 
* *Windows 命令行管理员的 Pocket 顾问*By William Stanek （Microsoft 按，2008） 

表1-2 列出了主要的 GUI 应用程序及其可执行文件，它们在服务器核心安装中提供。

**表 1-2**服务器核心安装中提供的 GUI 应用程序

| GUI 应用程序 | 路径为的可执行文件 |
| -------------   | -------------       | 
| 命令提示符 | %WINDIR%\System32\Cmd.exe |
| Microsoft 支持部门诊断工具 | %WINDIR%\System32\MSdt.exe |
| 记事本 | %WINDIR%\System32\Notepad.exe |
| 注册表编辑器 | %WINDIR%\System32\Regedt32.exe |
| 系统信息 | %WINDIR%\System32\MSinfo32.exe |
| 任务管理器 | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

这是一个非常简短的列表！ 下面列出了未包含在服务器核心中的用户界面元素：
* Windows 资源管理器桌面 shell （资源管理器）和任何支持功能，如主题 
* 所有 MMC 控制台 
* 除区域和语言选项（国际 cpl）以及日期和时间（Timedate.cpl）之外的所有控制面板实用工具 
* 所有超文本标记语言（HTML）渲染引擎，包括 Internet Explorer 和 HTML 帮助 
* Windows Mail 
* Windows Media Player 
* 大多数附件，如画图、计算器和 Wordpad

服务器核心中也不提供 .NET Framework，这意味着不支持在服务器核心安装上运行托管代码。 只有本机代码（使用 Windows 应用程序编程接口（Api）编写的代码）可以在服务器核心上运行。 总而言之，依赖于 .NET Framework 或资源管理器的任何 GUI 应用程序都不会在服务器核心上运行。 

>[!NOTE]
>由于 Windows PowerShell 需要 .NET Framework，因此不能在 Server Core 上安装 Windows PowerShell。 不过，只要只使用 PowerShell WMI 命令，就可以使用 Windows PowerShell 远程管理服务器核心安装。

## <a name="supported-server-roles"></a>支持的服务器角色 
与完全安装的 Windows Server 2008 相比，服务器核心安装仅包含有限数量的服务器角色。 表1-3 比较了可用于 Windows Server 2008 Enterprise Edition 的完全安装和服务器核心安装的角色。 

**表 1-3**服务器角色与 Windows Server 2008 企业版完全安装和服务器核心安装的比较

| 服务器角色  | 完全安装中提供  | 在服务器核心中提供  |
| ------------- | :-------------: | :------------: |
| Active Directory 证书服务 (AD CS)  | X |  |
| Active Directory 域服务 (AD DS)  | X  | X |
| Active Directory 联合身份验证服务 (AD FS)  | X  |  |
| Active Directory 轻型目录服务 (AD LDS)  | X  | X |
| Active Directory 权限管理服务 (AD RMS)  | X  |  |
| 应用程序服务器  | X  |  |
| DHCP 服务器  | X  | X |
| DNS 服务器  | X  | X |
| 传真服务器  | X  |  |
| 文件服务  | X  | X |
| Hyper-V  | X | X |
| 网络策略和访问服务  | X  |  |
| 打印服务  | X  | X |
| 流式媒体服务  | X  | X |
| 终端服务  | X  |  |
| UDDI 服务  | X  |  |
| Web 服务器 (IIS) | X | X |
| Windows 部署服务  | X |  |

虽然对于服务器核心可用的角色通常是相同的，但不考虑体系结构（x86 或 x64）和产品版本，但有几个例外：
* 仅当使用 Hyper-v 产品媒体购买 Windows Server 2008 （Hyper-v 仅适用于 x64 版本）时，Hyper-v （虚拟化）角色才可用。 如果不需要此角色，则可以购买不带 Hyper-v 产品媒体的 Windows Server 2008。 
* 标准版中的文件服务角色仅限一个独立的分布式文件系统（DFS）根，不支持跨文件复制（DFS-R）。 
* 在服务器核心上安装流媒体服务角色之前，需要从 Microsoft 下载中心下载并安装服务器体系结构（x86 或 x64）的相应 Microsoft 更新独立包（.msu 文件）。
* Web 服务器（IIS）角色不支持 ASP.NET。 这是因为服务器核心不支持 .NET Framework，这会限制你可以对服务器核心 Web 服务器执行的操作。 

## <a name="supported-optional-features"></a>支持的可选功能
在完整安装的 Windows Server 2008 上，服务器核心安装还仅支持有限的功能子集。 表1-4 对 Windows Server 2008 Enterprise Edition 的完全安装和服务器核心安装的可用功能进行了比较。

**表 1-4**Windows Server 2008 企业版完全安装和服务器核心安装的功能比较

| 功能  | 完全安装中提供  | 在服务器核心中提供  |
| ------------- | :-------------: | :------------: |
| .NET Framework 3.0 功能  | X  |  |
| BitLocker 驱动器加密  | X  | X |
| BITS 服务器扩展  | X  |  |
| 连接管理器管理工具包  | X |  |
| 桌面体验  | X |  |
| 故障转移群集  | X  | X |
| 组策略管理  | X  |  |
| Internet 打印客户端  | X  |  |
| Internet 存储名称服务器  | X  |  |
| LPR 端口监视器  | X  |  |
| 消息队列  | X  |  |
| 多路径 IO  | X  | X |
| 网络负载平衡  | X  | X |
| 对等名称解析协议  | X  |  |
| 高质量 Windows 音频视频体验  | X |  |
| 远程协助  | X  |  |
| 远程差分压缩 | X  |  |
| 远程服务器管理工具  | X  |  |
| 可移动存储管理器 | X  | X |
| HTTP 代理上的 RPC | X  |  |
| 简单 TCP/IP 服务  | X  |  |
| SMTP 服务器  | X  |  |
| SMNP 服务  | X  | X  |
| San 存储管理器  | X  |  |
| 基于 UNIX 的应用程序的子系统 | X | X  |
| Telnet 客户端  | X | X  |
| Telnet 服务器  | X   |  |
| TFTP 客户端  | X   |  |
| Windows 内部数据库  | X  |  |
| Windows PowerShell  | X  |  |
| Windows 产品激活服务  | X   |  |
| Windows Server 备份功能  | X  | X  |
| Windows 系统资源管理器  | X  |  |
| WINS 服务器  | X | X |
| 无线 LAN 服务 | X  |  |

同样，您需要了解有关服务器核心上可用功能的一些要点：
* 某些功能可能需要特殊硬件才能在服务器核心上正常运行。 这些功能包括 BitLocker 驱动器加密、故障转移群集、多路径 IO、网络负载平衡和可移动存储。 
* 标准版上不提供故障转移群集。

## <a name="server-core-architecture"></a>服务器核心体系结构
深入更深入地了解了 Server Core，接下来让我们简单了解一下 Windows Server 2008 的服务器核心安装的体系结构，并将其与完全安装的体系结构进行比较。 首先，请记住，服务器核心不是不同版本的 Windows Server 2008，而只是安装选项，可以在将 Windows Server 2008 安装到系统时选择此选项。 这表示：
* 服务器核心安装上的内核在相同硬件体系结构（x86 或 x64）和版本的完全安装上找到的同一个。 
* 如果某个二进制文件存在于服务器核心安装中，则相同硬件体系结构（x86 或 x64）和版本的完全安装将具有该特定二进制文件的相同版本（稍后将介绍两个例外）。 
* 如果特定的设置（例如，特定的防火墙例外或特定服务的启动类型）在服务器核心安装上具有特定的默认配置，则该设置的配置方式完全相同。硬件体系结构（x86 或 x64）和版本。

图1-3 显示了 Windows Server 2008 的完全安装和服务器核心安装的体系结构的简化视图。 点线指示服务器核心的结构，而整个关系图表示完全安装的体系结构。 

此图说明了 Windows Server 2008 的模块化体系结构，其中的服务器核心是在核心操作系统功能的子集上构建的。 对于相同的硬件体系结构和版本，全新安装的服务器核心上还提供了每个文件，但有两个特殊文件（.wsf 和了 oclist.exe），只存在于服务器核心上。 这些特殊文件包含在服务器核心上，用于简化服务器核心安装的初始配置，以及添加或删除角色和可选组件。 

![服务器核心和完整安装的体系结构](../media/what-is-server-core-2008/Fig1-3.png)

**图 1-3**服务器核心和完整安装的体系结构

## <a name="driver-support"></a>驱动程序支持
图1-3 中所示的服务器核心的体系结构示意图明显简化;它不显示的一个问题就是服务器核心和完全安装之间设备驱动程序支持之间的差异。 Windows Server 2008 的完整安装包含适用于不同类型设备的数千个内置驱动程序，使你能够在各种不同的硬件配置上安装产品。 （客户端操作系统（如 Windows Vista）包含更多驱动程序，以支持通常不与服务器一起使用的设备，如数字照相机和扫描仪。 

如果新设备连接到（或安装在中） Windows Server 2008 的完整安装，则即插即用（PnP）子系统首先检查是否存在设备的内置驱动程序。 如果找到兼容的内置驱动程序，PnP 子系统将自动安装驱动程序，然后设备将进行操作。 在完整安装的 Windows Server 2008 上，可能会显示气球弹出通知，指示该驱动程序已安装并且设备可供使用。 

在服务器核心安装中，驱动程序安装过程是相同的（在服务器核心上存在 PnP 子系统），具有两个资格。 首先，服务器核心只包含最少的内置驱动程序，且仅包含以下类型的设备：
* 标准视频图形阵列（VGA）视频驱动程序 
* 用于存储设备的驱动程序 
* 网络适配器的驱动程序

请注意，对于此处显示的三个设备类别中的每一个，服务器核心都包含在相应的完全安装上找到的相同内置驱动程序（适用于同一硬件体系结构）。 

此外，当 PnP 子系统自动安装新设备的驱动程序时，它会以静默方式执行此操作，而不会显示气球弹出通知。 为什么不呢？ 因为服务器核心上没有 GUI，所以任务栏上没有通知区域！ 

那么，当你将打印服务角色添加到服务器核心安装时，你要执行什么操作，并且你想要安装打印机？ 手动将打印机驱动程序添加到服务器-服务器核心没有内置的打印驱动程序。

## <a name="service-footprint"></a>服务占用
由于服务器核心是一个最小安装，因此，它比相同硬件体系结构和版本的对应完全安装的系统服务占用空间更小。 例如，默认情况下，大约为75的系统服务安装在 Windows Server 2008 的完全安装上，其50大约配置为自动启动。 与此相反，服务器核心在默认情况下仅安装了70个服务，且这些服务的启动时间少于40个。 

表1-5 列出了在服务器核心安装上默认安装的服务，以及每个服务使用的和帐户的启动模式。

**表 1-5**默认情况下，在 Server Core 上安装的系统服务

| 服务名称  | Display name  | 启动模式  | 帐户  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | 应用程序体验  | 自动 | LocalSystem |
| AppMgmt  | 应用程序管理  | Manual | LocalSystem |
| BFE | 基本筛选引擎  | 自动 | LocalService |
| BITS | 后台智能传送服务  | 自动 | LocalSystem |
| Browser | 计算机浏览器  | Manual | LocalSystem |
| CertPropSvc | 证书传播  | Manual | LocalSystem |
| COMSysApp  | COM+ 系统应用程序  | Manual | LocalSystem |
| CryptSvc  | 加密服务  | 自动 | 网络服务 |
| DcomLaunch  | DCOM 服务器进程启动器  | 自动 | LocalSystem |
| Dhcp  | DHCP 客户端  | 自动 | LocalService |
| Dnscache | DNS 客户端的运行情况  | 自动 | 网络服务 |
| DPS  | 诊断策略服务  | 自动 | LocalService |
| 事件日志 | Windows 事件日志  | 自动 | LocalService |
| EventSystem  | COM+ 事件系统  | 自动 | LocalService |
| FCRegSvc  | Microsoft 光纤通道平台注册服务  | Manual | LocalService |
| gpsvc  | 组策略客户端  | 自动 | LocalSystem |
| hidserv | 人体学接口设备访问  | Manual | LocalSystem |
| hkmsvc  | 运行状况密钥和证书管理  | Manual | LocalSystem |
| IKEEXT  | IKE 和 AuthIP IPsec 密钥模块  | 自动 | LocalSystem |
| iphlpsvc  | IP 帮助程序  | 自动 | LocalSystem |
| KeyIso | CNG 密钥隔离  | Manual | LocalSystem |
| KtmRm  | 适用于分布式事务处理协调器的 KtmRm  | 自动 | 网络服务 |
| LanmanServer  | Server  | 自动 | LocalSystem |
| LanmanWorkstation  | Workstatione  | 自动 | LocalService |
| lltdsvc  | 链路层拓扑发现映射器  | Manual | LocalService |
| lmhosts  | TCP/IP NetBIOS 帮助程序  | 自动 | LocalService |
| MpsSvc  | Windows 防火墙  | 自动 | LocalService |
| MSDTC  | 分布式事务处理协调器  | 自动 | 网络服务 |
| MSiSCSI  | Microsoft iSCSI 发起程序服务  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | 网络访问保护代理  | Manual | 网络服务 |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | 网络列表服务  | 自动 | LocalService |
| NlaSvc  | 网络位置感知  | 自动 | 网络服务 |
| nsi  | 网络存储接口服务  | 自动 | LocalService |
| pla  | 性能日志和警报  | Manual | LocalService |
| PlugPlay  | 即插即用  | 自动 | LocalSystem |
| PolicyAgent  | IPsec 策略代理  | 自动 | 网络服务 |
| ProfSvc  | 用户配置文件服务  | 自动 | LocalSystem |
| ProtectedStorage  | 受保护的存储  | Manual | LocalSystem |
| RemoteRegistry  | 远程注册表  | 自动 | LocalService |
| RpcSs  | 远程过程调用 (RPC)  | 自动 | 网络服务 |
| RSoPProv | 策略提供程序的结果集  | Manual | LocalSystem |
| sacsvr  | 特殊管理控制台帮助程序  | Manual | LocalSystem |
| SamSs  | 安全帐户管理器  | 自动 | LocalSystem |
| SCardSvr | 智能卡  | Manual | LocalService |
| 计划 | 任务计划程序  | 自动 | LocalSystem |
| SCPolicySvc | 智能卡移除策略  | Manual | LocalSystem |
| seclogon | 辅助登录  | 自动 | LocalSystem |
| SENS | 系统事件通知服务  | 自动 | LocalSystem |
| SessionEnv | 终端服务配置  | Manual | LocalSystem |
| slsvc  | 软件授权 | 自动 | 网络服务 |
| SNMPTRAP  | SNMP 陷阱  | Manual | LocalService |
| swprv  | Microsoft 软件卷影复制提供程序 | Manual | LocalSystem |
| TBS | TPM 基本服务  | Manual | LocalService |
| TermService  | 终端服务 | 自动 | 网络服务 |
| TrustedInstaller | Windows 模块安装程序  | 自动 | LocalSystem |
| UmRdpService | 终端服务 UserMode 端口重定向程序  | Manual | LocalSystem |
| vds | 虚拟磁盘  | Manual | LocalSystem |
| VSS | 卷影复制  | Manual | LocalSystem |
| W32Time | Windows 时间  | 自动 | LocalService |
| WcsPlugInService  | Windows 颜色系统  | Manual | LocalService |
| WdiServiceHost  | 诊断服务主机  | Manual | LocalService |
| WdiSystemHost  | 诊断系统主机  | Manual | LocalSystem |
| Wecsvc | Windows 事件收集器  | Manual | 网络服务 |
| WinHttpAuto-ProxySvc  | WinHTTP Web 代理自动发现服务  | 自动 | LocalService |
| Winmgmt | Windows Management Instrumentation | 自动 | LocalSystem |
| WinRM  | Windows 远程管理 (WS-Management) | 自动 | 网络服务 |
| wmiApSrv  | WMI 性能适配器  | Manual | LocalSystem |
| wuauserv | Windows 更新 | 自动 | LocalSystem |