---
title: 激活远程桌面服务许可证服务器
description: 安装和激活 RD 许可证服务器
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 68a688fff8c935ae051223aeeea69e1d1ef5b9db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404062"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>激活远程桌面服务许可证服务器

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

访问 RD 会话主机时，远程桌面服务许可证服务器会向用户和设备颁发客户端访问许可证 (CAL)。 可以使用远程桌面授权管理器激活许可证服务器。 

## <a name="install-the-rd-licensing-role"></a>安装 RD 授权角色

1. 使用管理员帐户登录要用作许可证服务器的服务器。
2. 在“服务器管理器”中，单击“角色摘要”  ，然后单击“添加角色”  。
   在角色向导的第一页上单击“下一步”  。
3. 选择“远程桌面服务”  ，然后单击“下一步”  ，然后在“远程桌面服务”页面上单击“下一步”  。
4. 选择“远程桌面授权”  ，然后单击“下一步”  。
5. 配置域 - 选择“为此许可证服务器配置发现范围”  ，单击“此域”  ，然后单击“下一步”  。
6. 单击“安装”  。

## <a name="activate-the-license-server"></a>激活许可证服务器

1. 打开远程桌面授权管理器：单击“开始”>“管理工具”>“远程桌面服务”>“远程桌面授权管理器”  。
2. 右键单击许可证服务器，然后单击“激活服务器”  。
3. 在欢迎页面上单击“下一步”  。
4. 对于连接方法，选择“自动连接(推荐)”  ，然后单击“下一步”  。
5. 输入你的公司信息（你的姓名、公司名称、地理区域），然后单击“下一步”  。
6. 可选择输入任何其他公司信息（例如电子邮件和公司地址），然后单击“下一步”  。 
7. 确保未选择“立即启动许可证安装向导”  （我们会在后面的步骤安装许可证），然后单击“下一步”  。

许可证服务器现在已准备好开始颁发和管理许可证。 