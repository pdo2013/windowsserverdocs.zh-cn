---
title: Windows Server 2016 中的新增功能
description: 计算、标识、管理、自动化、网络、安全性、存储方面的新增功能。
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 76cfd0f0cca18fb072883a9e14fae420516bd329
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391362"
---
# <a name="whats-new-in-windows-server-2016"></a>Windows Server 2016 中的新增功能

>适用于：Windows Server 2016

![显示报纸的图标](media/whats-new.png)若要了解 Windows 中的最新功能，请参阅 [Windows Server 中的新增功能](whats-new-in-windows-server.md)。 本部分的内容将介绍 Windows Server&reg; 2016 中的新增功能和更改的功能。 此处列出的新功能和更改在你使用此版本时最可能具有最大影响力。

## <a name="computevirtualizationvirtualizationmd"></a>[Compute](../virtualization/virtualization.md)

虚拟化区域包括适用于 IT 专业人员的虚拟化产品和功能，以设计、部署和维护 Windows Server。  

### <a name="general"></a>常规  
由于 Win32 Time 和 Hyper-V 时间同步服务的改进，物理和虚拟计算机从更高的时间准确性中受益。 现在，Windows Server 可以托管与即将推出的要求 UTC 准确性为 1 ms 的规则相容的服务。  

### <a name="hyper-v"></a>Hyper-V  
-   [Windows Server 2016 上的 Hyper-V 中的新增功能](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md)。 本主题介绍了 Windows Server 2016 中的 Hyper-V 角色、运行在 Windows 10 上的客户端 Hyper-V 和 Microsoft Hyper-V Server 2016 中的新增和更改的功能。  

-   [Windows 容器](https://msdn.microsoft.com/virtualization/windowscontainers/containers_welcome)：Windows Server 2016 容器支持增加了性能改进，简化了网络管理，并在 Windows 10 上支持 Windows 容器。 有关容器的某些其他信息，请参阅[容器：Docker、Windows 和趋势](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/)。  

### <a name="nano-server"></a>Nano Server  
[Nano Server](getting-started-with-nano-server.md) 的新增功能。 Nano Server 具有一个已更新的模块，用于构建 Nano Server 映像，包括物理主机和来宾虚拟机功能的更大分离度，以及对不同 Windows Server 版本的支持。   

恢复控制台也有改进，其中包括入站和出站防火墙规则分离及 WinRM 配置修复功能。  

### <a name="shielded-virtual-machines"></a>受防护的虚拟机  
Windows Server 2016 提供新的基于 Hyper-V 的受防护的虚拟机，以保护任何第 2 代虚拟机免受已损坏的构造影响。 Windows Server 2016 中引入的功能如下所示：  

- 新的“支持加密”模式提供比为普通虚拟机提供的更多、但比防护模式少的保护功能，同时仍支持 vTPM、磁盘加密、实时迁移通信加密和其他功能，包括直接构造管理便利（例如虚拟机控制台连接和 Powershell Direct）。  

- 完全支持将现有非受防护的第 2 代虚拟机转换为受防护的虚拟机，包括自动磁盘加密。

- Hyper-V 虚拟机管理器现在可以查看授权运行的受防护的虚拟机上的构造，为构造管理员提供了一种打开受防护的虚拟机的密钥保护程序 (KP) 并查看构造是否有权在其上运行的方式。  

- 你可以转换运行的主机保护者服务上的证明模式。 现在你可以即时在不太安全但更简单、基于 Active Directory 的证明和基于 TPM 的证明之间进行切换。  

- 基于 Windows PowerShell 的端到端诊断工具能够检测到受保护的 Hyper-V 主机和主机保护者服务中的错误配置或错误。  

- 恢复环境不仅提供在虚拟机可正常运行的构造中安全地排查故障并修复受防护的虚拟机的方法，还提供与受防护的虚拟机本身相同的保护级别。

- 主机保护者服务支持现有的安全 Active Directory – 可以指示主机保护者服务使用现有的 Active Directory 林作为其 Active Directory，而不是创建自己的 Active Directory 实例

有关使用受防护的虚拟机的详细信息和说明，请参阅 [Shielded VMs and Guarded Fabric Validation Guide for Windows Server 2016 (TPM)](https://aka.ms/shieldedvms)（Windows Server 2016 (TPM) 受防护的 VM 和受保护的构造验证指南）。  

## <a name="identity-and-accessidentityidentity-and-accessmd"></a>[身份标识和访问权限](../identity/Identity-and-Access.md)  
身份标识中的新功能提高了组织保护 Active Directory 环境的能力，并帮助他们迁移到仅限云的部署和混合部署，其中某些应用程序和服务托管在云中，其他的则托管在本地。  

### <a name="active-directory-certificate-services"></a>Active Directory 证书服务  
Windows Server 2016 中的 Active Directory 证书服务 (AD CS) 增加了对 TPM 密钥证明的支持：现可使用智能卡 KSP 进行密钥证明，而未加入域的设备现在可以使用 NDES 注册，以获得可证明 TPM 中密钥的证书。  

### <a name="active-directory-domain-services"></a>Active Directory 域服务  
Active Directory 域服务包括可帮助组织保护 Active Directory 环境并为公司和个人设备提供更好的标识管理体验的改进。 有关详细信息，请参阅 [What's new in Active Directory Domain Services (AD DS) in Windows Server 2016](../identity/whats-new-active-directory-domain-services.md)（Windows Server 2016 中 Active Directory 域服务 (AD DS) 中的新增功能）。   

### <a name="active-directory-federation-services"></a>Active Directory 联合身份验证服务  
Active Directory 联合身份验证服务中的新增功能。 Windows Server 2016 中的 Active Directory 联合身份验证服务 (AD FS) 包括使你可以配置 AD FS 以对轻型目录访问协议 (LDAP) 目录中存储的用户进行身份验证的新功能。 有关详细信息，请参阅 [Windows Server 2016 中 AD FS 的新增功能](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md)。  

### <a name="web-application-proxy"></a>Web 应用程序代理  
Web 应用程序代理的最新版本专注于为更多应用程序实现发布和预身份验证的新功能以及改进的用户体验。 查看新功能的完整列表，其中包括针对丰富的客户端应用（如 Exchange ActiveSync）的预身份验证以及用于更轻松地发布 SharePoint 应用的通配符域。 有关详细信息，请参阅 [Windows Server 2016 中的 Web 应用程序代理](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md)。  

##  <a name="administrationadministrationmanage-windows-servermd"></a>[管理](../administration/manage-windows-server.md)  
管理和自动化部分重点介绍适用于想要运行和管理 Windows Server 2016（包括 Windows PowerShell）的 IT 专业人员的工具和参考信息。

Windows PowerShell 5.1 包含重要的新功能（包括支持使用类进行开发、可扩展其用途的新安全功能），提高其可用性，并允许你更轻松、全面地控制和管理基于 Windows 的环境。 有关详细信息，请参阅 [WMF 5.1 中的新方案和功能](https://docs.microsoft.com/powershell/wmf/5.1/scenarios-features)。

Windows Server 2016 的新增功能包括：在 Nano Server 上本地运行 PowerShell.exe（不再仅限于远程），新增“本地用户和组”cmdlet 来替换 GUI，添加了 PowerShell 调试支持，并添加了对 Nano Server 中安全日志记录和脚本以及 JEA 的支持。

下面是一些其他新管理功能：

### <a name="powershell-desired-state-configuration-dsc-in-windows-management-framework-wmf-5"></a>Windows Management Framework (WMF) 5 中的 PowerShell 期望状态配置 (DSC)
Windows Management Framework 5 包括对 Windows PowerShell 期望状态配置 (DSC)、Windows 远程管理 (WinRM) 和 Windows 管理规范 (WMI) 的更新。

有关测试 Windows Management Framework 5 的 DSC 功能的详细信息，请参阅[验证 PowerShell DSC 的功能](https://blogs.msdn.microsoft.com/powershell/2015/07/06/validate-features-of-powershell-dsc/)中所论述的一系列博客文章。 若要下载，请参阅 [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure)。

### <a name="packagemanagement-unified-package-management-for-software-discovery-installation-and-inventory"></a>用于软件发现、安装和清单的 PackageManagement 统一包管理
Windows Server 2016 和 Windows 10 引入了一种新的 PackageManagement 功能（以前称为 OneGet），该功能可以允许 IT 专业人员或开发人员使软件发现、安装、清单 (SDII) 在本地或远程自动进行，无论安装程序技术为何，也不管软件位于何处。 

有关详细信息，请参阅 [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki)。

### <a name="powershell-enhancements-to-assist-digital-forensics-and-help-reduce-security-breaches"></a>有助于数字取证和减少安全漏洞的 PowerShell 增强功能
为了帮助负责调查受损系统的团队（有时称为“蓝队”），我们已添加其他 PowerShell 日志记录和其他数字取证功能，并且已添加有助于在脚本中减少漏洞的功能，例如受限的 PowerShell 和安全 CodeGeneration API。

有关详细信息，请参阅 [PowerShell ♥ 蓝队](https://blogs.msdn.microsoft.com/powershell/2015/06/09/powershell-the-blue-team/)。

## <a name="networkingnetworkingnetworkingmd"></a>[网络](../networking/Networking.md)  
本部分论述了适用于 IT 专业人员的网络产品和功能，可用于设计、部署和维护 Windows Server 2016。  

### <a name="software-defined-networking"></a>软件定义的网络
你现在可以将流量映射并传送到新的或现有虚拟设备。 与分布式防火墙和网络安全组联合使用，使你能够以类似于 Azure 的方式动态分段和保护工作负荷。 其次，你可以使用 System Center Virtual Machine Manager 部署并管理整个软件定义的网络 (SDN) 堆栈。 最后，可以使用 Docker 来管理 Windows Server 容器网络，并将 SDN 策略与虚拟机和容器关联。 有关详细信息，请参阅[计划软件定义的网络基础结构](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md)。

### <a name="tcp-performance-improvements"></a>TCP 性能改进
默认初始拥塞窗口 (ICW) 已从 4 增加到 10 并已实现 TCP 快速打开 (TFO)。 TFO 减少了建立 TCP 连接所需的时间，并且增加的 ICW 允许在初始突发中传输较大的对象。 此组合可以显著减少在客户端和云之间传输 Internet 对象所需的时间。

当从数据包丢失恢复时，为了改善 TCP 行为，我们实施了 TCP 尾部丢失探测 (TLP) 和最新确认 (RACK)。 TLP 可帮助将转发超时 (RTO) 转换为快速恢复，而 RACK 可减少快速恢复所需的时间，以重新传输丢失的数据包。 

## <a name="security-and-assurancesecuritysecurity-and-assurancemd"></a>[安全和保障](../security/Security-and-Assurance.md)  
此部分包含适用于 IT 专业人员的安全解决方案和功能，可在数据中心和云环境中进行部署。 有关 Windows Server 2016 中常规安全性的信息，请参阅[安全和保障](../security/Security-and-Assurance.md)。  

### <a name="just-enough-administration"></a>Just Enough Administration  
Windows Server 2016 中的 Just Enough Administration 是一种安全技术，可使能由 Windows PowerShell 管理的任何内容均可进行委派管理。 功能包括对在网络标识下运行、通过 PowerShell Direct 连接、安全地复制文件到 JEA 终结点或从 JEA 终结点安全地复制文件及配置 PowerShell 控制台来在 JEA 上下文中默认启动的支持。 有关详细信息，请参阅 [GitHub 上的 JEA](https://aka.ms/JEA)。

### <a name="credential-guard"></a>Credential Guard
凭据保护使用基于虚拟化的安全性来隔离密钥，以便只有特权系统软件可以访问它们。 请查看[使用 Credential Guard 保护派生的域凭据](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)。

###  <a name="remote-credential-guard"></a>远程 Credential Guard
Credential Guard 包括对 RDP 会话的支持，以便用户凭据能够保留在客户端上，且不会在服务器端暴露。 它还提供远程桌面的单一登录体验。 请参阅[使用 Windows Defender Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。   

### <a name="device-guard-code-integrity"></a>Device Guard（代码完整性）
Device Guard 通过创建指定哪些代码可以在服务器上运行的策略提供内核模式代码完整性 (KMCI) 和用户模式代码完整性 (UMCI)。 请参阅 [Windows Defender Device Guard 简介：基于虚拟化的安全性和代码完整性策略](https://docs.microsoft.com/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies)。


### <a name="windows-defender"></a>Windows Defender  
[Windows Server 2016 的 Windows Defender 概述](../security/windows-defender/windows-defender-overview-windows-server.md)。 默认情况下，Windows Server Antimalware 已在 Windows Server 2016 中安装并处于启用状态，但是 Windows Server Antimalware 的用户界面尚未安装。 但是，Windows Server Antimalware 会在没有用户界面的情况下更新反恶意软件定义并保护计算机。 如果需要 Windows Server Antimalware 的用户界面，则可以使用“添加角色和功能向导”在操作系统安装之后安装它。

### <a name="control-flow-guard"></a>控制流防护
控制流防护 (CFG) 是一种平台安全功能，旨在防止内存损坏漏洞。 有关详细信息，请参阅 [Control Flow Guard](https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx)（控制流防护）。


## <a name="storagestoragestoragemd"></a>[存储](../storage/storage.md)

Windows Server 2016 中的存储包括软件定义存储以及传统文件服务器的新功能和增强功能。 下面是几个新功能，有关更多增强功能和详细信息，请参阅 [Windows Server 2016 中的存储的新增功能](../storage/whats-new-in-storage.md)。

### <a name="storage-spaces-direct"></a>存储空间直通

存储空间直通允许通过使用具有本地存储的服务器构建高可用性和可缩放存储。 该功能简化了软件定义的存储系统的部署和管理并且允许使用 SATA SSD 和 NVMe 磁盘设备等新型磁盘设备，而之前群集存储空间无法使用共享磁盘。

有关详细信息，请参阅[存储空间直通](../storage/storage-spaces/storage-spaces-direct-overview.md)。

### <a name="storage-replica"></a>存储副本

存储副本可在各个服务器或群集之间实现存储不可知的块级同步复制，以便在站点间进行灾难恢复及故障转移群集扩展。 同步复制支持物理站点中的镜像数据和在崩溃时保持一致的卷，以确保文件系统级别的数据损失为零。 异步复制允许超出都市范围、可能存在数据损失的站点扩展。

有关详细信息，请参阅[存储副本](../storage/storage-replica/storage-replica-overview.md)。

### <a name="storage-quality-of-service-qos"></a>服务存储质量 (QoS)

现在可以使用存储服务质量 (QoS) 来集中监控端到端存储性能，并使用 Windows Server 2016 中的 Hyper-V 和 CSV 群集创建策略。

有关详参细信息，请阅[服务存储质量](../storage/storage-qos/storage-qos-overview.md)。

## <a name="failover-clusteringfailover-clusteringwhats-new-in-failover-clusteringmd"></a>[故障转移群集](../failover-clustering/whats-new-in-failover-clustering.md)

Windows Server 2016 中包括多个服务器的新功能和增强功能，它们使用故障转移群集功能组合到单个容错群集中。 下面列出了某些新增功能，有关完整的列表，请参阅 [Windows Server 2016 中故障转移群集中的新功能](../failover-clustering/whats-new-in-failover-clustering.md)。

### <a name="cluster-operating-system-rolling-upgrade"></a>群集操作系统滚动升级

群集操作系统滚动升级允许管理员将群集节点的操作系统从 Windows Server 2012 R2 升级至 Windows Server 2016，且无需中断 Hyper-V 或横向扩展文件服务器工作负荷。 使用此功能可以避免服务级别协议 (SLA) 的停机时间损失。

有关详细信息，请参阅[群集操作系统滚动升级](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。

### <a name="cloud-witness"></a>云见证

云见证是 Windows Server 2016 中一种新型的故障转移群集仲裁见证，它将 Microsoft Azure 作为仲裁点。 与其他仲裁见证一样，云见证获取投票，并可以参与仲裁计算。 可以使用“配置群集仲裁向导”将云见证配置为仲裁见证。

有关详细信息，请参阅[部署云见证](../failover-clustering/deploy-cloud-witness.md)。

### <a name="health-service"></a>运行状况服务

运行状况服务将改进存储空间直通群集上的日常监控、操作和群集资源维护体验。

有关详细信息，请参阅[运行状况服务](../failover-clustering/health-service-overview.md)。

## <a name="application-development"></a>应用程序开发

### <a name="internet-information-services-iis-100"></a>Internet Information Services (IIS) 10.0
Windows Server 2016 中的 IIS 10.0 Web 服务器提供的新增功能包括：

- 在网络堆栈中支持 HTTP/2 协议，并与 IIS 10.0 集成，允许 IIS 10.0 网站针对支持的配置为 HTTP/2 请求自动提供服务。 与 HTTP/1.1 相比，这会有大量的增强功能，例如，更有效地重用连接和减少延迟、提高网页的加载速度。 
- 在 Nano Server 中运行和管理 IIS 10.0 的功能。 请参阅 [Nano Server 上的 IIS](iis-on-nano-server.md)。
- 支持通配符主机头，使管理员能够为域设置 Web 服务器，然后让 Web 服务器为任何子域的请求提供服务。
- 一个用于管理 IIS 的新 PowerShell 模块 (IISAdministration)。 

有关更多详细信息，请参阅 [IIS](https://iis.net/learn)。

### <a name="distributed-transaction-coordinator-msdtc"></a>分布式事务处理协调器 (MSDTC)
Microsoft Windows 10 和 Windows Server 2016 中添加了三个新功能：

- 资源管理器可以使用资源管理器重新加入的新界面，以在数据库由于错误重启后确定未决事务的结果。 有关详细信息，请参阅 [IResourceManagerRejoinable::Rejoin](https://msdn.microsoft.com/library/mt203799(v=vs.85).aspx)。

- DSN 名称限制从 256 字节扩大到 3072 字节。 有关详细信息，请参阅 [IDtcToXaHelperFactory::Create](https://msdn.microsoft.com/library/ms686861(v=vs.85).aspx)、[IDtcToXaHelperSinglePipe::XARMCreate](https://msdn.microsoft.com/library/ms679248(v=vs.85).aspx) 或 [IDtcToXaMapper::RequestNewResourceManager](https://msdn.microsoft.com/library/ms680310(v=vs.85).aspx)。

- 利用改进的跟踪功能，可以设置注册表项以在跟踪日志文件名中包括映像文件路径，以便能够告知要检查的跟踪日志文件。 有关为 MSDTC 配置跟踪的详细信息，请参阅[如何在基于 Windows 的计算机上为 MS DTC 启用诊断跟踪](https://support.microsoft.com/en-us/kb/926099)。



## <a name="see-also"></a>另请参阅  
-   [发行说明：Windows Server 2016 中的重要问题](Windows-Server-2016-GA-Release-Notes.md)  

