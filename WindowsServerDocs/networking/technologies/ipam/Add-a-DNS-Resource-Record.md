---
title: 添加 DNS 资源记录
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f8fd9974ad1670ae4106c5c38470fa51b53cf4f5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405725"
---
# <a name="add-a-dns-resource-record"></a>添加 DNS 资源记录

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题通过使用 IPAM 客户端控制台来添加一个或多个新的 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-add-a-dns-resource-record"></a>添加 DNS 资源记录  
  
1.  在服务器管理器中，单击 " **IPAM**"。 IPAM 客户端控制台随即出现。  
  
2.  在导航窗格的 "**监视和管理**" 中，单击 " **DNS 区域**"。  导航窗格划分为一个上部导航窗格和一个较低的导航窗格。  
  
3.  在下部导航窗格中，单击 "**正向查找**"。 所有 IPAM 管理的 DNS 正向查找区域都显示在 "显示" 窗格搜索结果中。 右键单击要添加资源记录的区域，然后单击 "**添加 DNS 资源记录**"。  
  
    ![添加 DNS 资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  此时将打开 "**添加 DNS 资源记录**" 对话框。 在 "**资源记录属性**" 中，单击 " **dns 服务器**"，然后选择要在其中添加一个或多个新资源记录的 dns 服务器。 在 "**配置 DNS 资源记录**" 中，单击 "**新建**"。  
  
    ![配置 DNS 资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  该对话框将展开以显示**新的资源记录**。 单击 "**资源记录类型**"。  
  
    ![资源记录类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  将显示资源记录类型的列表。 单击要添加的资源记录类型。  
  
    ![选择要添加的记录类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  在 **"新建资源记录"** 的 "**名称**" 中，键入资源记录名称。 在 " **Ip 地址**" 中键入 ip 地址，然后选择适用于你的部署的资源记录属性。 单击 "**添加资源记录**"。  
  
    ![添加资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  如果不想创建其他新的资源记录，请单击 **"确定"** 。 如果要创建其他新的资源记录，请单击 "**新建**"。  
  
    ![单击 "确定" 或 "新建"](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. 该对话框将展开以显示**新的资源记录**。 单击 "**资源记录类型**"。 将显示资源记录类型的列表。 单击要添加的资源记录类型。  
  
10. 在 **"新建资源记录"** 的 "**名称**" 中，键入资源记录名称。 在 " **Ip 地址**" 中键入 ip 地址，然后选择适用于你的部署的资源记录属性。 单击 "**添加资源记录**"。  
  
    ![添加资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. 如果要添加更多的资源记录，请重复此过程以创建记录。 创建完新的资源记录后，请单击 "**应用**"。  
  
    ![完成资源记录创建](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. 当 IPAM 在指定的 DNS 服务器上创建资源记录时，"**添加资源记录**" 对话框将显示 "资源记录摘要"。 成功创建记录后，记录的**状态**为 "**成功**"。  
  
    ![记录添加状态](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. 单击 **“确定”** 。  
  
## <a name="see-also"></a>请参阅  
[DNS 资源记录管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


