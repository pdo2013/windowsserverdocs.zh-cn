---
title: 编辑 DNS 区域
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d688790828cb58b9fca2a17c95212c064532bb22
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282169"
---
# <a name="edit-a-dns-zone"></a>编辑 DNS 区域

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于编辑 IPAM 客户端控制台中的 DNS 区域。  
  
Administrators  组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-edit-a-dns-zone"></a>若要编辑 DNS 区域  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，在**监视和管理**，单击**DNS 区域**。 在导航窗格划分为上部导航窗格和导航窗格下半部分。  
  
3.  在下部导航窗格中，请选择下列选项之一：  
  
    -   正向查找  
  
    -   IPv4 反向查找  
  
    -   IPv6 反向查找  
  
4.  例如，选择 IPv4 反向查找。  
  
    ![选择区域类型](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  在显示窗格中，右键单击你想要编辑，然后单击该区域**编辑 DNS 区域**。  
  
    ![编辑 DNS 区域](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  **编辑 DNS 区域**对话框将打开带有**常规**选择页。 如果需要编辑常规区域属性：**DNS 服务器**，**区域类别**，和**区域类型**，然后单击**应用**或者，如果你的编辑都已完成，**确定**。  
  
    ![编辑区域属性并保存](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  在中**编辑 DNS 区域**对话框中，单击**高级**。 **高级**区域属性页随即打开。 如果需要编辑想要更改，并单击属性**Apply**或者，如果你的编辑都完成时，**确定**。  
  
    ![编辑高级的区域属性](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  如果需要请选择其他区域属性页名称 （名称服务器，SOA，区域传送），进行编辑，然后单击**Apply**或**确定**。 若要查看所有区域编辑，单击**摘要**，然后单击**确定**。  
  
## <a name="see-also"></a>请参阅  
[DNS 区域管理](DNS-Zone-Management.md)  
[管理 IPAM](Manage-IPAM.md)  
  


