---
title: 查看 HGS 先决条件
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9024557dd42ede27144bf10aa5873b6bb12d585c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403487"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>查看主机保护者服务的先决条件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


本主题介绍了用于准备 HGS 部署的 HGS 先决条件和初始步骤。

## <a name="prerequisites"></a>先决条件 

-   **硬件**：可以在物理计算机或虚拟机上运行 HGS，但建议使用物理计算机。

    如果要将 HGS 作为三节点物理群集运行（以实现可用性），则必须具有三个物理服务器。 （作为群集的最佳做法，这三个服务器应该具有非常相似的硬件。）
  
-   **操作系统**:主机密钥证明需要 Windows Server 2019 Standard 或 Datacenter edition，操作[v2 证明](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies)。 对于基于 TPM 的证明，HGS 可以运行 Windows Server 2019 或 Windows Server 2016 （Standard 或 Datacenter edition）。

-   **服务器角色**：主机保护者服务和支持服务器角色。

-   **Fabric （主机）域的配置权限/特权**：你将需要在结构（主机）域和 HGS 域之间配置 DNS 转发。 
    
## <a name="upgrading-hgs"></a>升级 HGS

如果已部署了 HGS 并想要升级其操作系统，请按照[升级指南](guarded-fabric-upgrade-to-2019.md)将 HGS 和 hyper-v 服务器升级到最新的操作系统。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [获取 HGS 证书](guarded-fabric-obtain-certs.md)