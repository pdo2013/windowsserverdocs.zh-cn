---
title: 部署受保护的主机
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: bc79d13b4dda96cd3e760958a6310276d2c45bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386751"
---
# <a name="deploy-guarded-hosts"></a>部署受保护的主机

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本节中的主题描述了构造管理员配置 Hyper-v 主机以使用主机保护者服务（HGS）的步骤。 [必须先设置 HGS 群集](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)中的至少一个节点，然后才能开始这些步骤。

**对于受 TPM 信任的证明**：
1. [配置构造 DNS](guarded-fabric-configuring-fabric-dns.md)：说明如何设置从 fabric 域到 HGS 域的 DNS 转发器。
2. [捕获 HGS 所需的信息](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)：说明如何捕获 TPM 标识符（也称为平台标识符）、创建代码完整性策略，以及创建 TPM 基线。 然后，你将向 HGS 管理员提供此信息以配置证明。
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**对于主机密钥证明**：
1. [创建主机密钥](guarded-fabric-create-host-key.md#create-a-host-key)：说明如何设置从 fabric 域到 HGS 域的 DNS 转发器。
2. [向证明服务添加主机密钥](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service)：说明如何在 fabric 域中设置一个 Active Directory 安全组，将受保护的主机添加为该组的成员，并向 HGS 管理员提供该组标识符。 
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**对于管理员信任的证明**：
1. [配置构造 DNS](guarded-fabric-configuring-fabric-dns.md)：说明如何设置从 fabric 域到 HGS 域的 DNS 转发器。
2. [创建安全组](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)：说明如何在 fabric 域中设置一个 Active Directory 安全组，将受保护的主机添加为该组的成员，并向 HGS 管理员提供该组标识符。 
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>请参阅

- [受保护的构造和受防护的 Vm 的部署任务](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
