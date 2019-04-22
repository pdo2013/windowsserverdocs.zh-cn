---
title: 管理多个 Active Directory 林中的资源
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c6752d87be2e689e517b287092a2570e27943c18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815968"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>管理多个 Active Directory 林中的资源

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何使用 IPAM 管理的域控制器、 DHCP 服务器和多个 Active Directory 林的 DNS 服务器。  
  
若要使用 IPAM 管理在远程 Active Directory 林中的资源，你想要管理的每个林必须具有两个方法使用 IPAM 的安装位置的林信任。  
  
若要启动不同的 Active Directory 林发现过程，打开服务器管理器，然后单击 IPAM。 在 IPAM 客户端控制台中，单击**配置服务器发现**，然后单击**获取林**。 这会启动发现受信任的林和其域的后台任务。 发现过程完成后，单击**配置服务器发现**，这会打开以下对话框。  
  
![配置服务器发现](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>组策略\-基于 Active Directory 跨林方案的预配，请确保在 IPAM 服务器上并不在信任域的 Dc 上运行以下 Windows PowerShell cmdlet。 例如，如果 IPAM 服务器加入到林 corp.contoso.com，信任林是 fabrikam.com，您可以运行以下 Windows PowerShell cmdlet corp.contoso.com 中的 IPAM 服务器上为组策略\-基于上预配fabrikam.com 林。 若要运行此 cmdlet，必须是 fabrikam.com 林中的域管理员组的成员。

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

在中**配置服务器发现**对话框中，单击**选择林**，然后选择你想要使用 IPAM 管理的林。 此外选择你想要管理，然后单击的域**添加**。

在中**选择服务器角色发现**，对于每个你想要管理的域，指定要发现的服务器类型。 选项包括**域控制器**， **DHCP 服务器**，并**DNS 服务器**。

默认情况下，发现域控制器、 DHCP 服务器和 DNS 服务器-因此如果您不想要发现其中一种类型的服务器，请确保取消选中该选项的复选框。

在上面的示例图中，在 contoso.com 的林中，安装了 IPAM 服务器和 fabrikam.com 林根级域添加有关 IPAM 管理。 所选的服务器角色允许 IPAM 发现和管理域控制器、 DHCP 服务器和 fabrikam.com 根域和 contoso.com 的根域中的 DNS 服务器。

指定目录林、 域和服务器角色后，单击**确定**。 IPAM 执行发现，并完成发现后，可以管理本地和远程林中的资源。
