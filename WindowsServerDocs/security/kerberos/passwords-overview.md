---
title: 密码概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869598"
---
# <a name="passwords-overview"></a>密码概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

面向 IT 专业人员的本主题介绍在 Windows 操作系统，以及指向文档和讨论有关使用凭据管理策略中的密码中使用的密码。

## <a name="BKMK_OVER"></a>功能说明
操作系统和应用程序立即构建就密码和即使你使用智能卡或生物识别系统，所有帐户都仍都具有密码和仍可以在某些情况下使用它们。 某些帐户，值得注意的是帐户用于运行服务，甚至不能使用智能卡和生物识别令牌，并因此必须使用密码进行身份验证。 Windows 保护使用加密哈希密码。

有关 Windows 密码的详细信息，请参阅[密码技术概述](https://technet.microsoft.com/library/hh994558(WS.10).aspx)。

## <a name="BKMK_APP"></a>实际应用程序
在 Windows 和许多其他操作系统中，身份验证用户身份的最常见方法是使用机密的密码。 保护您的网络环境需要的所有用户都使用强密码。 这有助于避免恶意用户猜测弱密码，无论是通过手动方法或通过使用工具，来获取被入侵的用户帐户的凭据的威胁。 这是用于管理帐户尤其如此。 定期更改的复杂密码时，它减少了会损害该帐户的密码攻击的可能性。

## <a name="BKMK_NEW"></a>新的和更改功能
在 Windows Server 2012 和 Windows 8 中，图片密码是新功能。 图片密码是用户所选映像结合一系列手势的组合。 图片密码功能禁用域上\-加入的计算机。 中列出了指向有关图片密码的详细信息[另请参阅](#BKMK_LINKS)下面。

已对 Windows Server 2012 和 Windows 8 中的密码功能没有更改。 已不添加任何新的组策略设置。 但是，改进和增强功能进行了凭据中\(和密码\)管理，例如使用图片密码、 凭据保险箱和使用 Microsoft 帐户登录到 Windows 8，以前称为 Windows Live ID.

## <a name="BKMK_DEP"></a>弃用的功能
Windows Server 2012 和 Windows 8 中已弃用无密码的功能。

## <a name="BKMK_SOFT"></a>软件要求
在企业环境中，通常与 Active Directory 域服务管理密码。 也可以使用本地安全设置，帐户策略密码策略中的设置在本地计算机上管理密码。

## <a name="BKMK_LINKS"></a>另请参阅
此表列出了用于密码功能的其他资源技术和凭据管理。

|内容类型|参考|
|--------|-------|
|**方案文档**|[保护你的数字标识](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**操作**|[Active Directory 用户和计算机](https://technet.microsoft.com/library/cc754217.aspx)|
|**疑难解答**|[找出你的密码过期时\-Active Directory PowerShell 博客](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全性**| Windows Server 2008 R2 和 Windows 7[威胁和对策指南：帐户策略](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />向提供指导[更改并创建强密码](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**工具和设置**|[Windows 和 Microsoft 下载中心上的 Windows Server 的组策略设置参考](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社区资源**|[保护你的数字标识](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[在登录到 Windows 8 和 Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[使用图片密码登录](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[优化图片密码安全性](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


