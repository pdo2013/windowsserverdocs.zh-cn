---
title: 部署受保护的主机
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845348"
---
# <a name="deploy-guarded-hosts"></a>部署受保护的主机

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

在本部分中的主题介绍构造管理员配置的 HYPER-V 主机能够使用主机保护者服务 (HGS) 所需的步骤。 在开始这些步骤中中的至少一个节点之前[HGS 群集必须设置](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)。

**为受信任的 TPM 证明**:
1. [配置结构 DNS](guarded-fabric-configuring-fabric-dns.md):介绍如何设置 DNS 转发器从 fabric 域到 HGS 域。
2. [捕获所需的 HGS 信息](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md):指示如何捕获 TPM 标识符 （也称为平台标识符）、 创建代码完整性策略，并创建 TPM 基线。 然后您将向 HGS 管理员若要配置证明提供此信息。
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**对主机密钥证明**:
1. [创建主机密钥](guarded-fabric-create-host-key.md#create-a-host-key):介绍如何设置 DNS 转发器从 fabric 域到 HGS 域。
2. [将主机密钥添加到证明服务](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service):介绍如何在构造域中，设置 Active Directory 安全组将受保护的主机添加为该组的成员并向 HGS 管理员提供的组标识符。 
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**为受信任的管理员证明**:
1. [配置结构 DNS](guarded-fabric-configuring-fabric-dns.md):介绍如何设置 DNS 转发器从 fabric 域到 HGS 域。
2. [创建安全组](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md):介绍如何在构造域中，设置 Active Directory 安全组将受保护的主机添加为该组的成员并向 HGS 管理员提供的组标识符。 
3. [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>请参阅

- [部署任务的受保护的构造和受防护的 Vm](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
