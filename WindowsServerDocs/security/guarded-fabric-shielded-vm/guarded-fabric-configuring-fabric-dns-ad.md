---
title: 为受保护的主机 (AD) 配置 fabric DNS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9d302dcd06b7a3a40afbb6f613c39caaabbeba91
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443724"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>为受保护的主机配置 fabric DNS

>适用于：Windows Server 2016


>[!IMPORTANT]
>AD 模式与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 

构造管理员需要配置 DNS 允许受保护的主机，若要解决 HGS 群集所需的构造。 HGS 群集必须处于[HGS 管理员设置](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [配置 HGS DNS 和单向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)
