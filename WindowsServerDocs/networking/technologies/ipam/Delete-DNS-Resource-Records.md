---
title: 删除 DNS 资源记录
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d3c5f4f02cc1a8386bf12fe634620ba98f2f23a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883218"
---
# <a name="delete-dns-resource-records"></a>删除 DNS 资源记录

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于使用 IPAM 客户端控制台中删除一个或多个 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-delete-dns-resource-records"></a>若要删除 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 区域**。  在导航窗格划分为上部导航窗格和导航窗格下半部分。  
  
3.  单击此项可展开**正向查找**和你想要删除的区域和资源记录所在的域。 单击该区域，然后在显示窗格中，单击**当前视图**。 单击**资源记录**。  
  
4.  在显示窗格中，找到并选择你想要删除的资源记录。  
  
    ![选择要删除的资源记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  右键单击所选的记录，然后依次**删除 DNS 资源记录**。  
  
    ![删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  **删除 DNS 资源记录**对话框随即打开。 验证选择了正确的 DNS 服务器。 如果不存在，请单击**DNS 服务器**选择想要删除的资源记录的服务器。 单击 **“确定”**。 IPAM 从 DNS 服务器中删除的资源记录。  
  
    ![验证选择了正确的 DNS 服务器和删除记录](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 资源记录管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


