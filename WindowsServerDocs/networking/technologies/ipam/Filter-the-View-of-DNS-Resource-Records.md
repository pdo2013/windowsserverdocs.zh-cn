---
title: 筛选 DNS 资源记录视图
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fed5e1f923d3560b91f514d1e59d79b847557c8b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847798"
---
# <a name="filter-the-view-of-dns-resource-records"></a>筛选 DNS 资源记录视图

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于筛选视图，IPAM 客户端控制台中的 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>若要筛选的 DNS 资源记录视图  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 区域**。  在导航窗格划分为上部导航窗格和导航窗格下半部分。  
  
3.  在下部导航窗格中，单击**正向查找**。 显示窗格搜索结果中将显示所有 IPAM 管理 DNS 正向查找区域。  
  
4.  单击该区域来查看和筛选器所需的记录。  
  
5.  在显示窗格中单击**当前视图**，然后单击**资源记录**。 在显示窗格中显示该区域的资源记录。  
  
6.  在显示窗格中单击**添加条件**。  
  
    ![添加条件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  从下拉列表中选择条件。 例如，如果你想要查看某个特定记录类型时，请单击**记录类型**。  
  
    ![选择条件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  单击**添加**。  
  
    ![添加条件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **记录类型**添加作为搜索参数。 文本框中输入你想要查找的记录的类型。 例如，如果你想要查看仅 SRV 记录，请键入**SRV**。  
  
    ![指定你想要查找的记录的类型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. 按 Enter。 DNS 资源记录根据条件筛选和搜索指定的短语。  
  
    ![运行筛选器](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 资源记录管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


