---
title: 什么是 Server Core 2008？
description: 了解如何在 Windows Server 2008 的服务器核心安装选项
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: c1ef71dbc589cfdeac63b46d720c4bdd0a44dbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815398"
---
#<a name="what-is-server-core-2008"></a>什么是 Server Core 2008？
>适用于：Windows Server 2008

>[!NOTE]
>此信息适用于 Windows Server 2008。 有关 Windows Server 中的 Server Core 的信息，请参阅[什么是 Windows Server 中的 Server Core 安装](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core)。 

服务器核心选项是新的最小安装选项，当您要部署 Windows Server 2008 Standard、 Enterprise 或 Datacenter edition 时才可用。 服务器核心提供支持将安装仅某些服务器角色的 Windows Server 2008 的最小安装更高版本中这一章所述。 与之相比，Windows Server 2008，支持将安装所有可用的服务器角色还其他 Microsoft 或第三方服务器应用程序，如 Microsoft Exchange Server 或 SAP 的完全安装选项。 

在继续之前，短语"安装选项"要求加以解释。 通常情况下，当你购买一份 Windows Server 2008，你购买的许可证可以使用特定版本或库存单位 (Sku)。 表 1-1 列出了可用的各种 Windows Server 2008 版本。 此表还指示了每个版本可用 （完整、 服务器核心或两者） 的安装选项。

**表 1-1** Windows Server 2008 版本和它们对安装选项的支持
| 版本       | 完全          | 服务器核心  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 标准版 （x86 和 x64）       | X | X        |
| Windows Server 2008 企业版 （x86 和 x64）       | X | X        |
| Windows Server 2008 Datacenter （x86 和 x64）        | X | X       |
| Windows Web Server 2008 （x86 和 x64）       | X | X  |
| Windows Server 2008 For itanium-based systems       | X |     |
| Windows HPC Server 2008 (仅限 x64)       | X |   |
| Windows Server 2008 标准版不带 HYPER-V （x86 和 x64） | X | X |
| 不带 HYPER-V （x86 和 x64） 的 Windows Server 2008 企业版  | X | X |
| Windows Server 2008 标准版不带 HYPER-V （x86 和 x64） | X | X |

若要了解什么"安装选项"是，请让我们假设你已购买批量许可证，可用于安装 Windows Server 2008 Enterprise Edition 的副本。 您将在批量许可的媒体插入到系统并开始安装过程中，您将看到的屏幕之一所示在图 1-1，显示您使用选择的版本和安装选项。

![选择要安装的服务器核心安装选项](../media/what-is-server-core-2008/FIg1-1.png)

**图 1-1**选择要安装的服务器核心安装选项

在图 1-1，您的批量许可证 （或产品密钥，零售媒体） 提供可以选择之间的两个安装选项： 第二个选项 (完整安装的 Windows Server 2008 Enterprise) 和第五个选项 (服务器核心安装的 WindowsServer 2008 企业版），后者选择在此示例中。 

##<a name="full-vs-server-core"></a>完全与。服务器核心 
Microsoft Windows 平台的早期阶段，Windows 服务器都实质上是"everything"包括所有类型的功能，其中一些您可能永远不会实际在环境中使用网络的服务器。 例如，在系统上安装 Windows Server 2003，路由和远程访问服务 (RRAS) 的二进制文件已在服务器上安装即使你有此服务不需要 （尽管你仍必须配置和启用 RRAS 并将它将开始工作前）。 Windows Server 2008 改进了通过安装所需的服务器角色，仅当你选择要在服务器上安装该特定角色的二进制文件的早期版本。 但是，Windows Server 2008 的完全安装选项仍然会安装许多服务和特定使用方案通常不需要其他组件。 

这是 Microsoft 创建了第二个安装选项的原因-服务器核心-对于 Windows Server 2008： 若要消除任何服务和其他功能的支持的某些常用的服务器角色不重要。 例如，真正的域名系统 (DNS) 服务器不需要安装它，因为您可能不希望浏览来自 DNS 服务器出于安全原因 Web 的 Windows Internet Explorer。 和 DNS 服务器甚至不需要图形用户界面 (GUI)，因为你可以管理 DNS 的几乎所有方面从命令行使用功能强大的 Dnscmd.exe 命令，或者远程使用 DNS Microsoft 管理控制台 (MMC) 管理单元中。

若要避免此问题，Microsoft 决定去除从已运行核心网络服务，如 Active Directory 域服务 (AD DS)、 DNS、 动态主机配置协议 (DHCP)、 文件和打印，绝对必要的 Windows Server 2008 的所有内容和几个其他服务器角色。 结果是可用于创建支持有限的数量的角色和功能的服务器的新服务器核心安装选项。 

##<a name="the-server-core-gui"></a>在 Server Core GUI
完成服务器核心上安装系统和登录第一次后，您在为一点惊喜。 图 1-2 后第一次登录显示了服务器核心用户界面。

![服务器核心用户界面](../media/what-is-server-core-2008/Fig1-2.png)

**图 1-2**服务器核心用户界面

没有任何桌面 ！ 也就是说，没有任何 Windows 资源管理器外壳，其开始菜单、 任务栏中，与你可能习惯于看到的其他功能。 您必须是工作的命令提示符下，这意味着你可以通过键入命令一次，其速度较慢，或使用脚本和批处理文件，它可帮助你加快和简化你的配置进行大部分来配置服务器核心安装通过自动执行这些任务。 此外可以执行一些初始配置任务在执行无人参与的服务器核心安装时使用应答文件。 

对于许多专家在使用 Netsh.exe、 Dfscmd.exe，等 Dnscmd.exe 命令行工具的管理员，配置和管理 Server Core 安装可以是简单、 甚至很有趣。 对于那些不是专家，但是，并非毫无希望。 仍可用于标准的 Windows Server 2008 MMC 工具管理服务器核心安装。 只需在运行带有 Service Pack 1 的是完整安装的 Windows Server 2008 或 Windows Vista 的不同系统上使用它们。 

您将了解有关配置和管理服务器核心安装中的章节 3 到 6 的这本书，而后面的章节讨论如何管理特定服务器角色和其他组件的详细信息。 若要了解有关各种 Windows 命令行工具以及如何使用它们的详细信息，有两个很好的资源进行协商：
* Windows Server 2008 技术库 （） 的命令参考部分 
* *Windows 命令行管理员随身顾问*William R.stanek 撰写 (Microsoft Press，2008年) 的 

表 1-2 列出了主要的 GUI 应用程序，以及可执行文件，在服务器核心安装中可用。

**表 1-2** GUI 应用程序在服务器核心安装中可用
| GUI 应用程序 | 可执行文件路径 |
| -------------   | -------------       | 
| 命令提示符 | %WINDIR%\System32\Cmd.exe |
| Microsoft 支持诊断工具 | %WINDIR%\System32\MSdt.exe |
| 记事本 | %WINDIR%\System32\Notepad.exe |
| 注册表编辑器 | %WINDIR%\System32\Regedt32.exe |
| 系统信息 | %WINDIR%\System32\MSinfo32.exe |
| 任务管理器 | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

这是一个漂亮的简短列表 ！ 现在这里是不包括 Server Core 中的用户界面元素的列表：
* Windows 资源管理器桌面外壳程序 (Explorer.exe) 和任何支持的功能，例如主题 
* 所有 MMC 控制台 
* 所有控制面板实用程序，但区域和语言选项 (Intl.cpl) 和日期和时间 (Timedate.cpl) 除外 
* 所有超文本标记语言 (HTML) 呈现引擎，其中包括 Internet Explorer 和 HTML 帮助 
* Windows Mail 
* Windows Media Player 
* 大多数附件，例如画图、 计算器和写字板

.NET Framework 不也存在在 Server Core 中，这意味着没有正在运行服务器核心安装上的托管代码的支持。 仅本机代码，使用 Windows 应用程序编程接口 (Api) 编写的代码，可以在 Server Core 上运行。 总之，任何 GUI 应用程序依赖.NET Framework 或 Explorer.exe 外壳程序不会运行在 Server Core 上。 

>[!NOTE]
>由于 Windows PowerShell 要求.NET Framework，不能安装到服务器核心上的 Windows PowerShell。 但是，可以管理使用 Windows PowerShell，远程处理程序，但前提使用仅 PowerShell WMI 命令的服务器核心安装。

##<a name="supported-server-roles"></a>支持的服务器角色 
服务器核心安装提供仅有限的数量的服务器角色与 Windows Server 2008 的完整安装。 表 1-3 比较了可用于的 Windows Server 2008 Enterprise Edition 的完全安装选项和服务器核心安装的角色。 

**表 1-3**完全安装选项和服务器核心安装的 Windows Server 2008 Enterprise Edition 的服务器角色之间的比较
| 服务器角色  | 在完整安装中可用  | Server Core 中可用  |
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

可用于服务器核心角色通常是而不考虑体系结构 （x86 或 x64） 和产品版本相同的虽然有少数例外情况：
* 仅当使用 HYPER-V 产品媒体购买 Windows Server 2008 提供 HYPER-V （虚拟化） 角色，并且 (HYPER-V 是仅适用于 x64 版本)。 如果不需要此角色，你可以改为不包含的 HYPER-V 产品媒体购买 Windows Server 2008。 
* Standard Edition 上的文件服务角色仅限于一个独立分布式文件系统 (DFS) 根和不支持 Cross-File 复制 (DFS-R)。 
* 您可以在 Server Core 上安装流媒体服务角色之前，需要下载并安装相应 Microsoft 更新独立程序包 （.msu 文件） 的服务器的体系结构 （x86 或 x64） 从 Microsoft 下载中心获得。
* Web 服务器 (IIS) 角色不支持 ASP.NET。 这是因为服务器核心，这可以限制与服务器核心 Web 服务器可以执行的操作不支持.NET Framework。 

##<a name="supported-optional-features"></a>支持的可选功能
服务器核心安装还支持仅提供的功能的有限的子集上完整安装的 Windows Server 2008。 表 1-4 进行比较可用于的 Windows Server 2008 Enterprise Edition 的完全安装选项和服务器核心安装的功能。

**表 1-4**的完全安装选项和服务器核心安装的 Windows Server 2008 Enterprise Edition 的功能比较

| 功能  | 在完整安装中可用  | Server Core 中可用  |
| ------------- | :-------------: | :------------: |
| .NET framework 3.0 功能  | X  |  |
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
| RPC Over HTTP 代理 | X  |  |
| 简单 TCP/IP 服务  | X  |  |
| SMTP 服务器  | X  |  |
| SMNP 服务  | X  | X  |
| 存储管理器 San  | X  |  |
| 基于 UNIX 的应用程序的子系统 | X | X  |
| Telnet 客户端  | X | X  |
| Telnet 服务器  | X   |  |
| TFTP 客户端  | X   |  |
| Windows 内部数据库  | X  |  |
| Windows PowerShell  | X  |  |
| Windows 产品激活服务  | X   |  |
| Windows Server Backup 功能  | X  | X  |
| Windows 系统资源管理器  | X  |  |
| WINS 服务器  | X | X |
| 无线 LAN 服务 | X  |  |

同样，有的几点需要了解有关在 Server Core 上提供的功能：
* 某些功能可能在 Server Core 上需要特殊硬件才能正确 （或根本）。 这些功能包括 BitLocker 驱动器加密、 故障转移群集、 多路径 IO、 网络负载平衡和可移动存储。 
* 故障转移群集上不可用标准版。

##<a name="server-core-architecture"></a>服务器核心体系结构
深入了解到服务器核心，让我们来简要介绍一下 Windows Server 2008 的服务器核心安装的体系结构进行比较，这与完全安装。 首先，请记住，服务器核心不是其他版本的 Windows Server 2008，但只需安装选项，可以选择到系统上安装 Windows Server 2008 时。 这意味着：
* 服务器核心安装上的内核是相同的相同的硬件体系结构 （x86 或 x64） 和版本类别的完整安装上找到。 
* 如果服务器核心安装上存在一个二进制文件，则相同的硬件体系结构 （x86 或 x64） 和版本的完整安装具有相同版本的该特定二进制文件 （稍后讨论的两种例外情况）。 
* 如果特定的设置 （如特定的防火墙例外或特定服务的启动类型） 的 Server Core 安装上的某些默认配置，配置该设置的相同的完全安装上完全相同的方式硬件体系结构 （x86 或 x64） 和版本。

图 1-3 显示了完整的安装和 Windows Server 2008 的服务器核心安装的体系结构的简化的视图。 点线表示服务器核心体系的结构，而整个关系图表示的完整安装的体系结构。 

下图说明了 Windows Server 2008 中，使用在核心操作系统功能的子集时正在构造的 Server Core 的模块化体系结构。 为相同的硬件体系结构和版本，存在的全新服务器核心安装上的每个文件还存在。 在完整安装中，两个特殊文件除外 （Scregedit.wsf 和 Oclist.exe） 是仅在服务器核心上存在 这些特殊文件包括在服务器核心，以简化的初始配置的服务器核心安装和添加或删除角色和可选组件。 

![服务器核心和完整安装的体系结构](../media/what-is-server-core-2008/Fig1-3.png)

**图 1-3**的服务器核心和完整安装的体系结构

##<a name="driver-support"></a>驱动程序支持
图 1-3 所示的服务器核心体系结构关系图很明显简化;它不会显示一件事是设备驱动程序支持在服务器核心和完整安装之间的差异。 Windows Server 2008 的完整安装包含成千上万个不同类型的设备，以便你能够在各种不同的硬件配置上安装产品的现成驱动程序。 （如 Windows Vista 客户端操作系统包括更多的驱动程序以支持设备，例如数码照相机和扫描仪通常不与服务器一起使用。） 

如果新的设备连接到 （或安装在） Windows Server 2008 的完整安装，插即用 (PnP) 子系统首先检查设备的现成驱动程序是否存在。 如果找到兼容的现成驱动程序，即插即用的子系统将自动安装该驱动程序和设备，然后进行操作。 在完整安装的 Windows Server 2008 中的气球状弹出通知可能会显示，指示安装了驱动程序和设备便可供使用。 

在服务器核心安装中，驱动程序安装过程是相同的 （即插即用的子系统是存在于服务器核心上） 有两个限制。 首先，服务器核心包括只有最少数量的现成驱动程序，并仅为设备的以下类型：
* 标准的视频图形数组 (VGA) 视频驱动程序 
* 存储设备的驱动程序 
* 网络适配器驱动程序

请注意，对于每个如下所示的三个设备类别，Server Core 包含相应的完全安装 （适用于相同的硬件体系结构） 找到的相同现成驱动程序。 

此外，当即插即用子系统会自动安装新设备的驱动程序，它会以无提示方式 — 不显示任何提示框弹出通知。 为什么不呢？ 因为在 Server Core 上没有任何 GUI 还有没有任务栏中，以便在任务栏上没有通知区域精彩内容 ！ 

因此要怎么做时打印服务角色添加到服务器核心安装，并且你想要安装打印机？ 您的打印机驱动程序手动添加到服务器-服务器核心有没有现成的打印驱动程序。

##<a name="service-footprint"></a>服务占用空间
服务器核心是最小安装，因为它具有较小的系统服务占用比相应的完整安装的相同的硬件体系结构和版本。 例如，默认情况下，Windows Server 2008 中，其中大约 50 配置为自动启动的完全安装上安装约 75 个系统服务。 与此相反，服务器核心已自动安装的默认值，且少于 40 这些开始仅约 70 服务。 

表 1-5 列出了默认情况下，在服务器核心安装中，启动模式下进行安装的服务和每个服务使用的帐户。

**表 1-5**默认情况下，在 Server Core 上安装的系统服务

| 服务名称  | 显示名称  | 启动模式  | 帐户  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | 应用程序体验  | 自动 | LocalSystem |
| AppMgmt  | 应用程序管理  | Manual | LocalSystem |
| BFE | 基本筛选引擎  | 自动 | LocalService |
| BITS | 后台智能传送服务  | 自动 | LocalSystem |
| 浏览器 | 计算机浏览器  | Manual | LocalSystem |
| CertPropSvc | 证书传播  | Manual | LocalSystem |
| COMSysApp  | COM + 系统应用程序  | Manual | LocalSystem |
| CryptSvc  | 加密服务  | 自动 | Network-Service |
| DcomLaunch  | DCOM 服务器进程启动器  | 自动 | LocalSystem |
| Dhcp  | DHCP 客户端  | 自动 | LocalService |
| 启动 Dnscache | DNS 客户端的运行情况  | 自动 | Network-Service |
| DPS  | 诊断策略服务  | 自动 | LocalService |
| 事件日志 | Windows 事件日志  | 自动 | LocalService |
| EventSystem  | COM + 事件系统  | 自动 | LocalService |
| FCRegSvc  | Microsoft 光纤通道平台注册服务  | Manual | LocalService |
| gpsvc  | 组策略客户端  | 自动 | LocalSystem |
| hidserv | 人机接口设备的访问  | Manual | LocalSystem |
| hkmsvc  | 运行状况密钥和证书管理  | Manual | LocalSystem |
| IKEEXT  | IKE 和 AuthIP IPsec 密钥模块  | 自动 | LocalSystem |
| iphlpsvc  | IP 帮助程序  | 自动 | LocalSystem |
| KeyIso | CNG 密钥隔离  | Manual | LocalSystem |
| KtmRm  | 适用于分布式事务处理协调器的 KtmRm  | 自动 | Network-Service |
| LanmanServer  | Server  | 自动 | LocalSystem |
| LanmanWorkstation  | Workstatione  | 自动 | LocalService |
| lltdsvc  | 链路层拓扑发现映射器  | Manual | LocalService |
| lmhosts  | TCP/IP NetBIOS 帮助程序  | 自动 | LocalService |
| MpsSvc  | Windows 防火墙  | 自动 | LocalService |
| MSDTC  | 分布式事务处理协调器  | 自动 | Network-Service |
| MSiSCSI  | Microsoft iSCSI 发起程序服务  | Manual | LocalSystem |
| msiserver  | Windows Installer  | Manual | LocalSystem |
| napagent  | 网络访问保护代理  | Manual | Network-Service |
| Netlogon  | Netlogon  | Manual | LocalSystem |
| netprofm  | 网络列表服务  | 自动 | LocalService |
| NlaSvc  | 网络位置感知  | 自动 | Network-Service |
| nsi  | 网络存储界面服务  | 自动 | LocalService |
| pla  | 性能日志和警报  | Manual | LocalService |
| PlugPlay  | Plug and Play  | 自动 | LocalSystem |
| PolicyAgent  | IPsec 策略代理  | 自动 | Network-Service |
| ProfSvc  | 用户配置文件服务  | 自动 | LocalSystem |
| ProtectedStorage  | 受保护的存储  | Manual | LocalSystem |
| RemoteRegistry  | 远程注册表  | 自动 | LocalService |
| RpcSs  | 远程过程调用 (RPC)  | 自动 | Network-Service |
| RSoPProv | 策略提供程序的结果集  | Manual | LocalSystem |
| sacsvr  | 特殊管理控制台帮助  | Manual | LocalSystem |
| SamSs  | 安全帐户管理器  | 自动 | LocalSystem |
| SCardSvr | 智能卡  | Manual | LocalService |
| 计划 | 任务计划程序  | 自动 | LocalSystem |
| SCPolicySvc | 智能卡移除策略  | Manual | LocalSystem |
| seclogon | 辅助登录  | 自动 | LocalSystem |
| SENS | 系统事件通知服务  | 自动 | LocalSystem |
| SessionEnv | 终端服务配置  | Manual | LocalSystem |
| slsvc  | 软件授权 | 自动 | Network-Service |
| SNMPTRAP  | SNMP 陷阱  | Manual | LocalService |
| swprv  | Microsoft 软件卷影复制提供程序 | Manual | LocalSystem |
| TBS | TPM 基本服务  | Manual | LocalService |
| TermService  | 终端服务 | 自动 | Network-Service |
| TrustedInstaller | Windows 模块安装程序  | 自动 | LocalSystem |
| UmRdpService | 终端服务 UserMode 端口重定向程序  | Manual | LocalSystem |
| vds | 虚拟磁盘  | Manual | LocalSystem |
| VSS | 卷影复制  | Manual | LocalSystem |
| W32Time | Windows 时间  | 自动 | LocalService |
| WcsPlugInService  | Windows 颜色系统  | Manual | LocalService |
| WdiServiceHost  | 诊断服务主机  | Manual | LocalService |
| WdiSystemHost  | 诊断系统主机  | Manual | LocalSystem |
| Wecsvc | Windows 事件收集器  | Manual | Network-Service |
| WinHttpAuto-ProxySvc  | WinHTTP Web 代理自动发现服务  | 自动 | LocalService |
| Winmgmt | Windows Management Instrumentation | 自动 | LocalSystem |
| WinRM  | Windows 远程管理 (Ws-management) | 自动 | Network-Service |
| wmiApSrv  | WMI 性能适配器  | Manual | LocalSystem |
| wuauserv | Windows 更新 | 自动 | LocalSystem |