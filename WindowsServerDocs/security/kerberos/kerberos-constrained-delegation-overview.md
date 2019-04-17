---
title: "Kerberos 约束的委派概述"
description: "Windows Server 安全"
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos 约束的委派概述

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此概述主题介绍了在 Windows Server 2012 R2 和 Windows Server 2012 Kerberos 受限委派的新功能。

## <a name="feature-description"></a>功能描述
在 Windows Server 2003 以提供更安全地形式的可能被运用到服务的委派引入了委派 Kerberos 约束。 当配置时，约束的委派将限制为其指定的服务器可以采取措施用户替的服务。 这需要域管理员权限配置服务某个域帐户，并且将限制单个域帐户。 在当今的企业前端服务不旨在将仅限于集成与仅与其域中的服务。

在更早的操作系统域管理员身份配置该服务的位置的服务管理员具有无法有用知道哪些前端服务委派给他们拥有的资源服务。 并且可能委派给资源服务任何前端服务代表潜在攻击点。 如果已受到威胁的服务器上托管前端服务，并且配置为委派给资源服务，资源服务还可能已泄露。

在 Windows Server 2012 R2 和 Windows Server 2012，能够配置为服务约束的委派已转移从域管理员到的服务管理员。 这种方式的后端服务管理员可以允许或拒绝前端服务。

有关详细信息约束委派作为引入了 Windows Server 2003，显示[Kerberos 协议过渡和受限委派](https://technet.microsoft.com/library/cc739587(v=ws.10))。

Windows Server 2012 R2 和 Windows Server 2012 Kerberos 协议实施包括专门针对约束委派扩展。  用户到代理服务器 (S4U2Proxy) 的服务允许服务使用用户为其 Kerberos 服务票证后端服务获取 service 票证从密钥 Distribution 中心 (KDC)。 这些扩展允许约束的委派要配置上的后端服务的帐户，可以在另一个域。 有关这些扩展的详细信息，请参阅[\[MS-SFU\]: Kerberos 协议扩展：用户和受限委派协议规范服务](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)MSDN 库中。

## <a name="practical-applications"></a>实际应用
约束的委派提供服务管理员指定和履行应用程序信任边界通过限制应用程序服务能根据用户代表范围的功能。 服务管理员可以配置的帐户前端服务可以委派给他们的后端服务。

通过跨域，Windows Server 2012 R2 和 Windows Server 2012 的支持约束委派，可配置前端服务，如 Microsoft Internet 安全和加速 (ISA) 服务器、Microsoft Forefront 威胁管理网关、Microsoft Exchange Outlook Web Access (OWA) 和 Microsoft SharePoint 服务器使用约束的委派给其他域中服务器身份验证。 这为提供支持以至服务解决方案使用现有 Kerberos 基础结构。 Kerberos 受限委派可以通过域管理员或服务管理员进行管理。

## <a name="new-and-changed-functionality"></a>新的和更改功能
**基于资源跨域约束委派**

可以使用 Kerberos 受限委派提供约束的委派前端服务和资源服务并不在相同的域。 服务管理员是可以配置新委派通过指定前端的服务，其中可以模拟用户帐户对象的资源服务上的域帐户。

**此更改添加哪些值？**

通过跨域支持约束的委派，可以服务配置为使用约束的委派给其他域，而无需使用不受约束的委派中服务器身份验证。 无需使用现有 Kerberos 基础结构信任前端服务对任何服务委派这提供跨域服务解决方案的身份验证的支持。

**内容的工作方式不同？**

基础协议中的某个更改以至允许约束的委派。 Windows Server 2012 R2 和 Windows Server 2012 Kerberos 协议实施包括扩展到代理 (S4U2Proxy) 协议用户的服务。 这是一套允许使用用户为其 Kerberos 服务票证后端服务获取 service 票证从密钥 Distribution 中心 (KDC) 的服务 Kerberos 协议的扩展。

有关这些扩展实现信息，请参阅[\[MS-SFU\]: Kerberos 协议扩展：用户和受限委派协议规范服务](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx)MSDN 中。

有关基本消息序列 Kerberos 委派与转发的授权票证 (TGT) 相比服务 (S4U) 用户扩展的详细信息，请参阅部分[1.3.3 协议概述](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx)中 [MS-SFU]: Kerberos 协议扩展：用户和受限委派协议规范服务。

若要配置资源服务允许用户替前端服务的访问权限，使用 Windows PowerShell cmdlet。

-   若要检索主体的列表，请使用**获取 ADComputer**，**获取 ADServiceAccount**，并**获取 ADUser**与 cmdlet**属性 PrincipalsAllowedToDelegateToAccount**参数。

-   配置资源服务，使用**新建 ADComputer**，**新建 ADServiceAccount**，**新建 ADUser**，**组 ADComputer**，**组 ADServiceAccount**，并**组 ADUser** cmdlet 与**PrincipalsAllowedToDelegateToAccount**参数。

## <a name="BKMK_SOFT"></a>软件要求
基于资源约束的委派只能在运行 Windows Server 2012 R2 和 Windows Server 2012，域控制器上配置，但可以内混合森林应用。

所有运行 Windows Server 2012 用户帐户的域中引用路径上运行的操作系统早于 Windows Server 前端和后台域之间的域控制器，必须将以下修补程序：基于资源受限的环境中有 Windows Server 2008 R2 基于域控制器 KDC_ERR_POLICY 失败委派。
