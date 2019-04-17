---
title: 管理多个 Active Directory 林资源
description: 本主题介绍的 IP 地址管理 (IPAM) 管理指南中的 Windows Server 2016 的一部分。
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
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>管理多个 Active Directory 林资源

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何使用 IPAM 管理域控制器 DHCP 服务器，在多个 Active Directory 树林中的 DNS 服务器。  
  
若要使用 IPAM 管理远程 Active Directory 森林中的资源，你希望管理每个森林必须有两种方式与装有 IPAM 森林信任。  
  
若要开始个不同的 Active Directory 林发现过程，打开服务器管理器，然后单击 IPAM。 在客户端 IPAM 控制台中，单击**配置服务器发现**，然后单击**获取林**。 这将启动发现受信任的森林以及它们的域的后台任务。 发现过程完成后，单击**配置服务器发现**，这将打开下面的对话框。  
  
![配置服务器搜索](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>对于基于组按预配 Active Directory 跨森林方案，请确保 IPAM 服务器上，而不是在信任域域控制器，运行以下 Windows PowerShell cmdlet。 例如，如果 IPAM 服务器加入森林 corp.contoso.com 并且信任林 fabrikam.com，可以运行以下 Windows PowerShell cmdlet 中的组按基于上 fabrikam.com 森林预配 corp.contoso.com IPAM 服务器上。 若要运行此 cmdlet，你必须域管理员组 fabrikam.com 森林中的成员。

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

在**配置服务器发现**对话框中，单击**选择森林**，然后选择你想要管理与 IPAM 森林。 选择你想要管理，然后单击域**添加**。

在**选择服务器角色发现**，为你想要管理每个域，指定的服务器来发现类型。 选项包括**域控制器**，**DHCP 服务器**，并**DNS 服务器**。

默认情况下，域控制器，DHCP 服务器和 DNS 服务器发现，，因此如果你不希望发现一个这些类型的服务器，请确保你取消选择该选项的复选框。

在上方的示例图中，contoso.com 林中安装 IPAM 服务器，并根域的 fabrikam.com 森林添加 IPAM 管理。 选定的服务器角色允许 IPAM 发现和管理域控制器，DHCP 服务器 fabrikam.com 根域和 contoso.com 根域中的 DNS 服务器。

森林、域和服务器角色指定后，单击**确定**。 IPAM 探索和发现完成后，你可以管理本地边远森林中的资源。
