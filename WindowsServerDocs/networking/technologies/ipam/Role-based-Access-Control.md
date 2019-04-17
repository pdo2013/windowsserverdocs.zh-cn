---
title: 基于角色访问控制
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
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eec6d2a89b24d4847cb993bab31d86881f2cae0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="role-based-access-control"></a>基于角色访问控制

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供在 IPAM 使用角色基于访问控制信息。  
  
> [!NOTE]  
> 本主题中，除了以下 IPAM 访问控制文档将显示在此部分中。  
>   
> -   [管理基于角色访问控制使用服务器管理器](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [管理基于角色访问与 Windows PowerShell 控件](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
基于角色访问控制，可指定各个级别，包括 DNS 服务器、DNS 区域和 DNS 资源录制级别的访问权限。  
使用基于角色访问控制，可指定谁有具体操作，以创建、编辑和删除不同类型的资源 DNS 记录的控制。  
  
你可以访问控制配置用户仅限于使用下面的权限。  
  
-   只有特定 DNS 资源记录，用户可以编辑  
  
-   用户可以编辑特定类型，例如 PTR 或 MX DNS 资源的记录  
  
-   用户可以编辑 DNS 资源特定区域记录  
  
## <a name="see-also"></a>请参阅  
[管理基于角色访问控制使用服务器管理器](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[管理基于角色访问与 Windows PowerShell 控件](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[管理 IPAM](Manage-IPAM.md)  
  


