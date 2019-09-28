---
title: 创建管理用户帐户
description: 在 MultiPoint Services 中创建具有管理权限的帐户
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6737f7b96396a13aa18485095e0687425cf8b93e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389698"
---
# <a name="create-an-administrative-user-account"></a>创建管理用户帐户
为将管理 MultiPoint 服务系统的个人创建管理用户帐户。 若要查看具有管理访问权限的人员，请在 MultiPoint 管理器中单击 "**用户**" 选项卡。管理用户帐户在“帐户类型”列中显示为“管理员”。 *管理用户*可以访问更改桌面和系统设置的所有 MultiPoint 管理器任务，例如：  
  
-   创建帐户  
  
-   添加和删除程序  
  
-   管理桌面和硬件  
  
-   结束其他用户会话  
  
管理用户可执行影响 MultiPoint 服务系统的所有其他用户的任务，如安装软件或更改安全设置。 所以，管理用户应具有仅自己知道的唯一用户名和密码。  
  
有关作为管理用户，在创建和管理用户帐户时应考虑的问题的详细信息，请参阅[用户帐户注意事项](User-Account-Considerations.md)主题。  
  
> [!NOTE]  
> 当在 MultiPoint 服务系统上执行与 MultiPoint 服务系统管理无关的任务时，你可能想要为自己创建一个标准用户帐户以供使用。 这样，你可以在需要执行系统管理任务时才登录管理用户帐户。  
  
#### <a name="to-create-an-administrative-user-account"></a>创建管理用户帐户  
  
1.  在 MultiPoint 管理器中，单击 "**用户**" 选项卡。  
  
2.  在“用户任务”下，单击“添加用户帐户”。 “添加用户帐户”向导随即打开。  
  
3.  在“用户帐户”字段中，键入用户登录名。 通常，登录用户名是将姓和名拼写在一起的（中间无空格）或将首字母和名拼写在一起（中间无空格）。  
  
4.  在“全名”字段中，以你偏爱的任何格式键入用户名，如给定名、全名或昵称。  
  
5.  在“密码”字段中，键入用户密码。 该密码应当只有你和用户知道，你应将此信息存储在一个安全的位置。 密码只能由管理用户更改。  
  
6.  在“确认密码”字段中，重新键入密码，然后单击“下一步”。  
  
7.  在访问级别页上，选择“管理用户”，然后单击“下一步”。  
  
8.  MultiPoint 服务将检查所有信息并在帐户设置完成后显示一则消息。 看到文本“已成功创建新用户帐户”时，单击“完成”。  
