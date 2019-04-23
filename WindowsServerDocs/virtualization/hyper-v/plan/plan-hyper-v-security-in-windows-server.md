---
title: Windows Server 中的 HYPER-V 安全性规划
description: 提供安全注意事项的 hyper-v 主机和虚拟机的列表
ms.prod: windows-server-threshold
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877408"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Windows Server 中的 HYPER-V 安全性规划

>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

保护的 HYPER-V 主机操作系统、 虚拟机、 配置文件和虚拟机数据。 使用下面列出的建议做法清单来帮助你保护的 HYPER-V 环境。

## <a name="secure-the-hyper-v-host"></a>保护的 HYPER-V 主机
- **使的主机操作系统保护。**
    - 使用最小所需的管理操作系统的 Windows Server 安装选项，最小化攻击面。 有关详细信息，请参阅[安装选项部分](/windows-server/windows-server#installation-options)的 Windows Server 技术内容库。 我们不建议在 Windows 10 上的 HYPER-V 上运行生产工作负荷。
    - 保留的 HYPER-V 主机操作系统、 固件和设备驱动程序最新的最新安全更新。 检查更新固件和驱动程序的供应商的建议。
    - 不要作为工作站中使用的 HYPER-V 主机或安装任何不必要的软件。
    - 远程管理的 HYPER-V 主机。 你必须管理本地 HYPER-V 主机，如果使用 Credential Guard。 有关详细信息，请参阅[使用 Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。
    - 启用代码完整性策略。 使用基于虚拟化的安全性受保护的代码完整性服务。 有关详细信息，请参阅[设备保护部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
- **使用安全网络。**
    - 使用具有专用的网络适配器为物理 HYPER-V 计算机的单独网络。
    - 使用专用网络或安全网络访问 VM 配置和虚拟硬盘文件。
    - 使用实时迁移流量的专用/专用网络。 请考虑使用加密和保护你的 VM 的数据是通过网络在迁移期间此网络上启用 IPSec。 有关详细信息，请参阅[设置为没有故障转移群集的实时迁移的主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。
- **安全存储迁移通信。** 

    有关端到端加密的 SMB 数据和数据保护篡改和窃听不受信任网络上使用 SMB 3.0。 使用专用网络访问 SMB 共享内容，以防止拦截的攻击。 有关详细信息，请参阅[SMB 的安全性增强功能](https://technet.microsoft.com/library/dn551363.aspx)。 
- **配置主机在受保护的构造的一部分。** 

    有关详细信息，请参阅[受保护的构造](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
- **确保设备的安全。** 

    保护存储设备，你在其中保存虚拟机资源文件。
    
- **保护的硬盘驱动器。** 

    使用 BitLocker 驱动器加密来保护资源。
    
- **强化的 HYPER-V 主机操作系统。** 

    使用中所述的基准安全设置建议[Windows Server 安全基线](https://docs.microsoft.com/windows/device-security/windows-security-baselines)。
    
- **授予适当的权限。**
    - 添加需要管理的 HYPER-V 主机到 HYPER-V 管理员组的用户。
    - 不授予虚拟机管理员权限的 HYPER-V 主机操作系统。

- **配置防病毒排除项和选项的 HYPER-V。**  

    Windows Defender 已[自动排除项](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)配置。 有关排除的详细信息，请参阅[建议的 HYPER-V 主机的防病毒排除项](https://support.microsoft.com/kb/3105657)。 

- **不要将未知的 Vhd 装载。** 这可以公开到文件系统级别攻击的主机。

- **不要启用嵌套在生产环境中，除非这是必需。**

    如果启用嵌套时，不会在虚拟机上运行不受支持的虚拟机监控程序。  

有关更安全的环境：

- **使用带受信任的平台模块 (TPM) 2.0 芯片硬件设置受保护的构造。** 

    有关详细信息，请参阅[System requirements for Windows Server 2016 上的 HYPER-V 要求](../system-requirements-for-hyper-v-on-windows.md)。

## <a name="secure-virtual-machines"></a>保护虚拟机
- **创建生成 2 个虚拟机的受支持的来宾操作系统。** 

    有关详细信息，请参阅[第 2 代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **启用安全启动。** 

    有关详细信息，请参阅[第 2 代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **请在来宾操作系统保护。**

    - 在生产环境中的虚拟机上启用之前，请安装最新的安全更新。
    - 安装集成服务的需要并使其保持最新的受支持的来宾操作系统。 提供通过 Windows 更新的运行受支持的 Windows 版本的来宾的集成服务更新。
    - 强制执行基于的角色执行每个虚拟机中运行的操作系统。 使用中所述的基准安全设置建议[Windows 安全基线](https://docs.microsoft.com/windows/device-security/windows-security-baselines)。
    
- **使用安全网络。** 

    请确保虚拟网络适配器连接到正确的虚拟交换机，并应用适当的安全设置和限制。
    
- **将虚拟硬盘和快照文件存储在安全位置。**

- **确保设备的安全。** 

    配置仅用于虚拟机所需的设备。 除非特定方案的需要否则不要启用在生产环境中的离散设备分配。 如果您启用它，请确保仅公开来自受信任的供应商的设备。 
    
- **配置防病毒、 防火墙和入侵检测软件**作为相应的虚拟机中基于虚拟机角色。

- **为运行 Windows 10 或 Windows Server 2016 或更高版本的来宾启用基于虚拟化安全功能。** 

    有关详细信息，请参阅[设备保护部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
    
- **如果所需的特定工作负荷仅启用离散设备分配**。 

    由于传递通过物理设备的特性，请设备制造商，若要了解是否它应使用在安全的环境。

有关更安全的环境：

- **部署与防护启用虚拟机并将其部署到受保护的构造。** 

    有关详细信息，请参阅[第 2 代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)并[受保护的构造](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
