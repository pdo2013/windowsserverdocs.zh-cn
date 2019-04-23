---
title: NTLM 概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890638"
---
# <a name="ntlm-overview"></a>NTLM 概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

面向 IT 专业人员的本主题描述 NTLM，功能中的任何更改，并为 Windows 身份验证和 Windows Server 2012 的 NTLM 和以前的版本提供技术资源的链接。

## <a name="BKMK_OVER"></a>功能说明
NTLM 身份验证是一系列的身份验证协议中包括 Windows Msv1\_0.dll。 NTLM 身份验证协议包括 LAN Manager 版本 1 和 2 以及 NTLM 版本 1 和 2。 NTLM 身份验证协议进行身份验证的用户和计算机基于质询\/证明到服务器或用户知道的密码与帐户关联的域控制器的响应机制。 在使用 NTLM 协议时，每当需要新的访问令牌时，资源服务器必须执行以下操作之一来验证计算机或用户的身份：

-   如果计算机或用户的帐户是域帐户，请联系域控制器的部门域认证服务来获取该帐户的域。

-   如果该计算机或用户的帐户是本地帐户，请在本地帐户数据库中查找该帐户。

## <a name="BKMK_APP"></a>当前应用程序
NTLM 身份验证仍受支持，必须用于系统配置为工作组成员的 Windows 身份验证。 NTLM 身份验证还用于在非本地登录身份验证\-域控制器。 Kerberos 版本 5 身份验证是 Active Directory 环境中，但非的首选身份验证方法\-Microsoft 或 Microsoft 应用程序仍可以使用 NTLM。

在 IT 环境中减少 NTLM 协议的使用需要了解 NTLM 上的部署应用程序要求，以及配置计算环境以使用其他协议所需的策略和步骤。 添加了新工具和设置以帮助你了解如何使用 NTLM 以便有选择地限制 NTLM 流量。 有关如何在你的环境中分析和限制 NTLM 使用的信息，请参阅 [NTLM 身份验证限制简介](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) 以访问审核和限制 NTLM 使用指南。

## <a name="BKMK_NEW"></a>新的和更改功能
在 Windows Server 2012 的 NTLM 的功能有任何更改。

## <a name="BKMK_DEP"></a>已删除或弃用的功能
没有 Windows Server 2012 的 NTLM 无删除或弃用的功能。

## <a name="BKMK_INSTALL"></a>服务器管理器信息
NTLM 无法从服务器管理器进行配置。 你可以使用安全策略设置或组策略管理计算机系统之间的 NTLM 身份验证使用。 在域中，Kerberos 是默认身份验证协议。

## <a name="BKMK_LINKS"></a>另请参阅
下表列出了 NTLM 和其他 Windows 身份验证技术的相关资源。

|内容类型|参考|
|--------|-------|
|**产品评估**|[NTLM 身份验证限制简介](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[NTLM 身份验证中的更改](https://technet.microsoft.com/library/dd566199.aspx)|
|**规划**|[IT 基础结构威胁建模指南](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[威胁和对策：Windows Server 2003 和 Windows XP 中的安全设置](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[威胁和对策指南：Windows Server 2008 和 Windows Vista 中的安全设置](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[威胁和对策指南：Windows Server 2008 R2 和 Windows 7 中的安全设置](https://technet.microsoft.com/library/hh125921.aspx)|
|**部署**|[身份验证扩展的保护](https://support.microsoft.com/kb/968389)<br /><br />[审核和限制 NTLM 使用指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[询问目录服务团队：NTLM 阻止和你：应用程序分析和审核在 Windows 7 中的方法](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Windows 身份验证博客](https://blogs.technet.com/authentication/)<br /><br />[为 NTLM 传递配置 MaxConcurrentAPI\-通过身份验证](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**开发**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]:NT LAN 管理器\(NTLM\)身份验证协议规范](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]:NT LAN 管理器\(NTLM\)身份验证：网络新闻传输协议\(NNTP\)扩展](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]:基于 HTTP 协议规范的 NTLM](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**疑难解答**|尚未提供|
|**社区资源**|[死信尚未是此程序：NTLM 瓶颈和 RPC 运行时](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



