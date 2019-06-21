---
title: 为 DNS 区域设置访问范围
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 67e6e16d361d6d975c4cf900dc9c6b9e7abd3f20
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282205"
---
# <a name="set-access-scope-for-a-dns-zone"></a>为 DNS 区域设置访问范围

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题使用 IPAM 客户端控制台设置 DNS 区域的访问作用域。  
  
Administrators  组成员或同等身份是执行此过程的最低要求。  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>若要设置为 DNS 区域的访问作用域  
  
1.  在服务器管理器中，单击**IPAM**。 IPAM 客户端控制台将显示。  
  
2.  在导航窗格中，单击**DNS 区域**。 在显示窗格中，右键单击你想要更改访问作用域。，，然后单击 DNS 区域**设置访问作用域**。  
  
    ![设置访问作用域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  **设置访问作用域**对话框随即打开。 如果需要为你的部署，单击要取消选定**从父级继承访问作用域**。 在中**选择访问作用域**，选择一个项，然后单击**确定**。  
  
    ![选择访问作用域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  在 IPAM 客户端控制台的显示窗格中，验证，更改该区域的访问作用域。  
  
    ![验证更改该区域的访问作用域](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>请参阅  
[基于角色的访问控制](Role-based-Access-Control.md)  
[管理 IPAM](Manage-IPAM.md)  
  


