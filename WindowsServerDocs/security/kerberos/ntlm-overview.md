---
title: "NTLM 概述"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>NTLM 概述

>适用于：Windows Server（半年通道），Windows Server 2016

此主题以供 IT 专业人员描述 NTLM，功能中的任何更改，并提供对 Windows 身份验证和 NTLM technical 资源的链接，Windows Server 2012 和以前的版本。

## <a name="BKMK_OVER"></a>功能描述
NTLM 身份验证是一系列的身份验证协议包含在 Windows Msv1\_0.dll 程度。 身份验证 NTLM 协议包含 LAN Manager 版本 1 和 2，以及 NTLM 1 和 2 的版本。 身份验证 NTLM 协议身份验证的用户和基于向用户知道密码的帐户相关联的域控制器服务器或证明 challenge\/响应机制计算机。 当使用 NTLM 协议时，资源服务器必须花验证计算机或用户的身份时需要一个新访问令牌下列操作之一：

-   如果该帐户是域帐户，请与计算机或用户帐户域，域控制器上的域身份验证服务联系。

-   查找计算机或用户帐户在本地帐户数据库中，该帐户是否本地帐户。

## <a name="BKMK_APP"></a>当前应用程序
NTLM 身份验证仍受支持，并且必须用于 Windows 身份验证的系统配置为在工作组中的成员。 NTLM 身份验证也适用于本地登录 non\ 域控制器上的身份验证。 Kerberos 5 版本身份验证的 Active Directory 环境的首选身份验证方法，但 non\ Microsoft 或 Microsoft 应用程序可能仍然会使用 NTLM。

降低 NTLM 协议在 IT 环境中使用需要部署的应用程序要求 NTLM 和策略和配置的计算环境需要步骤以使用其他协议的这两个的知识。 已添加新的工具和设置，以帮助你发现 NTLM 以便选择性限制 NTLM 流量的使用方式。 有关如何分析和限制 NTLM 使用您的环境的信息，请参阅[引入这限制的种身份验证](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx)访问审核和限制 NTLM 使用量指南。

## <a name="BKMK_NEW"></a>新的和更改功能
Windows Server 2012 的 NTLM 的功能有任何更改。

## <a name="BKMK_DEP"></a>已删除或不建议使用功能
没有为 Windows Server 2012 的 NTLM 已删除或过时功能。

## <a name="BKMK_INSTALL"></a>服务器管理器信息
无法配置 NTLM 从服务器管理器。 你可以使用安全策略设置或组策略管理 NTLM 身份验证使用量之间的计算机系统。 在域中，Kerberos 是默认身份验证的协议。

## <a name="BKMK_LINKS"></a>请参阅
下表列出 NTLM 和其他 Windows 身份验证技术相关的资源。

|键入的内容|引用|
|--------|-------|
|**产品评估**|[引入种身份验证的限制](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[更改在 NTLM 身份验证](https://technet.microsoft.com/library/dd566199.aspx)|
|**计划**|[IT 基础结构威胁建模指南](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[威胁和对策：Windows Server 2003 中的安全设置与 Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[威胁并对策指南：安全设置，在 Windows Server 2008 和 Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[威胁并对策指南：Windows Server 2008 R2 和 Windows 7 中的安全设置](https://technet.microsoft.com/library/hh125921.aspx)|
|**部署**|[扩展的保护进行身份验证](https://support.microsoft.com/kb/968389)<br /><br />[审核和限制 NTLM 使用量指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[询问目录服务团队：NTLM 阻止和您：分析应用程序和审核在 Windows 7 的方法](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Windows 身份验证博客](https://blogs.technet.com/authentication/)<br /><br />[对于 NTLM pass\ 通过身份验证配置 MaxConcurrentAPI](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**开发**|[MicrosoftNTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) 身份验证协议规范](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) 身份验证：网络资讯传输协议 \(NNTP\) 扩展](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: HTTP 协议规范通过 NTLM](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**故障排除**|仍不可|
|**社区资源**|[此马死尚：NTLM 瓶颈和 RPC 运行时](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



