---
title: 为 DNS 资源记录设置访问范围
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2e63c82ef0c58a9b4392ad8b9b1fc896d075ab71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882908"
---
# <a name="set-access-scope-for-dns-resource-records"></a>为 DNS 资源记录设置访问范围

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题使用 IPAM 客户端控制台设置的访问作用域的 DNS 资源记录。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>若要设置访问作用域的 DNS 资源记录  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，单击**DNS 区域**。  在下部导航窗格中，展开**正向查找**并浏览到并选择包含你想要更改其访问作用域的资源记录的区域。  
  
3.  在显示窗格中，找到并选择你想要更改其访问作用域的资源记录。  
  
    ![选择的资源记录](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  右键单击所选的 DNS 资源记录，然后依次**设置访问作用域**。  
  
    ![设置访问作用域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  **设置访问作用域**对话框随即打开。 如果需要为你的部署，单击要取消选定**从父级继承访问作用域**。 在中**选择访问作用域**，选择一个项，然后单击**确定**。  
  
    ![选择访问作用域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


