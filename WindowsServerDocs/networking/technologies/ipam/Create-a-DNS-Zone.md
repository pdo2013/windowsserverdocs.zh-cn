---
title: 创建一个 DNS 区域
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
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e532837e6c98694fa040a6d47a8e536eecb4c3da
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-dns-zone"></a>创建一个 DNS 区域

>适用于：Windows Server（半年通道），Windows Server 2016

本主题可用于创建 DNS 区域时使用 IPAM 客户端控制台。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-create-a-dns-zone"></a>若要创建 DNS 区域  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 和 DHCP 服务器**。 在显示窗格中，单击**服务器类型**，然后单击**DNS**。 在搜索结果中列出由 IPAM 的所有 DNS 服务器。  
  
3.  找到你想要添加的区域，请的服务器，并右键单击的服务器。  单击**创建 DNS 区域**。  
  
    ![创建 DNS 区域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  **创建 DNS 区域**对话框中打开。 在**常规属性**、 选择一个时区类别，区域类型和中输入名称**区域名称**。 此外可以选择适用于在你部署值**高级属性**，然后单击**确定**。  
  
    ![高级的属性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


