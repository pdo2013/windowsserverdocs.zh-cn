---
title: 为 DNS 资源记录设置访问范围
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b1790f2cbf84fd68f33ca30d2fe7663dde824240
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405642"
---
# <a name="set-access-scope-for-dns-resource-records"></a>为 DNS 资源记录设置访问范围

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此主题通过使用 IPAM 客户端控制台设置 DNS 资源记录的访问作用域。  
  
Administrators组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>设置 DNS 资源记录的访问作用域  
  
1.  在服务器管理器中，单击 " **IPAM**"。 IPAM 客户端控制台随即出现。  
  
2.  在导航窗格中，单击 " **DNS 区域**"。  在下部导航窗格中，展开 "**正向查找**" 并浏览到并选择包含要更改其访问作用域的资源记录的区域。  
  
3.  在 "显示" 窗格中，找到并选择要更改其访问作用域的资源记录。  
  
    ![选择资源记录](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  右键单击所选的 DNS 资源记录，然后单击 "**设置访问作用域**"。  
  
    ![设置访问作用域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  "**设置访问作用域**" 对话框将打开。 如果你的部署需要，请单击以取消选择 "**从父级继承访问作用域**"。 在 "**选择访问作用域**" 中，选择一个项目，然后单击 **"确定"** 。  
  
    ![选择访问作用域](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


