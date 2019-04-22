---
title: Kerberos Constrained Delegation Overview
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821928"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本概述主题面向 IT 专业人员介绍了 Windows Server 2012 R2 和 Windows Server 2012 中 Kerberos 约束委派的新功能。

**功能说明**

在 Windows Server 2003 中引入的 Kerberos 约束委派，为服务所使用的委派提供了一种更安全的形式。 配置了 Kerberos 约束委派后，它限制了指定服务器可代表用户执行的服务。 这需要域管理员权限来为服务配置一个域账户，并把该账户限制到单个域。 在如今的企业，前端服务不用于将限制为与仅在其域中的服务的集成。

在域管理员配置服务的早期操作系统中，服务管理员没有有效途径了解其拥有哪些委派到资源服务的前端服务。 可委派到资源服务的任何前端服务都代表一个潜在的攻击点。 如果托管前端服务的服务器受到危害，并且其被配置为委派到资源服务，则资源服务也会受到危害。

在 Windows Server 2012 R2 和 Windows Server 2012 中，配置服务约束的委派的能力已从转移域管理员为服务管理员。 以这种方式，后端服务管理员可以允许或拒绝前端服务。

有关 Windows Server 2003 中引入的约束委派的详细信息，请参阅 [Kerberos 协议转换和约束委派](https://technet.microsoft.com/library/cc739587(v=ws.10))。

Kerberos 协议的 Windows Server 2012 R2 和 Windows Server 2012 实现包括约束委派的专门扩展。  Service for User to Proxy (S4U2Proxy) 允许服务使用其用户 Kerberos 服务票证从密钥发行中心 (KDC) 获得服务票证，以用于后端服务。 这些扩展允许将约束的委派，必须在后端服务的帐户，可以在另一个域上配置。 有关这些扩展的详细信息，请参阅[ \[MS-SFU\]:Kerberos 协议扩展：用户服务和约束的委派协议规范](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)MSDN 库中。

**实际应用程序**

约束的委派可让服务管理员的功能来指定和加强应用程序信任边界通过限制应用程序服务可以对代表用户的范围。 服务管理员可以配置哪些前端服务账户能委派到其后端服务。

通过支持 Windows Server 2012 R2 和 Windows Server 2012 中，如 Microsoft Internet Security and Acceleration (ISA) Server、 Microsoft Forefront Threat Management Gateway、 Microsoft Exchange 前端服务中的跨域的约束委派Outlook Web Access (OWA) 和 Microsoft SharePoint Server 可以配置为使用约束的委派对其他域中的服务器进行身份验证。 这将通过使用现有的 Kerberos 基础结构来支持跨域的服务解决方案。 域管理员或服务管理员可以管理 Kerberos 约束委派。

## <a name="resource-based-constrained-delegation-across-domains"></a>跨域的基于资源的约束委派

Kerberos 约束委派可以在前端服务与资源服务不在同一域中时用于提供约束委派。 服务管理员能通过指定可以在资源服务账户对象上代表用户的前端服务域账户来配置新委派。

**这一更改增添了什么价值？**

通过支持跨域约束委派，可以将服务配置为使用约束委派（而不是非约束委派）来对其他域中的服务器进行身份验证。 这将通过使用现有的 Kerberos 基础结构来支持跨域的服务解决方案身份验证，而不需要信任委派到任何服务的前端服务。

这还将移动服务器是否应信任委派从域管理员向资源所有者的委托标识源的决策。

**工作原理的不同之处是什么？**

基础协议中的更改允许跨域约束委派。 Kerberos 协议的 Windows Server 2012 R2 和 Windows Server 2012 实现包括 service for User to Proxy (S4U2Proxy) 协议的扩展。 这组 Kerberos 协议扩展允许服务使用其用户 Kerberos 服务票证从密钥发行中心 (KDC) 获得服务票证，以用于后端服务。

有关这些扩展的实现信息，请参阅[ \[MS-SFU\]:Kerberos 协议扩展：用户服务和约束的委派协议规范](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx)MSDN 中。

有关使用与 Service for User (S4U) 扩展相比提早的票证授予票证 (TGT) 的 Kerberos 委派的基础消息队列的详细信息，请参阅 [1.3.3 协议概述](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx)部分（“[MS-SFU]：Kerberos 协议扩展：用户服务和约束委派协议规范”）。

**安全隐患的基于资源的约束委派**

基于资源的约束委派会使控件的手中拥有待访问资源的管理员的委派。 它依赖于资源服务而不是受信任委派的服务的属性。 因此，基于资源的约束的委派不能使用以前控制协议转换的受信任-到-进行身份验证-为-委托位。 执行基于资源的约束委派，就好像已设置了位时，KDC 将始终允许协议转换。

KDC 不会限制协议转换，因为引入了两个新的已知 Sid 来为资源管理员提供此控制。  这些 Sid 标识是否已发生的协议转换，以及可用的标准访问控制列表来授予或限制访问，根据需要。

|SID|描述|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|一个意味着客户端的标识的 SID 宣称的身份验证机构基于客户端凭据的所有权证明。|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|一个表示客户端的标识的 SID 添加由服务中。|

后端服务可以使用标准 ACL 表达式以确定如何对用户进行身份验证。

**如何配置基于资源的约束委派？**

若要配置资源服务以允许代表用户的前端服务访问，则需要使用 Windows PowerShell cmdlet。

-   若要检索主体列表，请使用**Get-adcomputer**， **Get-adserviceaccount**，并**Get-aduser** cmdlet 以及**属性PrincipalsAllowedToDelegateToAccount**参数。

-   若要配置资源服务，请使用**New-adcomputer**， **New-adserviceaccount**， **New-aduser**， **Set-adcomputer**， **Set-adserviceaccount**，并**Set-aduser** cmdlet 以及**PrincipalsAllowedToDelegateToAccount**参数。

## <a name="BKMK_SOFT"></a>软件要求
基于资源的约束的委派只能配置在运行 Windows Server 2012 R2 和 Windows Server 2012 的域控制器上，但可以在混合模式林中应用。

必须将以下修补程序应用到运行 Windows Server 2012 中用户帐户域的引用路径上运行的操作系统早于 Windows Server 的前端和后端域之间的所有域控制器：基于资源的约束委派 KDC_ERR_POLICY 在具有基于 Windows Server 2008 R2 的域控制器的环境中失败。
