---
title: 查看 DNS 资源记录 DNS 区域
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 786c1ee8fd673bd17465ab9586dd1e0bcfd7971c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>查看 DNS 资源记录 DNS 区域

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题中 IPAM 客户端控制台查看 DNS 资源 DNS 区域记录。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>若要查看为某个区域 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 区域**。  导航窗格中将分为右上导航窗格和低导航窗格。  
  
3.  在较低的导航窗格中，单击**向前查找**，然后展开域和区域列表，以查找并选择你想要查看的区域。 例如，如果你有 dublin 名为某个区域，请单击**都柏林**。  
  
    ![选择你想要查看的区域](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  在显示窗格中，默认视图是区域 DNS 服务器。 若要更改视图，请单击**当前视图**，然后单击**资源记录**。  
  
    ![更改视图资源记录](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  显示该区域 DNS 资源记录。 若要筛选的记录，键入你想要查找中的文本**筛选器**。  
  
    ![筛选记录对键入文本](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  若要筛选通过记录类型、 访问范围或其他条件资源记录，请单击**添加条件**，并在条件列表中进行选择，然后单击**添加**。  
  
    ![使用筛选记录条件](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


