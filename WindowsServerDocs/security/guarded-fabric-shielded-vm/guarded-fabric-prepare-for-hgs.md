---
title: 查看 HGS 先决条件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9a668a39990b79862b99c2c7d9aeaf6540fa376d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447373"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>查看主机保护者服务的先决条件

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


本主题介绍 HGS 先决条件和准备 HGS 部署的初始步骤。

## <a name="prerequisites"></a>系统必备 

-   **硬件**:HGS 可以运行在物理计算机或虚拟机，但建议使用物理计算机。

    如果你想要运行 HGS 作为 （适用于可用性） 的三节点物理群集，必须具有三个物理服务器。 （作为聚类分析的最佳做法，三个服务器应具有非常相似的硬件。）
  
-   **操作系统**:主机密钥证明需要 Windows Server 2019 Standard 或 Datacenter edition 运行时带有[v2 证明](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies)。 对于基于 TPM 的证明，HGS 可以运行 Windows Server 2019 或 Windows Server 2016、 Standard 或 Datacenter edition。

-   **服务器角色**:主机保护者服务和支持服务器角色。

-   **配置权限/特权 fabric （主机） 域**:你将需要配置 DNS 转发 fabric （主机） 域和 HGS 域之间。 
    
## <a name="upgrading-hgs"></a>升级 HGS

如果你已部署 HGS，并想要其操作系统升级，请按照[升级指南](guarded-fabric-upgrade-to-2019.md)你 HGS 和 HYPER-V 的服务器升级到最新的操作系统。

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [获取用于 HGS 证书](guarded-fabric-obtain-certs.md)