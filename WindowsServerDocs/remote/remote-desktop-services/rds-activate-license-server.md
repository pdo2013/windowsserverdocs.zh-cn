---
title: 激活远程桌面服务许可证服务器
description: 安装和激活远程桌面许可证服务器
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888008"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>激活远程桌面服务许可证服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在访问 RD 会话主机时，远程桌面服务向用户和设备许可证服务器问题客户端访问许可证 (Cal)。 可以使用远程桌面授权管理器来激活许可证服务器。 

## <a name="install-the-rd-licensing-role"></a>安装 RD 许可角色

1. 登录到想要使用与许可证服务器，使用管理员帐户的服务器。
2. 在服务器管理器中，单击**角色摘要**，然后单击**添加角色**。
   单击**下一步**角色向导的第一页。
3. 选择**远程桌面服务**，然后单击**下一步**，然后**下一步**远程桌面服务页面上。
4. 选择**远程桌面授权**，然后单击**下一步**。
5. 配置域-选择**配置此许可证服务器发现作用域**，单击**该域**，然后单击**下一步**。
6. 单击“安装” 。

## <a name="activate-the-license-server"></a>激活许可证服务器

1. 打开远程桌面授权管理器： 单击**开始 > 管理工具 > 远程桌面服务 > 远程桌面授权管理器**。
2. 右键单击许可证服务器，然后依次**激活服务器**。
3. 单击**下一步**的欢迎页上。
4. 对于连接方法中，选择**自动连接 （推荐）**，然后单击**下一步**。
5. 输入你的公司信息 （您的姓名、 公司名称、 地理区域），然后单击**下一步**。
6. 根据需要输入任何其他公司信息 （例如，电子邮件和公司地址），然后依次**下一步**。 
7. 请确保**立即启动许可证安装向导**未选中 （我们将在后面的步骤安装许可证），然后单击**下一步**。

您的许可证服务器是立即准备好开始颁发和管理许可证。 