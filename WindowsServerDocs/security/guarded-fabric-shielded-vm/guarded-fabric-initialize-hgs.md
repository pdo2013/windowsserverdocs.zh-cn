---
title: 初始化 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865448"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>初始化主机保护者服务 (HGS)

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在初始化 HGS，您指定的 HGS 将用于测量受保护的主机的运行状况的模式。 有两个互相排斥的选项。 有关要选择的模式的背景信息，请参阅[受保护的构造和受防护的 VM 计划指南的托管商](guarded-fabric-planning-for-hosters.md)。

下列主题介绍了每种模式的部署步骤：

- [受信任的 TPM 证明 （TPM 模式）](guarded-fabric-initialize-hgs-tpm-mode.md)
- [主机密钥证明 （密钥模式）](guarded-fabric-initialize-hgs-key-mode.md)
- [受信任的管理员证明 （AD 模式）](guarded-fabric-initialize-hgs-ad-mode.md)

物理服务器上，应执行这些步骤。