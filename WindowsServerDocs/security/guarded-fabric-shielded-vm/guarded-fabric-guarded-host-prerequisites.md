---
title: 受保护的主机系统必备组件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 40c0f6df31061268b1e1ef8c15b0a02b0f50b0de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447472"
---
# <a name="prerequisites-for-guarded-hosts"></a>受保护的主机的先决条件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

查看主机的先决条件的证明模式已选择，然后单击下一步，添加受保护的主机。

## <a name="tpm-trusted-attestation"></a>受信任的 TPM 证明

使用 TPM 模式下受保护的主机必须满足以下先决条件：

-   **硬件**:需要用于初始部署一台主机。 若要测试的 HYPER-V 实时迁移受防护的 vm，必须具有至少两个主机。

    主机必须具有：
    
    - IOMMU 和二级地址转换 (SLAT)
    - TPM 2.0
    - 配有 UEFI 2.3.1 或更高版本
    - 配置为使用 （不 BIOS 或"旧"模式） 的 UEFI 启动
    - 已启用安全启动
        
-   **操作系统**:Windows Server 2016 Datacenter edition 或更高版本

    > [!IMPORTANT]
    > 请确保在安装[最新累积更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。  

-   **角色和功能**:HYPER-V 角色和主机保护者 HYPER-V 支持功能。 主机保护者 HYPER-V 支持功能才可在数据中心版的 Windows Server 上。 

> [!WARNING]
> 主机保护者 HYPER-V 支持功能，可能与某些设备不兼容的代码完整性的基于虚拟化的保护。 我们强烈建议启用此功能前在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。 有关详细信息，请参阅[与基于 Windows Server 虚拟化的代码完整性保护兼容的硬件](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：** 
> [!div class="nextstepaction"]
> [捕获 TPM 信息](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>主机密钥证明

使用主机密钥证明的受保护的主机必须满足以下先决条件：

- **硬件**:任何服务器，能够使用 Windows Server 2019 运行的 HYPER-V 开始
- **操作系统**:Windows Server 2019 Datacenter Edition
- **角色和功能**:HYPER-V 角色和主机保护者 HYPER-V 支持功能 

主机可以联接到域或工作组。 

对主机密钥证明，必须运行 Windows Server 2019 HGS 的大小和 v2 证明操作。 有关详细信息请参阅[HGS 先决条件](guarded-fabric-prepare-for-hgs.md#prerequisites)。 

**下一步：** 
> [!div class="nextstepaction"]
> [创建密钥对](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>受信任的管理员证明

>[!IMPORTANT]
>受信任的管理员证明 （AD 模式） 与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](#host-key-attestation)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 

HYPER-V 主机必须满足 AD 模式下的以下先决条件：

-   **硬件**:任何能够运行的 HYPER-V 开始使用 Windows Server 2016 的服务器。 需要用于初始部署一台主机。 若要测试的 HYPER-V 实时迁移受防护的 vm，你需要在至少两台主机。

-   **操作系统**:Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > 安装[最新累积更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色和功能**:HYPER-V 角色和主机保护者 HYPER-V 支持功能，这是仅在 Windows Server 2016 Datacenter edition 中可用。 

> [!WARNING]
> 主机保护者 HYPER-V 支持功能，可能与某些设备不兼容的代码完整性的基于虚拟化的保护。 我们强烈建议启用此功能前在实验室中测试此配置。 如果不这样做，可能会导致意外故障，其中包括数据丢失或蓝屏错误（也称为停止错误）。 有关详细信息，请参阅[与基于 Windows Server 2016 虚拟化的代码完整性保护兼容的硬件](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：** 
> [!div class="nextstepaction"]
> [安全组中置于受保护的主机](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)