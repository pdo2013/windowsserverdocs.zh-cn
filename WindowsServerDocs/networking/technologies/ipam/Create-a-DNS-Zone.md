---
title: 创建 DNS 区域
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
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31dbae80882e1375e548d5f6942c6473786f838f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870218"
---
# <a name="create-a-dns-zone"></a>创建 DNS 区域

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于使用 IPAM 客户端控制台中创建 DNS 区域。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-create-a-dns-zone"></a>若要创建 DNS 区域  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 和 DHCP 服务器**。 在显示窗格中单击**服务器类型**，然后单击**DNS**。 在搜索结果中列出了由 IPAM 管理的所有 DNS 服务器。  
  
3.  找到你想要添加一个区域的服务器，然后右键单击该服务器。  单击**创建 DNS 区域**。  
  
    ![创建 DNS 区域](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  **创建 DNS 区域**对话框随即打开。 在中**常规属性**，选择某一区域分类，一种区域类型，并输入中的名称**区域名称**。 此外选择适合你的部署中的值**高级属性**，然后单击**确定**。  
  
    ![高级的属性](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


