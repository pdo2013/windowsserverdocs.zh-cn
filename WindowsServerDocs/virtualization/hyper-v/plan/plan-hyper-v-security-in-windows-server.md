---
title: 规划 Windows Server 中的 Hyper-v 安全
description: 提供 Hyper-v 主机和虚拟机的安全注意事项列表
ms.prod: windows-server
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
ms.openlocfilehash: 8fd86ae500fff1e6b8c27b0d34d1dcbeeade9f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392958"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>规划 Windows Server 中的 Hyper-v 安全

>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

保护 Hyper-v 主机操作系统、虚拟机、配置文件和虚拟机数据。 使用以下建议做法列表作为清单来帮助你保护 Hyper-v 环境。

## <a name="secure-the-hyper-v-host"></a>保护 Hyper-v 主机
- **确保主机操作系统的安全。**
    - 使用管理操作系统所需的最小 Windows Server 安装选项，将攻击面降到最低。 有关详细信息，请参阅 Windows Server 技术内容库中的 "[安装选项" 部分](/windows-server/windows-server#installation-options)。 建议你不要在 Windows 10 上的 Hyper-v 上运行生产工作负荷。
    - 通过最新的安全更新使 Hyper-v 主机操作系统、固件和设备驱动程序保持最新。 请查看供应商的建议，以更新固件和驱动程序。
    - 不要将 Hyper-v 主机用作工作站或安装任何不必要的软件。
    - 远程管理 Hyper-v 主机。 如果必须本地管理 Hyper-v 主机，请使用 Credential Guard。 有关详细信息，请参阅[使用 Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。
    - 启用代码完整性策略。 使用基于虚拟化的安全保护代码完整性服务。 有关详细信息，请参阅[Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
- **使用安全的网络。**
    - 将单独的网络与用于物理 Hyper-v 计算机的专用网络适配器配合使用。
    - 使用私有或安全网络访问 VM 配置和虚拟硬盘文件。
    - 使用专用/专用网络进行实时迁移流量。 请考虑在此网络上启用 IPSec 来使用加密，并确保在迁移过程中通过网络传输 VM 的数据。 有关详细信息，请参阅在[没有故障转移群集的情况下，设置用于实时迁移的主机](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。
- **保护存储迁移流量。** 

    使用 SMB 3.0 对不受信任的网络进行端到端的 SMB 数据加密和数据保护篡改或窃听。 使用专用网络访问 SMB 共享内容，以防止中间人攻击。 有关详细信息，请参阅[SMB 安全增强功能](https://technet.microsoft.com/library/dn551363.aspx)。 
- **将主机配置为受保护的构造的一部分。** 

    有关详细信息，请参阅[受保护的构造](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
- **保护设备。** 

    保护存放虚拟机资源文件的存储设备。
    
- **保护硬盘驱动器。** 

    使用 BitLocker 驱动器加密来保护资源。
    
- **强化 Hyper-v 主机操作系统。** 

    使用[Windows Server Security 基线](https://docs.microsoft.com/windows/device-security/windows-security-baselines)中所述的基准安全设置建议。
    
- **授予适当的权限。**
    - 将需要管理 Hyper-v 主机的用户添加到 Hyper-v 管理员组。
    - 不要向虚拟机管理员授予 Hyper-v 主机操作系统的权限。

- **为 Hyper-v 配置防病毒排除和选项。**  

    Windows Defender 已配置[自动排除](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)项。 有关排除项的详细信息，请参阅[hyper-v 主机的建议防病毒排除项](https://support.microsoft.com/kb/3105657)。 

- **请勿装入未知 Vhd。** 这可能会使主机面临文件系统级攻击。

- **除非需要，否则不要在生产环境中启用嵌套。**

    如果启用嵌套，请不要在虚拟机上运行不受支持的虚拟机监控程序。  

对于更安全的环境：

- **将硬件与受信任的平台模块（TPM）2.0 芯片配合使用来设置受保护的构造。** 

    有关详细信息，请参阅[Windows Server 2016 上的 Hyper-v 系统要求](../system-requirements-for-hyper-v-on-windows.md)。

## <a name="secure-virtual-machines"></a>保护虚拟机
- **为支持的来宾操作系统创建第2代虚拟机。** 

    有关详细信息，请参阅[第2代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **启用安全启动。** 

    有关详细信息，请参阅[第2代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **确保来宾操作系统的安全。**

    - 在生产环境中打开虚拟机之前，请安装最新的安全更新。
    - 为需要的支持来宾操作系统安装 integration services，并使其保持最新状态。 运行受支持的 Windows 版本的来宾的集成服务更新通过 Windows 更新提供。
    - 根据其执行的角色强化在每个虚拟机中运行的操作系统。 使用[Windows 安全基线](https://docs.microsoft.com/windows/device-security/windows-security-baselines)中描述的基准安全设置建议。
    
- **使用安全的网络。** 

    请确保虚拟网络适配器连接到正确的虚拟交换机，并应用相应的安全设置和限制。
    
- **将虚拟硬盘和快照文件存储在安全的位置。**

- **保护设备。** 

    仅配置虚拟机所需的设备。 不要在生产环境中启用离散设备分配，除非你需要用于特定方案。 如果启用此设置，请确保仅从受信任的供应商处公开设备。 
    
- 根据虚拟机角色，在虚拟机中**配置防病毒、防火墙和入侵检测软件**。

- **为运行 Windows 10 或 Windows Server 2016 或更高版本的来宾启用基于虚拟化的安全性。** 

    有关详细信息，请参阅[Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
    
- **仅在特定工作负荷需要时才启用离散设备分配**。 

    由于通过物理设备进行传递，因此请与设备制造商合作，以了解是否应该在安全的环境中使用它。

对于更安全的环境：

- **部署启用了防护的虚拟机，并将其部署到受保护的构造。** 

    有关详细信息，请参阅[第2代安全设置](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)和[受保护的构造](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
