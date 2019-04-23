---
title: 设置电子邮件发现以订阅为您的 RDS 源
description: 了解如何将 Azure AD 域服务集成到 RDS 部署。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878318"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>设置电子邮件发现以订阅为您的 RDS 源

是否曾经时遇到问题最终用户连接到源，其已发布 RDS，或者是因为源中的单个缺失字符 URL 或因失去带 URL 的电子邮件？ 几乎所有远程桌面客户端应用程序支持通过输入你的电子邮件地址，使它比以往要获取连接到其 Remoteapp 和桌面用户更轻松查找你的订阅。

>[!IMPORTANT]
>Microsoft Store 中的 Microsoft 远程桌面应用程序不支持这一次的电子邮件地址订阅。

设置电子邮件发现之前，请执行以下操作：

- 请确保您有权向与你的电子邮件域添加 TXT 记录 (例如，如果你的用户有@contoso.com电子邮件地址，您将需要在 contoso.com 域的权限)
- 创建 RD Web 源 URL (https://\<rdweb dns 名称\>.domain/RDWeb/Feed/webfeed.aspx，如 https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

现在，使用以下步骤来设置电子邮件发现：

1. 在浏览器中，连接到其中注册你的域的域名注册机构的网站。
2. 导航到已注册域，可以查看、 添加和编辑 DNS 记录的相应页。
3. 输入新的 DNS 记录具有以下属性：
   - **主机：** _msradc
   - **文本：**\<RD Web 源的 URL\>
   - **TTL:** 300

   DNS 记录字段的名称因域注册机构，但此过程会导致名为 _msradc 的 TXT 记录。\<domain_name\> （如 _msradc.contoso.com) 的具有完整的 RD Web 源的值。

完成了！ 现在，启动你的设备上的远程桌面应用程序，并为自己订阅 ！