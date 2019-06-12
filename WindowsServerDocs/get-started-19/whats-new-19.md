---
title: Windows Server 2019 中的新增功能
description: Windows Server 2019 中新功能的概述，包括桌面体验、存储迁移服务、系统见解、Azure 网络适配器、存储空间直通改进以及其他更改。
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: 7110fe78982fec616174a93514d86fb2e1cf9fa5
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810764"
---
# <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 中的新增功能

> 适用于：Windows Server 2019

本主题将介绍 Windows Server 2019 中的一些新增功能。 Windows Server 2019 强大的 Windows Server 2016 基础上构建，并大量创新上四个关键主题：混合云、 安全、 应用程序平台和超聚合基础结构 (HCI)。

若要了解什么是 Windows Server 半年频道发布中的新增功能，请参阅[What's New in Windows Server](../get-started/whats-new-in-windows-server.md)。

## <a name="general"></a>常规

### <a name="windows-admin-center"></a>Windows Admin Center

Windows Admin Center 是本地部署的基于浏览器的应用，用于管理服务器、群集、超级集成基础架构和 Windows 10 电脑。 它不会在 Windows 之外产生额外费用，并可以在生产中使用。

可以在 Windows Server 2019，以及 Windows 10 和早期版本的 Windows 和 Windows Server 上安装 Windows Admin Center 和使用它来管理服务器和群集运行 Windows Server 2008 R2 和更高版本。

有关详细信息，请参阅[Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

### <a name="desktop-experience"></a>桌面体验

Windows Server 2019 是长期服务频道 (LTSC) 版本，因此它包含<b>桌面体验</b>。 (半年频道\(SAC\)版本不包含桌面体验的设计; 它们是严格 Server Core 和 Nano Server 容器映像版本。)与 Windows Server 2016 中，在操作系统安装过程中您可以选择服务器核心安装或服务器带桌面体验安装。

### <a name="system-insights"></a>系统见解

系统见解是 Windows Server 2019 中提供的一项新功能，以本机方式为 Windows Server 带来了本地预测分析功能。 这些预测功能中的每种功能都受机器学习模型支持，可在本地分析 Windows Server 系统数据（例如性能计数器和事件），提供你的服务器功能的见解，并帮助减少与被动管理 Windows Server 部署中的问题相关的操作支出。

## <a name="hybrid-cloud"></a>混合云

### <a name="server-core-app-compatibility-feature-on-demand"></a>服务器核心应用兼容性按需功能

[服务器核心应用兼容性按需功能 (FOD)](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19)通过包含 Windows Server 中的二进制文件和组件子集与桌面体验而无需添加 Windows Server 桌面体验图形环境，显著提高了 Windows Server 核心安装选项的应用兼容性。  此举可增加服务器核心的功能和兼容性，同时尽可能保持精简。  

此可选按需功能适用于单独的 ISO，并且可使用 DISM 仅添加到 Windows Server Core 安装和映像中。 

## <a name="security"></a>安全性

### <a name="windows-defender-advanced-threat-protection-atp"></a>Windows Defender 高级威胁防护 (ATP)

ATP 的深度平台传感器和响应操作可暴露内存和内核级别攻击，并通过抑制恶意文件和终止恶意进程进行响应。

-   有关 Windows Defender ATP 的详细信息，请参阅 [Windows Defender ATP 功能概述](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview)。

-   有关载入服务器的详细信息，请参阅[将服务器载入 Windows Defender ATP 服务](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection)。

**Windows Defender ATP 攻击防护**是一组新的主机入侵防护功能。 Windows Defender 攻击防护的四个组件旨在锁定设备免受各种不同攻击媒介的威胁并阻止恶意软件攻击中常用的行为，同时让你能够在安全风险和工作效率要求之间实现平衡。

-   [攻击面减少 (ASR)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) 是一组控件，企业可以启用它们以通过阻止可疑恶意文件（例如，Office 文件）、脚本、横向移动、勒索软件行为和基于电子邮件的威胁防止恶意软件入侵计算机。

-   [网络保护](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/network-protection-exploit-guard?ocid=cx-blog-mmpc)可通过 Windows Defender SmartScreen 来阻止设备上的任何出站进程访问不受信任的主机 IP 地址，从而保护终结点免受基于 Web 的威胁。

-   [受控文件夹访问权限](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc)可阻止不受信任的进程访问受保护的文件夹，从而保护敏感数据免受勒索软件的威胁。

-   [Exploit Protection](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) 是针对漏洞利用的一组缓解（代替 EMET），可以轻松地进行配置以保护你的系统和应用程序。

[Windows Defender 应用程序控制](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)（也称为代码完整性 (CI) 策略）已在 Windows Server 2016 中发布。
客户反馈表明，这个概念虽好，但却难以实施。
为了解决此问题，我们构建了默认 CI 策略，允许所有 Windows 内置文件和 Microsoft 应用程序（如 SQL Server），同时阻止可以绕过 CI 的已知可执行文件。 

### <a name="security-with-software-defined-networking-sdn"></a>软件定义的网络 (SDN) 的安全性

[SDN 的安全性](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top)提供多种功能来增强客户在本地或作为云中的服务提供商运行工作负载的信心。 

这些安全增强功能集成到了 Windows Server 2016 中引入的全面 SDN 平台中。

有关 SDN 中新增功能的完整列表，请参阅 [Windows Server 2019 的 SDN 中的新增功能](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new)。

### <a name="shielded-virtual-machines-improvements"></a>受防护的虚拟机改进

- **分支机构改进**

    您现在可以通过利用新的[回退 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[脱机模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能，利用主机保护者服务的间歇性连接在计算机上运行受防护的虚拟机。 回退 HGS 允许你配置第二组用于 Hyper-V 的 URL，以尝试是否无法访问你的主 HGS 服务器。

    即使不能访问 HGS，脱机模式也允许你继续启动受防护的虚拟机，只要虚拟机成功启动一次并且主机的安全配置并未更改即可。

- **疑难解答改进功能**

    我们还通过启用对 VMConnect 增强会话模式和 PowerShell Direct 的支持简化了[受防护虚拟机的故障排除](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 这些工具尤其适用于到 VM 的网络连接已断开，需要更新其配置才能恢复访问的情况。 

    这些功能不需要进行配置，当受防护的 VM 置于运行 Windows Server 版本 1803 或更高版本的 Hyper-V 主机上时，这些功能会自动变为可用状态。

- **Linux 支持**

    如果你运行混合操作系统环境，Windows Server 2019 现在支持在受防护的虚拟机内运行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

### <a name="http2-for-a-faster-and-safer-web"></a>HTTP/2 实现更快、更安全的 Web

- 改进了连接合并，可提供不间断且正确加密的浏览体验。

- 升级的 HTTP/2 的服务器端加密套件协商可自动缓解连接失败且易于部署。

- 已将默认 TCP 拥塞提供程序更改为 Cubic，以为你提供更大的吞吐量！

## <a name="storage"></a>存储

下面是我们已对 Windows Server 2019 中的存储做出的一些更改。 有关详细信息，请参阅[存储中的新增功能](../storage/whats-new-in-storage.md)。

### <a name="storage-migration-service"></a>存储迁移服务

存储迁移服务是一种新技术，可以更轻松地将服务器迁移到更新版本的 Windows Server。 它提供一个图形工具，可清查服务器上的数据、将该数据和配置传输到更新的服务器，然后可以选择将旧服务器的标识移动到新服务器，以便应用和用户不必做出任何更改。 有关详细信息，请参阅[存储迁移服务](../storage/storage-migration-service/overview.md)。

### <a name="storage-spaces-direct"></a>存储空间直通

下面是存储空间直通中的新增功能的列表。 有关详细信息，请参阅[存储空间直通中的新增功能](../storage/whats-new-in-storage.md#storage-spaces-direct)。 另请参阅[Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview)获取信息验证这些存储空间直通系统。

- **重复数据删除和 ReFS 卷的压缩**
- **对永久性内存的本机支持**
- **两个节点在边缘超聚合基础结构的嵌套复原能力**
- **两个服务器群集使用 USB 闪存驱动器作为见证服务器**
- **Windows Admin Center 支持**
- **性能历史记录**
- **缩放最高支持每个群集 PB**
- **镜像加速奇偶校验更快地为 2 X**
- **驱动器延迟离群值检测**
- **手动分隔卷，以提高容错能力的分配**

### <a name="storage-replica"></a>存储副本

下面是存储副本中的新增功能。 有关详细信息，请参阅[存储副本中的新增功能](../storage/whats-new-in-storage.md#storage-replica)。

- 存储副本现在适用于 Windows Server 2019 标准版。
- 测试故障转移是一项新功能，允许安装目标存储以验证复制或备份数据。 有关详细信息，请参阅[关于存储副本的常见问题](../storage/storage-replica/storage-replica-frequently-asked-questions.md)。
- 存储副本记录性能改进
- Windows Admin Center 支持

## <a name="failover-clustering"></a>故障转移群集

以下是故障转移群集中的一系列新增功能。 有关详细信息，请参阅[故障转移群集中的新增功能](../failover-clustering/whats-new-in-failover-clustering.md)。

- **群集集**
- **Azure 感知群集**
- **跨域的群集迁移**
- **USB 见证服务器**
- **群集基础结构改进**
- **群集感知更新支持存储空间直通**
- **文件共享见证服务器增强功能**
- **群集强化**
- **故障转移群集不再使用 NTLM 身份验证**

## <a name="application-platform"></a>应用程序平台

### <a name="linux-containers-on-windows"></a>Windows 上的 Linux 容器

现在可以使用相同的 Docker 守护程序在同一容器主机上运行基于 Windows 和 Linux 的容器。 这使你能够具有异构容器主机环境，同时向应用程序开发人员提供灵活性。

### <a name="built-in-support-for-kubernetes"></a>适用于 Kubernetes 的内置支持

Windows Server 2019 通过推出半年频道版本不断改进计算、联网和存储功能，以支持 Kubernetes 在 Windows 上运行。 即将推出的 Kubernetes 版本中将提供更多详细信息。

- Windows Server 2019 中的[容器网络](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview)通过增强平台网络复原和容器网络插件支持极大地提高了 Windows 上 Kubernetes 的可用性。

- 在 Kubernetes 上部署的工作负载将能够利用网络安全性来保护使用嵌入式工具的 Linux 和 Windows 服务。

### <a name="container-improvements"></a>容器改进
    
- **改进了集成的身份**

    我们简化了容器中的集成 Windows 身份验证并提高了其可靠性，并解决了早期 Windows Server 版本中的几个限制。

- **更好的应用程序兼容性**

    只需将容器基于 Windows 的应用程序更加简单：对现有应用程序兼容性*windowsservercore*映像已增加。 对于具有其他 API 依赖项的应用程序，现在还增加了第三个基本映像：*windows*。

- **减少的大小和更高的性能**

    基本容器映像的下载大小、磁盘大小和启动时间都得到了改善。 这可以加快容器工作流

- **使用 Windows Admin Center 的管理体验\(预览\)**

    现在，使用 Windows Admin Center 的新扩展，用户可以比以往更轻松地查看计算机上正在运行哪些容器并管理各个容器。 查找 [Windows Admin Center 公共源](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions)中的“容器”扩展。

### <a name="encrypted-networks"></a>加密网络

[加密网络](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new) - 虚拟网络加密允许对在标记为**启用加密**的子网内相互通信的虚拟机之间的虚拟网络流量进行加密。 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。

### <a name="network-performance-improvements-for-virtual-workloads"></a>虚拟工作负载的网络性能提升

[虚拟工作负载的网络性能提升](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview)可最大程度地提升到虚拟机的网络吞吐量，且无需不断地调整或过度预配你的主机。 这降低了操作和维护成本，同时提高了主机的可用密度。 这些新功能包括：

* 在 vSwitch 中接收段合并

* 动态虚拟机多队列 (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>低额外延迟后台传输

低额外延迟后台传输 (LEDBAT) 是一款针对延迟进行优化的网络拥塞控制提供程序，旨在自动产生为用户和应用程序分配的带宽，同时在网络未使用时使用整个可用带宽。   
此技术用于在整个 IT 环境中部署较大的关键更新，而不会影响面向客户的服务和关联带宽。

### <a name="windows-time-service"></a>Windows 时间服务

[Windows 时间服务](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview)包括符合 UTC 标准的闰秒级支持、称为精确时间协议的新时间协议和端到端可追溯性。


### <a name="high-performance-sdn-gateways"></a>高性能 SDN 网关

Windows Server 2019 中的[高性能 SDN 网关](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance)极大地提高了 IPsec 和 GRE 连接的性能，在显著降低 CPU 使用率的同时可提供超高性能吞吐量。
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>用于 SDN 的新部署 UI 和 Windows Admin Center 扩展

现在，借助 Windows Server 2019，人们可以通过有助于充分利用 SDN 的新部署 UI 和 Windows Admin Center 扩展轻松实现部署和管理。 

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Hyper-V VM 的持久性内存支持

为了在虚拟机中充分利用持久性内存（也称为 存储类内存）的高吞吐量和低延迟，它现在可以直接投影到虚拟机。 这有助于大大减少数据库事务延迟或减少出现故障时低延迟内存中数据库的恢复时间。

