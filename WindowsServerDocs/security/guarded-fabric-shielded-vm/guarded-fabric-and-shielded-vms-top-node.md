---
title: 受保护的构造和受防护的 VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857428"
---
# <a name="guarded-fabric-and-shielded-vms"></a>受保护的构造和受防护的 VM

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

提供托管的环境中的最重要目标之一是确保在环境中运行的虚拟机的安全。 作为云服务商或企业私有云管理员，你可以使用受保护的构造为 VM 提供更安全的环境。 受保护的结构包括一项主机保护者服务 (HGS)（通常是由三个节点组成的群集）、一个或多个被保护的主机以及一组受防护的虚拟机 (VM)。

> [!IMPORTANT]
> 请确保在部署在生产环境中受防护的虚拟机之前已安装最新的累积更新。

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>视频、 博客及概述主题关于受保护的构造和受防护的 Vm

- 视频：[如何从与 Windows Server 2019 内部人员威胁保护在虚拟化结构](https://myignite.techcommunity.microsoft.com/sessions/64690)
- 视频：[Windows Server 2016 中受防护的虚拟机简介](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- 视频：[深入了解受防护的 Vm 使用 Windows Server 2016 的 HYPER-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- 视频：[部署受防护的 Vm 和受保护的构造与 Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- 博客：[数据中心和私有云安全博客](https://blogs.technet.microsoft.com/datacentersecurity/)
- 概述：[受保护的构造和受防护的 Vm 概述](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>规划主题

- [宿主须知的规划指南](guarded-fabric-planning-for-hosters.md)
- [对于租户的规划指南](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>部署主题

- [部署指南](guarded-fabric-deploying-hgs-overview.md)
    - [快速入门](guarded-fabric-deployment-overview.md)
    - [部署 HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [部署受保护的主机](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [将成为受保护的主机的主机的配置结构 DNS](guarded-fabric-configuring-fabric-dns.md)
        - [部署受保护的主机使用 AD 模式](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [部署受保护的主机使用 TPM 模式](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [确认受保护的主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [受防护的 Vm 托管服务提供商将部署在 VMM 中受保护的主机](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [创建受防护的 VM 模板](guarded-fabric-create-a-shielded-vm-template.md)
        - [准备 VM 防护帮助程序 VHD](guarded-fabric-vm-shielding-helper-vhd.md)
        - [设置 Windows Azure 包](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [创建防护数据文件](guarded-fabric-tenant-creates-shielding-data.md)
        - [使用 Windows Azure Pack 部署受防护的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [通过使用 Virtual Machine Manager 中部署受防护的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>操作和管理主题

- [管理主机保护者服务](guarded-fabric-manage-hgs.md)
