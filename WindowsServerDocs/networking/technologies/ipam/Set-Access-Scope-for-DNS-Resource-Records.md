---
title: 对 DNS 资源记录的访问权限范围的设置
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>对 DNS 资源记录的访问权限范围的设置

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题设置使用 IPAM 客户端控制台 DNS 资源记录的访问权限范围。  
  
在会员**管理员**，或等效的最低要求执行此过程。  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>若要设置的资源 DNS 记录的访问权限范围  
  
1.  在服务器管理器中，单击**IPAM**。 显示 IPAM 客户端控制台。  
  
2.  在导航窗格中，单击**DNS 区域**。  在较低的导航窗格中，展开**向前查找**浏览到和选择包含你想要更改其访问范围的资源记录的区域。  
  
3.  显示窗格中，找到，并选择你想要更改其访问范围的资源记录。  
  
    ![选择资源记录](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  右键单击所选的 DNS 资源记录，，然后单击**设置访问范围**。  
  
    ![将访问范围设置](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  **设置访问范围**对话框中打开。 如果所需的部署，单击要取消选择**继承访问范围内父从**。 在**选择访问范围**，选择的项目，然后单击**确定**。  
  
    ![选择访问范围](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


