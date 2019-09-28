---
title: NTLM 概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b8dec2877646fd2bfe00da9d5c9047e8edfd6f1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386256"
---
# <a name="ntlm-overview"></a>NTLM 概述

>适用于：Windows Server（半年频道）、Windows Server 2016

适用于 IT 专业人员的本主题介绍 NTLM、功能的任何更改，并提供指向 Windows 身份验证的技术资源的链接以及 Windows Server 2012 和早期版本的 NTLM。

## <a name="BKMK_OVER"></a>功能说明
NTLM 身份验证是 Windows Msv1\_0.dll 中包含的一系列身份验证协议。 NTLM 身份验证协议包括 LAN Manager 版本 1 和 2 以及 NTLM 版本 1 和 2。 NTLM 身份验证协议基于质询 @ no__t-0response 机制对用户和计算机进行身份验证，该机制向服务器或域控制器证明用户知道与帐户关联的密码。 在使用 NTLM 协议时，每当需要新的访问令牌时，资源服务器必须执行以下操作之一来验证计算机或用户的身份：

-   如果计算机或用户的帐户是域帐户，请联系域控制器的部门域认证服务来获取该帐户的域。

-   如果该计算机或用户的帐户是本地帐户，请在本地帐户数据库中查找该帐户。

## <a name="BKMK_APP"></a>当前应用程序
NTLM 身份验证仍受支持，必须用于系统配置为工作组成员的 Windows 身份验证。 NTLM 身份验证还用于非 @ no__t-0domain 控制器上的本地登录身份验证。 Kerberos 版本5身份验证是 Active Directory 环境的首选身份验证方法，但非 @ no__t-0Microsoft 或 Microsoft 应用程序仍可以使用 NTLM。

在 IT 环境中减少 NTLM 协议的使用需要了解 NTLM 上的部署应用程序要求，以及配置计算环境以使用其他协议所需的策略和步骤。 添加了新工具和设置以帮助你了解如何使用 NTLM 以便有选择地限制 NTLM 流量。 有关如何在你的环境中分析和限制 NTLM 使用的信息，请参阅 [NTLM 身份验证限制简介](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) 以访问审核和限制 NTLM 使用指南。

## <a name="BKMK_NEW"></a>新增功能和更改的功能
对于适用于 Windows Server 2012 的 NTLM，没有任何功能更改。

## <a name="BKMK_DEP"></a>删除或弃用的功能
适用于 Windows Server 2012 的 NTLM 没有已删除或弃用的功能。

## <a name="BKMK_INSTALL"></a>服务器管理器信息
NTLM 无法从服务器管理器进行配置。 你可以使用安全策略设置或组策略管理计算机系统之间的 NTLM 身份验证使用。 在域中，Kerberos 是默认身份验证协议。

## <a name="BKMK_LINKS"></a>另请参阅
下表列出了 NTLM 和其他 Windows 身份验证技术的相关资源。

|内容类型|参考资料|
|--------|-------|
|**产品评估**|[NTLM 身份验证的限制简介](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[NTLM 身份验证的更改](https://technet.microsoft.com/library/dd566199.aspx)|
|**规划**|[IT 基础结构威胁建模指南](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />@no__t 0Threats 和对策：Windows Server 2003 和 Windows XP @ no__t 中的安全设置<br /><br />@no__t 0Threats 和对策指南：Windows Server 2008 和 Windows Vista @ no__t 中的安全设置<br /><br />@no__t 0Threats 和对策指南：Windows Server 2008 R2 和 Windows 7 @ no__t 中的安全设置|
|**部署**|[针对验证的扩展保护](https://support.microsoft.com/kb/968389)<br /><br />[审核和限制 NTLM 使用指南](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />@no__t 目录服务团队0Ask：NTLM 阻止和你：Windows 7 @ no__t 中的应用程序分析和审核方法<br /><br />[Windows 身份验证博客](https://blogs.technet.com/authentication/)<br /><br />[配置 MaxConcurrentAPI 以进行 NTLM pass @ no__t-1through authentication](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**研发**|[Microsoft NTLM \(Windows @ no__t-2](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[ @ NO__T-1MS @ NO__T-2NLMP @ NO__T-3：NT LAN Manager \(NTLM @ no__t-1 身份验证协议规范 @ no__t-2<br /><br />[ @ NO__T-1MS @ NO__T-2NNTP @ NO__T-3：NT LAN Manager \(NTLM @ no__t Authentication：网络新闻传输协议 \(NNTP @ no__t-1 Extension @ no__t-2<br /><br />[ @ NO__T-1MS @ NO__T-2NTHT @ NO__T-3：NTLM Over HTTP 协议规范 @ no__t-0|
|**疑难解答**|尚未提供|
|**社区资源**|[Is 此马停滞：NTLM 瓶颈和 RPC 运行时 @ no__t-0|



