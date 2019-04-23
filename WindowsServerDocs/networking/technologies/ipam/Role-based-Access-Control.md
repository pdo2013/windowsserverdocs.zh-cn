---
title: 基于角色的访问控制
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
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 86a57e5a74073ecf749c4ec8209999e8ace31508
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838898"
---
# <a name="role-based-access-control"></a>基于角色的访问控制

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供有关在 IPAM 中使用基于角色的访问控制的信息。  
  
> [!NOTE]  
> 除了本主题，以下 IPAM 访问控制提供了文档在本部分中。  
>   
> -   [管理基于角色的访问控制使用服务器管理器](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [管理基于角色的访问控制使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
基于角色的访问控制允许您指定不同级别，包括 DNS 服务器、 DNS 区域和 DNS 资源记录级别的访问权限。  
通过使用基于角色的访问控制，可以指定有精细的控制操作来创建、 编辑和删除不同类型的 DNS 资源记录的人员。  
  
您可以配置访问控制，使用户仅限于以下权限。  
  
-   用户可以编辑仅特定的 DNS 资源记录  
  
-   用户可编辑特定类型，例如 PTR 或 MX DNS 资源的记录  
  
-   用户可编辑特定区域的 DNS 资源的记录  
  
## <a name="see-also"></a>请参阅  
[管理基于角色的访问控制使用服务器管理器](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[管理基于角色的访问控制使用 Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[管理 IPAM](Manage-IPAM.md)  
  


