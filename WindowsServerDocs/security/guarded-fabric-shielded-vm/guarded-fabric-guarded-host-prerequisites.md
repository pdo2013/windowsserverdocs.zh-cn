---
title: 受保护的主机先决条件
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a9273eef906130b11b98148cf1e84f7e18812b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402373"
---
# <a name="prerequisites-for-guarded-hosts"></a>受保护主机的先决条件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

查看所选证明模式的主机必备项，然后单击 "下一步" 以添加受保护的主机。

## <a name="tpm-trusted-attestation"></a>受 TPM 信任的证明

使用 TPM 模式的受保护主机必须满足以下先决条件：

-   **硬件**：初始部署需要一个主机。 若要测试受防护的 Vm 的 Hyper-v 实时迁移，必须至少有两个主机。

    主机必须具有：
    
    - IOMMU 和二级地址转换（SLAT）
    - TPM 2.0
    - UEFI 2.3.1 或更高版本
    - 配置为使用 UEFI （而非 BIOS 或 "旧" 模式）启动
    - 已启用安全启动
        
-   **操作系统**:Windows Server 2016 Datacenter edition 或更高版本

    > [!IMPORTANT]
    > 请确保安装[最新的累积更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。  

-   **角色和功能**：Hyper-v 角色和主机保护者 Hyper-v 支持功能。 主机保护者 Hyper-v 支持功能仅在 Windows Server Datacenter edition 上可用。 

> [!WARNING]
> 主机保护者 Hyper-v 支持功能可为可能与某些设备不兼容的代码完整性启用基于虚拟化的保护。 在启用此功能之前，强烈建议在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。 有关详细信息，请参阅[兼容硬件和基于 Windows Server 虚拟化的代码完整性保护](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：** 
> [!div class="nextstepaction"]
> [捕获 TPM 信息](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>主机密钥证明

使用主机密钥证明的受保护主机必须满足以下先决条件：

- **硬件**：所有能够运行 Hyper-v 的服务器，从 Windows Server 2019 开始
- **操作系统**:Windows Server 2019 Datacenter Edition
- **角色和功能**：Hyper-v 角色和主机保护者 Hyper-v 支持功能 

主机可以加入域或工作组。 

对于主机密钥证明，HGS 必须运行 Windows Server 2019 并与 v2 证明一起运行。 有关详细信息，请参阅[HGS 必备组件](guarded-fabric-prepare-for-hgs.md#prerequisites)。 

**下一步：** 
> [!div class="nextstepaction"]
> [创建密钥对](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>管理员信任的证明

>[!IMPORTANT]
>从 Windows Server 2019 开始，已弃用管理受信任的证明（AD 模式）。 对于不可能进行 TPM 证明的环境，请配置[主机密钥证明](#host-key-attestation)。 主机密钥证明向 AD 模式提供类似的保障，并更易于设置。 

Hyper-v 主机必须满足 AD 模式的下列先决条件：

-   **硬件**：从 Windows Server 2016 开始，任何能够运行 Hyper-v 的服务器。 初始部署需要一个主机。 若要测试受防护 Vm 的 Hyper-v 实时迁移，需要至少两个主机。

-   **操作系统**:Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > 安装[最新的累积更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色和功能**：Hyper-v 角色和主机保护者 Hyper-v 支持功能，它仅在 Windows Server 2016 Datacenter edition 中可用。 

> [!WARNING]
> 主机保护者 Hyper-v 支持功能可为可能与某些设备不兼容的代码完整性启用基于虚拟化的保护。 在启用此功能之前，强烈建议在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。 有关详细信息，请参阅[兼容硬件和基于 Windows Server 2016 的代码完整性保护](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：** 
> [!div class="nextstepaction"]
> [将受保护的主机置于安全组中](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)