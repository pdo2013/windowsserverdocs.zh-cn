---
title: 查看用于 DNS 区域的 DNS 资源记录
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e4b5ecd87e4976c0c65403bd63180e1d659e40b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405638"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>查看用于 DNS 区域的 DNS 资源记录

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题在 IPAM 客户端控制台中查看 DNS 区域的 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>查看区域的 DNS 资源记录  
  
1.  在服务器管理器中，单击 " **IPAM**"。 IPAM 客户端控制台随即出现。  
  
2.  在导航窗格的 "**监视和管理**" 中，单击 " **DNS 区域**"。  导航窗格划分为一个上部导航窗格和一个较低的导航窗格。  
  
3.  在下部导航窗格中，单击 "**正向查找**"，然后展开 "域和区域" 列表以查找并选择要查看的区域。 例如，如果你有一个名为都柏林的区域，请单击 "**都柏林**"。  
  
    ![选择要查看的区域](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  在 "显示" 窗格中，默认视图为区域的 DNS 服务器。 若要更改视图，请单击 "**当前视图**"，然后单击 "**资源记录**"。  
  
    ![将视图更改为资源记录](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  将显示该区域的 DNS 资源记录。 若要筛选记录，请在 "**筛选器**" 中键入要查找的文本。  
  
    ![键入文本以筛选记录](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  若要按记录类型、访问作用域或其他条件筛选资源记录，请单击 "**添加条件**"，然后从条件列表中进行选择，然后单击 "**添加**"。  
  
    ![使用条件筛选记录](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


