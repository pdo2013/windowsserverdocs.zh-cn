---
title: 使用受 TPM 信任的证明初始化 HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c67e15aa011dbff0be5d428c8012a161d9eaea2f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386604"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>使用受 TPM 信任的证明初始化 HGS

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

这些步骤取决于你是在新林中还是在现有堡垒林中初始化 HGS：

1. [在新林中初始化 HGS 群集（默认值）](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   -或者-

   [初始化现有堡垒林中的 HGS 群集](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [安装受信任的 TPM 根证书](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [配置构造 DNS](guarded-fabric-configuring-fabric-dns.md)

