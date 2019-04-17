---
title: 将访问范围 DNS 区域的设置
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
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a4e84f249e57df6bfd04203c8b825ff5d4d643b2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-a-dns-zone"></a>将访问范围 DNS 区域的设置

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题通过 IPAM 客户端主机设置为 DNS 区域访问范围。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>若要将访问范围 DNS 区域的设置  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，单击**DNS 区域**。 在显示窗格中，右键单击你想要更改访问范围。，然后单击 DNS 区域**设置访问范围**。  
  
    ![将访问范围设置](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  **设置访问范围**对话框中打开。 如果所需的部署，单击要取消选择**继承访问范围内父从**。 在**选择访问范围**，选择的项目，然后单击**确定**。  
  
    ![选择访问范围](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  在 IPAM 客户端控制台显示器窗格中，验证访问范围区域发生了更改。  
  
    ![验证该区域的访问权限范围发生了更改](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


