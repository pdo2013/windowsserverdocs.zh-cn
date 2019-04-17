---
title: 删除 DNS 资源记录
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>删除 DNS 资源记录

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以使用 IPAM 客户端控制台删除一个或多个 DNS 资源记录。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-delete-dns-resource-records"></a>若要删除的资源 DNS 记录  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 区域**。  导航窗格中将分为右上导航窗格和低导航窗格。  
  
3.  单击以展开**向前查找**和你想要删除的区域和资源记录所在的域。 单击该区域，在和显示窗格中，单击**当前视图**。 单击**资源记录**。  
  
4.  在显示窗格中，查找并选择你想要删除的资源记录。  
  
    ![选择要删除的资源记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  右键单击所选的记录，然后单击**删除 DNS 资源记录**。  
  
    ![删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  **删除 DNS 资源记录**对话框中打开。 验证选择了正确的 DNS 服务器。 如果不存在，请单击**DNS 服务器**，然后选择你想要删除的资源记录的服务器。 单击**确定**。 IPAM 从 DNS 服务器中删除的资源记录。  
  
    ![验证选择了正确的 DNS 服务器和删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 资源录制管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


