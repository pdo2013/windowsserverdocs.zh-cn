---
title: 初始化使用受信任的管理员证明 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821828"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>初始化使用受信任的管理员证明 HGS

>适用于：Windows 服务器 （半年频道），Windows Server 2016

>[!IMPORTANT]
>受信任的管理员证明 （AD 模式） 与 Windows Server 2019 从开始已弃用。 对于 TPM 证明不可能的环境，配置[托管密钥证明](guarded-fabric-initialize-hgs-key-mode.md)。 主机密钥证明提供了类似保障为 AD 模式，并且易于设置。 


这些步骤会有所不同，具体取决于是否正在初始化 HGS 中新的林或现有的堡垒林：

1. [初始化 HGS 群集在新林中 （默认值）](guarded-fabric-initialize-hgs-ad-mode-default.md)

   -或者-

   [初始化在现有的堡垒林中 HGS 群集](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Fabric 域中配置 DNS 转发](guarded-fabric-configuring-fabric-dns.md)

3. [配置 DNS 转发和 HGS 域中的单向信任](guarded-fabric-configure-dns-forwarding-and-trust.md)



