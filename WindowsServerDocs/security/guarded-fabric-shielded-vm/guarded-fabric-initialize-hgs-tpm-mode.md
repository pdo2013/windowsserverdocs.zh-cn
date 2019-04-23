---
title: 初始化使用受信任的 TPM 证明的 HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 672054462437b615236dfc4f00c8b20c946f11a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882328"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>初始化使用受信任的 TPM 证明的 HGS

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

这些步骤会有所不同，具体取决于是否正在初始化 HGS 中新的林或现有的堡垒林：

1. [初始化 HGS 群集在新林中 （默认值）](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   -或者-

   [初始化在现有的堡垒林中 HGS 群集](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [安装受信任的 TPM 根证书](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [配置结构 DNS](guarded-fabric-configuring-fabric-dns.md)

