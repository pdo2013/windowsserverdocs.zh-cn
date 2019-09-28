---
title: 删除 DNS 资源记录
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcaaf89989a44cc7a0e84e296ce16083fe021577
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405656"
---
# <a name="delete-dns-resource-records"></a>删除 DNS 资源记录

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题通过使用 IPAM 客户端控制台来删除一个或多个 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-delete-dns-resource-records"></a>删除 DNS 资源记录  
  
1.  在服务器管理器中，单击 " **IPAM**"。 IPAM 客户端控制台随即出现。  
  
2.  在导航窗格的 "**监视和管理**" 中，单击 " **DNS 区域**"。  导航窗格划分为一个上部导航窗格和一个较低的导航窗格。  
  
3.  单击以展开 "**正向查找**" 以及要删除的区域和资源记录所在的域。 单击该区域，然后在 "显示" 窗格中，单击 "**当前视图**"。 单击 "**资源记录**"。  
  
4.  在 "显示" 窗格中，找到并选择要删除的资源记录。  
  
    ![选择要删除的资源记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  右键单击所选记录，然后单击 "**删除 DNS 资源记录**"。  
  
    ![删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  此时将打开 "**删除 DNS 资源记录**" 对话框。 验证是否选择了正确的 DNS 服务器。 如果不是，请单击 " **DNS 服务器**"，然后选择要从中删除资源记录的服务器。 单击 **“确定”** 。 IPAM 将删除 DNS 服务器中的资源记录。  
  
    ![验证是否选择了正确的 DNS 服务器并删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 资源记录管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


