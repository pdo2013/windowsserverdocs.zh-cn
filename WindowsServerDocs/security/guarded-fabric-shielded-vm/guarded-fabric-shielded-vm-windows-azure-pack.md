---
title: 租户-使用 Windows Azure Pack 部署受防护的 VM 的受防护的 Vm
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e0372cb5b1f891bb724f246a3f8a7931619ce7ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847188"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>租户-使用 Windows Azure Pack 部署受防护的 VM 的受防护的 Vm

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

如果你的托管服务提供商支持，可以使用 Windows Azure Pack 部署受防护的 VM。

完成以下步骤：

<!-- When we have a link to the topic about how tenants subscribe, add that link as an indented item just under step 1 below. -->

1. 订阅 Windows Azure Pack 中提供的一个或多个计划。

2. 使用 Windows Azure Pack 创建受防护的 VM。

    [使用受防护的虚拟机](https://technet.microsoft.com/library/mt720674.aspx)，以下主题中所述：

    - [创建屏蔽数据](https://technet.microsoft.com/library/mt720672.aspx)（和上载屏蔽数据文件，如本主题中的第二个过程中所述）。
    
    > [!NOTE]
    > 作为创建屏蔽数据的一部分，将下载保护者密钥文件，这将为 utf-8 格式的 XML 文件。 未更改该文件为 utf-16。
    
    - [创建受防护的虚拟机](https://technet.microsoft.com/library/mt720673.aspx)-可以通过**快速创建**、 通过受防护的模板，或通过常规模板。
    
        > [!WARNING]
        > 如果您[使用常规模板创建受防护的虚拟机](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2)，务必要注意，预配 VM*非屏蔽*。 这意味着，与在防护数据文件中，受信任的磁盘列表未验证的模板磁盘也不屏蔽数据文件中的机密用于预配 VM。 如果有可用的受防护的模板，最好是部署受防护的 VM 与受防护的模板来提供端到端保护的机密。
    
    - [将第 2 代虚拟机转换为受防护的虚拟机](https://technet.microsoft.com/library/mt720670.aspx)
    
        > [!NOTE]
        > 如果虚拟机转换为受防护的虚拟机后，不加密现有检查点和备份。 应删除旧的检查点时可以防止访问旧的、 已解密数据。

## <a name="see-also"></a>请参阅

- [托管服务提供程序的配置步骤受保护的主机和受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受保护的构造和受防护的 Vm](guarded-fabric-and-shielded-vms-top-node.md)
