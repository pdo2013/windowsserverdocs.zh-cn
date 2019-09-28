---
title: 基于角色的访问控制
description: 本主题是 Windows Server 2016 中的 IP 地址管理（IPAM）管理指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cf6b8418482b606c86bac77b2790e3365e80bf5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355156"
---
# <a name="role-based-access-control"></a>基于角色的访问控制

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供有关在 IPAM 中使用基于角色的访问控制的信息。  
  
> [!NOTE]  
> 除了本主题之外，本节还提供了以下 IPAM 访问控制文档。  
>   
> -   [使用服务器管理器管理基于角色的访问控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [使用 Windows PowerShell 管理基于角色的访问控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
基于角色的访问控制允许你指定不同级别的访问权限，包括 DNS 服务器、DNS 区域和 DNS 资源记录级别。  
通过使用基于角色的访问控制，你可以指定对操作进行精细控制的人员，以创建、编辑和删除不同类型的 DNS 资源记录。  
  
你可以配置访问控制，以便将用户限制为以下权限。  
  
-   用户只能编辑特定的 DNS 资源记录  
  
-   用户可以编辑特定类型的 DNS 资源记录，如 PTR 或 MX  
  
-   用户可以编辑特定区域的 DNS 资源记录  
  
## <a name="see-also"></a>请参阅  
[使用服务器管理器管理基于角色的访问控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[使用 Windows PowerShell 管理基于角色的访问控制](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[管理 IPAM](Manage-IPAM.md)  
  


