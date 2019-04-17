---
title: 设置电子邮件发现以订阅源您 RDS
description: 了解如何集成 RDS 部署 Azure AD 域服务。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691666"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>设置电子邮件发现以订阅源您 RDS

您曾经排除最终用户连接到源，原因订阅源中的单个缺少字符是其已发布 RDS 故障 URL 或因为它们丢失的 url 的电子邮件？ 几乎所有远程桌面客户端应用程序支持通过输入您的电子邮件地址，使其成为比以往任何时候都以获取您连接到其 Remoteapp 和桌面的用户更容易查找您的订阅。

>[!IMPORTANT]
>Microsoft 存储区中的 Microsoft 远程桌面应用程序不支持这次电子邮件地址订阅。

设置电子邮件发现之前，请执行以下操作：

- 请确保您有权向您的电子邮件与关联的域添加 TXT 记录 (例如，如果用户具有@contoso.com电子邮件地址，您需要的权限 contoso.com 域)
- 创建源 URL RD Web (https://\ < rdweb dns name\ >.domain/RDWeb/Feed/webfeed.aspx，如https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

现在，使用下列步骤设置电子邮件发现：

1. 在浏览器中，连接到您的域注册其中域名注册机构的网站。
2. 导航到您已注册的域，其中可以查看、 添加和编辑 DNS 记录的相应页面。
3. 输入新的 DNS 记录具有以下属性：
   - **主机：** _msradc
   - **文本：** \ < RD Web 源 URL\ >
   - **TTL:** 300

   DNS 记录字段的名称随域名注册机构，但此过程会导致 TXT 记录名为 _msradc。 \ < domain_name\ > （如 _msradc.contoso.com)，其完全 RD Web 馈送的值。

就是这样！ 现在，启动远程桌面应用程序在设备上的，并订阅自己 ！