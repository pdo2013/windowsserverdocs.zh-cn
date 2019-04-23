---
title: 用户帐户注意事项
description: 提供用户帐户、 用户名和密码 MultiPoint Services 的注意事项
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 00fb5e83921ba0b8ad86a6f75bdfd7bf16419b73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850788"
---
# <a name="user-account-considerations"></a>用户帐户注意事项
本主题介绍时，作为管理用户，应考虑的创建和管理用户帐户的问题。 管理 MultiPoint 管理器中的用户选项卡中的用户帐户。 有关详细信息，请参阅[管理用户帐户](Manage-User-Accounts.md)主题。  
  
## <a name="user-account-types"></a>用户帐户类型  
用户帐户是一个信息集合，它告知 MultiPoint 服务用户可以访问的文件和文件夹、可对 MultiPoint 服务系统进行的更改，以及每个用户的偏好设置（如桌面背景）。 每个人通过使用唯一的帐户名和密码访问各自的用户帐户。 MultiPoint 服务支持三种类型的用户帐户：  
  
-   **管理用户帐户**适用于使用 MultiPoint 管理器来使用和管理 MultiPoint 服务系统的个人。 有关详细信息，请参阅[创建管理用户帐户](Create-an-Administrative-User-Account.md)。  
  
-   标准用户帐户适用于定期访问工作站但不管理系统的个人。 通常，应为大多数 MultiPoint 服务系统用户创建标准用户帐户。 有关详细信息，请参阅[创建标准用户帐户](Create-a-Standard-User-Account.md)。  
  
-   MultiPoint 仪表板用户帐户适用于使用 MultiPoint 仪表板管理标准用户会话并且可从任何工作站登录的个人。 有关详细信息，请参阅[创建 MultiPoint 仪表板用户帐户](Create-a-MultiPoint-Dashboard-User-Account.md)。  
  
## <a name="user-name-and-password-considerations"></a>用户名和密码注意事项  
管理用户可执行影响 MultiPoint 服务系统的所有其他用户的任务，如安装软件或更改安全设置。 所以，管理用户应具有仅自己知道的唯一用户名和密码。  
  
用户帐户的一个重要注意事项是，每个用户帐户都被分配了一个 Windows 资源管理器中的唯一“文档”库（该库包含“我的文档”文件夹）。 如果 MultiPoint 服务系统上的标准用户要在其 Windows 资源管理器中的“文档”库中存储私有文档，则该用户也应使用仅自己知道的唯一用户名和密码登录 MultiPoint 服务系统。 有关在 Windows 资源管理器中存储文档的详细信息，请参阅[管理用户文件](Manage-User-Files.md)主题。  
  
> [!TIP]  
> 为了提高系统安全性，所有用户的密码都应为强密码。 强密码是指无法轻易猜到或破解，是至少为八个字符、 不包含全部或一部分用户的帐户名称和包含至少三个四个以下类别的字符： 大写字符、 小写字母字符、 数字和键盘上的符号 (如 ！、 @、 #)。  
  
## <a name="see-also"></a>请参阅  
[创建管理用户帐户](Create-an-Administrative-User-Account.md)  
[创建标准用户帐户](Create-a-Standard-User-Account.md)  
[管理用户文件](Manage-User-Files.md)
[管理用户帐户](Manage-User-Accounts.md)