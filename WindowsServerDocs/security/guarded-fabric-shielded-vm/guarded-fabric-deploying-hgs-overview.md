---
title: 部署主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c6b891cee905f917fab16869cd6deccc66d91a2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857418"
---
# <a name="deploying-the-host-guardian-service"></a>部署主机保护者服务 

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

提供托管的环境中的最重要目标之一是确保在环境中运行的虚拟机的安全。 作为云服务商或企业私有云管理员，你可以使用受保护的构造为 VM 提供更安全的环境。 受保护的结构包括一项主机保护者服务 (HGS)（通常是由三个节点组成的群集）、一个或多个被保护的主机以及一组受防护的虚拟机 (VM)。

## <a name="video-deploying-a-guarded-fabric"></a>视频：部署受保护的构造 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>部署任务的受保护的构造和受防护的 Vm

下表将分解部署受保护的构造和创建受防护的 Vm，根据不同管理员角色的任务。 请注意当 HGS 管理员已授权的 HYPER-V 主机使用配置 HGS，构造管理员将收集并提供有关主机在同一时间的标识信息。    

|<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-administrator-tasks.png" alt="Host Guardian Service administrator tasks" width="238" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-fabric-administrator-tasks.png" alt="Fabric administrator tasks" width="300" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-tenant-administrator-tasks.png" alt="Tenant administrator tasks" width="184" height="66" align="left" /> |
|-------------------------------------|--------------------------------|-----------------------------------------|
|<img src="../media/Guarded-Fabric-Shielded-VM/1111.png" alt="Step 1" hspace="8" align="left" /> [验证 HGS 先决条件](guarded-fabric-prepare-for-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 1" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/2222.png" alt="Step 2" hspace="8" align="left" /> [配置第一个 HGS&nbsp;节点](guarded-fabric-choose-where-to-install-hgs.md)&nbsp;<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png" alt="Step 2" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/3333.png" alt="Step 3" hspace="8" align="left" /> [配置其他 HGS&nbsp;节点](guarded-fabric-configure-additional-hgs-nodes.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png" alt="Step 3" hspace="8" align="right" />| | |
|<img src="../media/Guarded-Fabric-Shielded-VM/4444.png" alt="Step 4" hspace="8" align="left" /> [验证 HGS 配置](guarded-fabric-verify-hgs-configuration.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify-hgs-configuration.png" alt="Step 4" hspace="8" align="right" />| | |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/5555.png" alt="Step 5" hspace="8" align="left" /> [配置 DNS 结构](guarded-fabric-configuring-fabric-dns.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png" alt="Step 5" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/6666.png" alt="Step 6" hspace="8" align="left" /> [验证主机&nbsp;先决条件 （密钥）](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation)<br>[验证主机&nbsp;先决条件 (TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation)<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 6" hspace="8" align="right" />| |
|<img src="../media/Guarded-Fabric-Shielded-VM/8888.png" alt="Step 8" hspace="8" align="left" /> [配置 HGS 使用主机信息](guarded-fabric-add-host-information-to-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png" alt="Step 8" hspace="8" align="right" />|<img src="../media/Guarded-Fabric-Shielded-VM/7777.png" alt="Step 7" hspace="8" align="left" /> [创建主机密钥 （密钥）](guarded-fabric-create-host-key.md)<br>[收集主机信息 (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png" alt="Step 7" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/9999.png" alt="Step 9" hspace="8" align="left" /> [确认主机可以证明](guarded-fabric-confirm-hosts-can-attest-successfully.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png" alt="Step 9" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/101010.png" alt="Step 10" hspace="8" align="left" /> [配置 VMM （可选）](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png" alt="Step 10" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/11eleven.png" alt="Step 11" hspace="8" align="left" /> [创建模板磁盘](guarded-fabric-create-a-shielded-vm-template.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png" alt="Step 11" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/121212.png" alt="Step 12" hspace="8" align="left" /> [创建为 VMM VM 防护帮助程序磁盘 （可选）](guarded-fabric-vm-shielding-helper-vhd.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png" alt="Step 12" hspace="8" align="right" />| |
| &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/131313.png" alt="Step 13" hspace="8" align="left" /> [设置 Windows Azure Pack （可选）](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png" alt="Step 13" hspace="8" align="right" />| |
| &nbsp; | &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/141414.png" alt="Step 14" hspace="8" align="left" /> [创建防护数据文件](guarded-fabric-tenant-creates-shielding-data.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png" alt="Step 14" hspace="8" align="right" />|
| &nbsp; | &nbsp; |<img src="../media/Guarded-Fabric-Shielded-VM/151515.png" alt="Step 15" hspace="8" align="left" /> [创建受防护的 Vm 使用 Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" /><br>[创建受防护的 Vm 使用 VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" />|


## <a name="see-also"></a>请参阅

- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
