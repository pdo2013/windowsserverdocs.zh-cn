---
title: 使用管理员信任的证明初始化 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402358"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>使用管理员信任的证明初始化 HGS

>适用于：Windows Server（半年频道）、Windows Server 2016

>[!IMPORTANT]
>从 Windows Server 2019 开始，已弃用管理受信任的证明（AD 模式）。 对于不可能进行 TPM 证明的环境，请配置[主机密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明向 AD 模式提供类似的保障，并更易于设置。 


这些步骤取决于你是在新林中还是在现有堡垒林中初始化 HGS：

1. [在新林中初始化 HGS 群集（默认值）](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -或者-

   [初始化现有堡垒林中的 HGS 群集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [在 fabric 域中配置 DNS 转发](guarded-fabric-configuring-fabric-dns.md)

3. [在 HGS 域中配置 DNS 转发和单向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)



