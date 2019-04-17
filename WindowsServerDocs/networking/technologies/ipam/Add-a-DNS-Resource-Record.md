---
title: 添加 DNS 资源记录
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e14a59e9f172b20e85a34d2299e3733a796adafc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-dns-resource-record"></a>添加 DNS 资源记录

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以使用 IPAM 客户端控制台添加一个或多个新 DNS 资源记录。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-add-a-dns-resource-record"></a>若要添加 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 区域**。  导航窗格中将分为右上导航窗格和低导航窗格。  
  
3.  在较低的导航窗格中，单击**向前查找**。 显示窗格中搜索结果中显示所有 IPAM 管理 DNS 向前查找区域。 右键单击你想要添加资源记录，然后单击的区域**添加 DNS 资源记录**。  
  
    ![添加 DNS 资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  **添加 DNS 资源记录**对话框中打开。 在**资源录制属性**，单击**DNS 服务器**，然后选择你想要添加的一个或多个新的资源记录 DNS 服务器。 在**配置 DNS 资源记录**，单击**新建**。  
  
    ![配置 DNS 资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  对话框中将展开以显示**新资源记录**。 单击**资源记录类型**。  
  
    ![资源录制类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  将显示资源记录类型的列表。 单击你想要添加的资源记录类型。  
  
    ![选择要添加的录制类型](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  在**新的资源记录**中**名称**，键入资源记录名称。 在**IP 地址**，键入 IP 地址，然后依次选择适合你的部署资源记录属性。 单击**添加资源记录**。  
  
    ![添加了资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  如果你不希望创建新的其他资源记录，请单击**确定**。 如果你想要创建新的其他资源记录，请单击**新建**。  
  
    ![单击确定新或](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. 对话框中将展开以显示**新资源记录**。 单击**资源记录类型**。 将显示资源记录类型的列表。 单击你想要添加的资源记录类型。  
  
10. 在**新的资源记录**中**名称**，键入资源记录名称。 在**IP 地址**，键入 IP 地址，然后依次选择适合你的部署资源记录属性。 单击**添加资源记录**。  
  
    ![添加了资源记录](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. 如果你想添加更多资源记录，请重复该过程，用于创建记录。 当你完成创建新的资源记录后时，单击**应用**。  
  
    ![完成资源录制创建](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. **添加资源记录**对话框中显示资源记录摘要，而在你要指定 DNS 服务器上的 IPAM 创建资源记录。 成功创建了记录，当**状态**记录的已**成功**。  
  
    ![记录添加状态](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. 单击**确定**。  
  
## <a name="see-also"></a>请参阅  
[DNS 资源录制管理](DNS-Resource-Record-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


