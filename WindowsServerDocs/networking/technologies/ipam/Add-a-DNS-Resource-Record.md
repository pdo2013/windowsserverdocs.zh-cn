---
title: 添加 DNS 资源记录
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 868b27a2a2b2005c3cf54d544d2534ae66ae0d98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849478"
---
# <a name="add-a-dns-resource-record"></a>添加 DNS 资源记录

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题使用 IPAM 客户端控制台添加一个或多个新的 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-add-a-dns-resource-record"></a>若要添加的 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 区域**。  在导航窗格划分为上部导航窗格和导航窗格下半部分。  
  
3.  在下部导航窗格中，单击**正向查找**。 显示窗格搜索结果中将显示所有 IPAM 管理 DNS 正向查找区域。 右键单击你想要添加的资源记录，并单击的区域**添加 DNS 资源记录**。  
  
    ![添加 DNS 资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  **添加 DNS 资源记录**对话框随即打开。 在中**资源记录属性**，单击**DNS 服务器**并选择你想要添加一个或多个新的资源记录的 DNS 服务器。 在中**配置 DNS 资源记录**，单击**新建**。  
  
    ![将 DNS 资源记录配置](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  该对话框将展开以显示**新建资源记录**。 单击**资源记录类型**。  
  
    ![资源记录类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  显示资源记录类型的列表。 单击你想要添加的资源记录类型。  
  
    ![选择要添加的记录类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  在中**新建资源记录，** 中**名称**，键入资源记录名称。 在中**IP 地址**，键入 IP 地址，并选择适合你的部署的资源记录属性。 单击**资源记录添加**。  
  
    ![添加资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  如果不想创建更多的新资源记录，请单击**确定**。 如果你想要创建其他新的资源记录，请单击**新建**。  
  
    ![单击确定，或在新](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. 该对话框将展开以显示**新建资源记录**。 单击**资源记录类型**。 显示资源记录类型的列表。 单击你想要添加的资源记录类型。  
  
10. 在中**新建资源记录，** 中**名称**，键入资源记录名称。 在中**IP 地址**，键入 IP 地址，并选择适合你的部署的资源记录属性。 单击**资源记录添加**。  
  
    ![添加资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. 如果你想要添加更多资源记录，用于创建记录重复该过程。 完成后创建新资源记录，请单击**应用**。  
  
    ![创建完整的资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. **添加资源记录**对话框的显示资源记录摘要在 IPAM 中指定的 DNS 服务器上创建的资源记录。 已成功创建了记录，当**状态**记录的是**成功**。  
  
    ![记录添加状态](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. 单击 **“确定”**。  
  
## <a name="see-also"></a>请参阅  
[DNS 资源记录管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


