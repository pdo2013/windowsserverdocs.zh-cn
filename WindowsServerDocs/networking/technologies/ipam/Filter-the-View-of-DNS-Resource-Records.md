---
title: 筛选 DNS 资源记录视图
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>筛选 DNS 资源记录视图

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题筛选在 IPAM 客户端控制台 DNS 资源记录视图。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>若要筛选 DNS 资源记录视图  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 区域**。  导航窗格中将分为右上导航窗格和低导航窗格。  
  
3.  在较低的导航窗格中，单击**向前查找**。 显示窗格中搜索结果中显示所有 IPAM 管理 DNS 向前查找区域。  
  
4.  单击该区域所需视图和筛选器其的记录。  
  
5.  在显示窗格中，单击**当前视图**，然后单击**资源记录**。 显示窗格中所示的区域的资源记录。  
  
6.  在显示窗格中，单击**添加条件**。  
  
    ![添加条件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  从下拉列表中选择标准。 例如，如果你想要在查看特定记录类型，请单击**记录类型**。  
  
    ![选择标准](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  单击**添加**。  
  
    ![添加条件](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **录制类型**添加为搜索参数。 文本输入你想要查找的记录的类型。 例如，如果你想要仅查看 SRV 记录时，键入**SRV**。  
  
    ![指定你想要查找的记录的类型](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. 按 ENTER。 DNS 资源记录筛选根据条件，并且搜索你所指定的短语。  
  
    ![运行筛选器](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 资源录制管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


