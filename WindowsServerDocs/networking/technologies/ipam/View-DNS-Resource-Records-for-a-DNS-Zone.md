---
title: 查看用于 DNS 区域的 DNS 资源记录
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44db34199257367e98279ccbcbc2d5041ee9884c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283805"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>查看用于 DNS 区域的 DNS 资源记录

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于在 IPAM 客户端控制台中查看 DNS 区域的 DNS 资源记录。  
  
Administrators  组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>若要查看的区域的 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 区域**。  在导航窗格划分为上部导航窗格和导航窗格下半部分。  
  
3.  在下部导航窗格中，单击**正向查找**，然后展开要找到并选择你想要查看的区域的域和区域的列表。 例如，如果您具有名为 dublin 的区域，单击**dublin**。  
  
    ![选择你想要查看的区域](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  在显示窗格中的默认视图是该区域的 DNS 服务器。 若要更改的视图，请单击**当前视图**，然后单击**资源记录**。  
  
    ![将视图更改为资源记录](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  显示区域的 DNS 资源记录。 若要筛选的记录，请键入你想要在中查找的文本**筛选器**。  
  
    ![键入筛选器记录的文本](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  若要筛选的记录类型、 访问作用域或其他条件的资源记录，请单击**添加条件**，然后在条件列表中进行选择并单击**添加**。  
  
    ![使用筛选器记录的条件](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


