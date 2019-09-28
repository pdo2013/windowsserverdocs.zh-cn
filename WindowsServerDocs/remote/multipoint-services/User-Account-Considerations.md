---
title: 用户帐户注意事项
description: 提供 MultiPoint Services 的用户帐户、用户名和密码注意事项
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: c81d14d46e96d39676e1fb6fa31892e0d5e1b683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389261"
---
# <a name="user-account-considerations"></a>用户帐户注意事项
本主题介绍你作为管理用户在创建和管理用户帐户时应考虑的问题。 可在 MultiPoint 管理器的 "用户" 选项卡中管理用户帐户。 有关详细信息，请参阅[管理用户帐户](Manage-User-Accounts.md)主题。  
  
## <a name="user-account-types"></a>用户帐户类型  
用户帐户是信息的集合，通知 MultiPoint 服务用户可以访问的文件和文件夹、可对 MultiPoint 服务系统进行哪些更改以及每个用户的首选项（如桌面背景）。 每个人通过使用唯一的帐户名和密码访问各自的用户帐户。 MultiPoint 服务支持三种类型的用户帐户：  
  
-   **管理用户帐户**适用于将使用 Multipoint 管理器来使用和管理 multipoint 服务系统的个人。 有关详细信息，请参阅[创建管理用户帐户](Create-an-Administrative-User-Account.md)。  
  
-   标准用户帐户适用于定期访问工作站但不管理系统的个人。 通常，应为大多数 MultiPoint 服务系统用户创建标准用户帐户。 有关详细信息，请参阅[创建标准用户帐户](Create-a-Standard-User-Account.md)。  
  
-   MultiPoint 仪表板用户帐户适用于使用 MultiPoint 仪表板管理标准用户会话并且可从任何工作站登录的个人。 有关详细信息，请参阅[创建 MultiPoint 仪表板用户帐户](Create-a-MultiPoint-Dashboard-User-Account.md)。  
  
## <a name="user-name-and-password-considerations"></a>用户名和密码注意事项  
管理用户可执行影响 MultiPoint 服务系统的所有其他用户的任务，如安装软件或更改安全设置。 所以，管理用户应具有仅自己知道的唯一用户名和密码。  
  
用户帐户的一个重要注意事项是，每个用户帐户都被分配了一个 Windows 资源管理器中的唯一“文档”库（该库包含“我的文档”文件夹）。 如果 MultiPoint 服务系统上的标准用户要在其 Windows 资源管理器中的“文档”库中存储私有文档，则该用户也应使用仅自己知道的唯一用户名和密码登录 MultiPoint 服务系统。 有关在 Windows 资源管理器中存储文档的详细信息，请参阅[管理用户文件](Manage-User-Files.md)主题。  
  
> [!TIP]  
> 为了获得更强的系统安全性，所有用户的密码都应该是强密码。 强密码是一种无法轻易猜到或破解的密码，长度至少为八个字符，不包含全部或部分用户帐户名，并且至少包含以下四类字符中的三种：大写字符，小写键盘上找到的字符、数字和符号（如！、@、#）。  
  
## <a name="see-also"></a>请参阅  
[创建管理用户帐户](Create-an-Administrative-User-Account.md)  
[创建标准用户帐户](Create-a-Standard-User-Account.md)  
[管理用户文件](Manage-User-Files.md)
[管理用户帐户](Manage-User-Accounts.md)