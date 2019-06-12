---
title: 部署受防护的虚拟机
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9badbfcb709c29451425aaecc56b46ac98837e18
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443800"
---
# <a name="deploy-shielded-vms"></a>部署受防护的虚拟机


>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

以下主题介绍如何租户才能使用受防护的 Vm。

1. （可选）[创建 Windows 模板磁盘](guarded-fabric-create-a-shielded-vm-template.md)或[创建 Linux 模板磁盘](guarded-fabric-create-a-linux-shielded-vm-template.md)。 可以通过租户或托管服务提供商创建的模板磁盘。 

2. （可选）[将现有 Windows VM 转换为受防护的 VM](guarded-fabric-vm-shielding-helper-vhd.md)。 

3. [创建防护数据来定义受防护的 VM](guarded-fabric-tenant-creates-shielding-data.md)。

    有关描述和关系图的防护数据文件，请参阅[什么屏蔽数据和为何有必要吗？](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    有关创建答案文件在受防护的数据文件中包含的信息，请参阅[受防护的 Vm-通过使用新建 ShieldingDataAnswerFile 函数生成一个答案文件](guarded-fabric-sample-unattend-xml-file.md)。

4. 创建受防护的 VM:
 
    - 使用**Windows Azure 包**:[使用 Windows Azure Pack 部署受防护的 VM](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - 使用**Virtual Machine Manager**:[通过使用 Virtual Machine Manager 中部署受防护的 VM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [创建受防护的 VM 模板](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>请参阅

- [受保护的结构和受防护的 VM](guarded-fabric-and-shielded-vms-top-node.md)
