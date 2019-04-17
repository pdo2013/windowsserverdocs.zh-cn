---
title: 编辑 DNS 区域
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
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3e7cc75017c2b59293a042d4af0a677d3eda46c0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="edit-a-dns-zone"></a>编辑 DNS 区域

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题编辑在 IPAM 客户端控制台 DNS 区域。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-edit-a-dns-zone"></a>若要编辑 DNS 区域  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，在**监视器和管理**，单击**DNS 区域**。 导航窗格中将分为右上导航窗格和低导航窗格。  
  
3.  在较低导航窗格中，请选择以下选项之一：  
  
    -   向前查找  
  
    -   IPv4 反向查找  
  
    -   IPv6 反向查找  
  
4.  例如，选择 IPv4 反向搜索。  
  
    ![选择一个时区类型](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  在显示窗格中，右键单击你想要编辑，然后单击区域**编辑 DNS 区域**。  
  
    ![编辑 DNS 区域](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  **编辑 DNS 区域**打开具有对话框**常规**选定的页面。 如果需要可编辑常规区域属性：**DNS 服务器**，**区域类别**，并**区域类型**，然后单击**应用**或编辑完成之后，如果**确定**。  
  
    ![编辑区域属性和保存](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  在**编辑 DNS 区域**对话框中，单击**高级**。 **高级**打开区域属性页。 如果需要可编辑你想要更改，然后单击属性**应用**或编辑完成之后，如果**确定**。  
  
    ![编辑高级的区域属性](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  如果需要选择其他区域的属性页名称（SOA 的名称服务器区域传输）、进行编辑，并单击**应用**或**确定**。 若要查看所有区域编辑，请单击**摘要**，然后单击**确定**。  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


