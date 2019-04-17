---
title: "密码概述"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="passwords-overview"></a>密码概述

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的本主题介绍 Windows 操作系统、和文档和使用有关的凭据管理战略中的密码的讨论指向中使用的密码。

## <a name="BKMK_OVER"></a>功能描述
围绕密码体系结构操作系统和应用程序今天即使在你使用智能卡或生物特征系统，所有帐户都还有密码并仍在某些情况下要使用。 某些帐户值得注意的是用于运行服务的客户甚至不能使用智能卡和生物令牌和因此必须使用密码，以验证身份。 Windows 保护使用加密哈希的密码。

有关 Windows 密码的详细信息，请参阅[密码 Technical 概述](https://technet.microsoft.com/library/hh994558(WS.10).aspx)。

## <a name="BKMK_APP"></a>实际应用
在 Windows 和其他许多操作系统，用户的身份验证身份的最常见的方法是使用密码或密钥密码。 保护你的网络环境要求的所有用户都使用强密码。 这有助于避免猜测弱的密码，无论是通过手动方法或使用工具，以获取受到威胁的用户帐户凭据恶意用户的威胁。 这是用于管理的帐户，尤其如此。 当你将定期更改复杂的密码时，这样可减少密码攻击损害该帐户的可能性。

## <a name="BKMK_NEW"></a>新的和更改功能
在 Windows Server 2012 和 Windows 8，图片密码是新增功能。 图片密码是结合一系列的手势的用户所选映像的组合。 图片密码的功能已禁用 domain\ 加入计算机上。 中列出了有关图片密码的详细信息的链接[参阅](#BKMK_LINKS)下方。

已在 Windows Server 2012 和 Windows 8 密码功能不变。 添加了没有新组策略设置。 但是的改进和增强已经在凭据 \(and password\) 管理，如图片密码、凭据保险箱内和使用 Microsoft 帐户登录到 Windows 8，以前称为 Windows Live id。

## <a name="BKMK_DEP"></a>不建议使用的功能
已在 Windows Server 2012 和 Windows 8 弃用没有密码的功能。

## <a name="BKMK_SOFT"></a>软件要求
企业环境中使用 Active Directory 域服务通常管理密码。 此外可以使用本地帐户策略的安全设置密码的策略中的设置在本地计算机上管理密码。

## <a name="BKMK_LINKS"></a>请参阅
下表列出了密码的功能，用于其他资源技术和凭据管理。

|键入的内容|引用|
|--------|-------|
|**方案文档**|[保护你的数字身份](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**操作**|[Active Directory 的用户和计算机](https://technet.microsoft.com/library/cc754217.aspx)|
|**故障排除**|[了解你的密码的过期时 \-Active Directory PowerShell 博客](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全**| Windows Server 2008 R2 和 Windows 7[威胁并对策指南：帐户策略](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />指南到[更改，并创建强密码](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**工具和设置**|[Windows 和 Windows Server Microsoft 下载中心的组策略设置参考](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社区资源**|[保护你的数字身份](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[登录到 Windows 8 的 Windows Live id](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[使用图片密码登录](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[优化图片密码安全](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


