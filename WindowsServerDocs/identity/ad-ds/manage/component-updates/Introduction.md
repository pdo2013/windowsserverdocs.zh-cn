---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: 介绍
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab21cca727342f6dc69ceecfb0c8991b30b0f227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389878"
---
# <a name="introduction"></a>介绍

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

对于计算基础结构的攻击（无论是简单还是复杂），只要计算机存在即可。 但是，在过去十年中，世界上所有地区的所有规模的组织中有越来越多的组织受到攻击和破坏，这些攻击和破坏的方式显著改变了威胁态势。 网络战争和网络犯罪以创记录的速度在增长。 "黑客主义"，在这种情况下，攻击是通过 activist 位置进行的，已被声称作为一些违规的动机，旨在公开组织的机密信息、创建服务拒绝甚至销毁基础结构。 针对公共和私有机构的攻击（窃取组织的知识财产（IP））已变得无处不在。  
  
没有信息技术（IT）基础结构的组织不受攻击，但如果实施适当的策略、流程和控制来保护组织计算基础结构的关键段，则从渗透到完全破坏可能是预防的。 由于从组织外部发起的攻击数量和规模在最近几年 eclipsed 内部威胁，因此本文档通常会讨论外部攻击者，而不是由授权用户滥用环境。 尽管如此，本文档中提供的原则和建议旨在帮助保护环境免受外部攻击者以及有被误导或恶意内部人员的攻击。  
  
本文档中提供的信息和建议是从多个源中提取出来的，并派生自旨在保护 Active Directory 安装免遭破坏的做法。 尽管无法防止攻击，但是可以减少 Active Directory 攻击面，并实现使目录泄露的控件，这对攻击者来说更难。 本文档介绍了我们在已泄露环境中发现的最常见的漏洞，以及我们为客户提供的最常见的建议，以提高其 Active Directory 安装的安全性。  
  
## <a name="account-and-group-naming-conventions"></a>帐户和组命名约定  
下表提供了本文档中使用的命名约定的指南，适用于整个文档所引用的组和帐户。 表中包含每个帐户/组的位置、名称，以及如何在本文档中引用这些帐户/组。  
  


|**帐户/组位置**|**帐户/组的名称**|**本文档中的引用方式**|
| --- | --- | --- |   
|Active Directory-每个域|管理员|内置管理员帐户|  
|Active Directory-每个域|Administrators|内置管理员（BA）组|  
|Active Directory-每个域|Domain Admins|域管理员（DA）组|  
|Active Directory 林根级域|Enterprise Admins|Enterprise Admins （EA）组|  
|不是域控制器的计算机上运行 Windows Server 和工作站的本地计算机安全帐户管理器（SAM）数据库|管理员|本地管理员帐户|  
|不是域控制器的计算机上运行 Windows Server 和工作站的本地计算机安全帐户管理器（SAM）数据库|Administrators|本地管理员组|  
  
## <a name="about-this-document"></a>关于本文档  
Microsoft 信息技术（MSIT）中的 Microsoft 信息安全和风险管理（ISRM）组织可与内部业务部门、外部客户和行业同行合作，收集、传播和定义策略。实践和控件。 Microsoft 和我们的客户可使用此信息来提高安全性并减少其 IT 基础结构的受攻击面。 本文档中提供的建议基于 MSIT 和 ISRM 中使用的多个信息源和实践。 以下各节提供了有关此文档的来源的详细信息。  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM  
MSIT 和 ISRM 中已开发了许多实践和控件，用于保护 Microsoft AD DS 的林和域。 如果这些控件广泛适用，则已将其集成到本文档中。 SAFE （用于新兴技术的解决方案加速器）是 ISRM 中的一个团队，其职责是识别新兴技术，并定义安全要求和控制以加速其采用。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全评估  
在 Microsoft ISRM 中，评估、咨询和工程（ACE）团队与内部 Microsoft 业务部门和外部客户合作，以评估应用程序和基础结构的安全性，并提供战术性和战略指导以提高组织的安全状况。 一个 ACE 服务产品是 Active Directory 安全评估（ADSA），它是组织 AD DS 环境的整体评估，可评估人员、流程和技术并生成特定于客户的建议。 根据组织的独特特征、做法和风险兴趣，向客户提供建议。 除了我们的客户外，还对 Microsoft 中的 Active Directory 安装执行了 ADSAs。 随着时间的推移，我们发现大量的建议适用于不同规模和行业的客户。  
  
### <a name="content-origin-and-organization"></a>内容来源和组织  
本文档中的很多内容都是从 ADSA 和其他 ACE 团队评估派生的，这些评估是为遭受侵害的客户和不会产生严重损害的客户而执行的。 尽管未使用个人客户数据创建本文档，但我们收集了我们在评估中确定的最常被利用的漏洞，以及我们为客户提供的建议，以提高其 AD DS 的安全性成千上万. 并非所有漏洞都适用于所有环境，也并非所有建议都可在每个组织中实现。  
  
本文档的结构如下所示：  
  
## <a name="executive-summary"></a>执行摘要  
执行摘要（可作为独立文档读取或与完整文档一起读取）提供了本文档的高级摘要。 执行摘要中包含的是我们发现的用于破坏客户环境的最常见攻击媒介、用于保护 Active Directory 安装的摘要建议以及计划部署新 AD DS 的客户的基本目标林。  
  
### <a name="introduction"></a>介绍  
这是你现在正在阅读的部分。  
  
### <a name="avenues-to-compromise"></a>危及系统安全的途径  
本部分介绍一些最常利用的漏洞的信息，攻击者可能会利用这些漏洞来损害客户的基础结构。 本部分首先介绍常见的漏洞类别，以及如何利用这些漏洞来初步了解客户的基础结构，在其他系统之间传播泄露，并最终定位到 AD DS 和域控制器以获得完整控制组织的林。  
  
本部分不提供有关解决每种类型的漏洞的详细建议，尤其是在未使用漏洞直接以 Active Directory 的情况下。 但对于每种类型的漏洞，我们提供了指向其他信息的链接，你可以使用这些信息来开发对策并减少组织的攻击面。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>减少 Active Directory 攻击面  
本部分首先提供有关 Active Directory 中的特权帐户和组的背景信息，以提供相关信息，帮助阐明用于保护和管理特权组的后续建议的原因，以及帐户. 接下来，我们将讨论降低使用高特权帐户进行日常管理的方法，这不需要授予组（如 Enterprise Admins （EA）、Domain Admins （DA）和内置的权限级别）Active Directory 中的管理员（BA）组。 接下来，我们将提供有关保护特权组和帐户以及实现安全管理实践和系统的指导。  
  
虽然本部分提供了有关这些配置设置的详细信息，但我们还提供了每个建议的附录，这些附录提供可按原样使用的分步配置说明，或者可以修改组织的需求。 本部分通过提供信息来安全地部署和管理域控制器（应在基础结构中最得到的安全系统中）来完成。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>监视 Active Directory 遭到破坏的迹象  
无论你是在环境中实施了可靠的安全信息和事件监视（SIEM）还是使用其他机制来监视基础结构的安全性，本部分都提供了可用于在 Windows 上标识事件的信息可能指示组织正在受到攻击的系统。 我们讨论传统和高级审核策略，包括 Windows 7 和 Windows Vista 操作系统中审核子类别的有效配置。 本部分包含要审核的对象和系统的综合列表，以及相关的附录，其中列出了应监视的事件，前提是该目标是要检测折衷尝试。  
  
### <a name="planning-for-compromise"></a>规划泄露  
本部分首先介绍技术详细信息，重点介绍可实现的原则和流程，以确定对 IT 基础结构最重要的用户、应用程序和系统，但对业务而言是最重要的。 确定组织的稳定性和运营最重要的内容后，可以专注于隔离和保护这些资产，无论他们是知识产权、人员还是系统。 在某些情况下，可以在现有 AD DS 环境中执行隔离和保护资产，而在其他情况下，应考虑实施小型独立的 "单元"，使用户能够围绕关键资产建立安全边界并监视资产比不太重要的组件得到。 一种名为 "创造性销毁" 的概念，它是一种机制，通过这种机制，可以通过创建新的解决方案来消除旧的应用程序和系统，并以可帮助维护更安全的环境将业务和 IT 信息结合起来，以构造正常操作状态的详细信息。 了解组织的正常情况后，可以更轻松地确定攻击和威胁的异常。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳做法建议摘要  
本部分提供了一个表，其中汇总了本文档中所做的建议，并按相对优先级对它们进行排序，此外还提供了一些链接，可在文档及其附录中找到有关每个建议的详细信息。  
  
### <a name="appendices"></a>附录  
本文档中包含附录，以补充文档正文中包含的信息。 下表列出了附录和每个附录的简要说明。  
  
 
|**附录**|**说明**|
| --- | --- | 
|[附录 B：Active Directory 中有权限的帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供的背景信息可帮助你识别你应该将精力集中在保护上的用户和组，因为攻击者可能会利用它们来损害甚至销毁你的 Active Directory 安装。|  
|[附录 C：Active Directory 中受保护的帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含有关 Active Directory 中的受保护组的信息。 它还包含对被视为受保护组的组的有限自定义（删除）的信息，这些信息受 AdminSDHolder 和 SDProp 的影响。|  
|[附录 D：保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含帮助保护林中每个域中的管理员帐户的准则。|  
|[附录 E：保护 Active Directory 中的 Enterprise Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含帮助保护林中的 Enterprise Admins 组的准则。|  
|[附录 F：保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含帮助保护林中每个域中的 Domain Admins 组的准则。|  
|[附录 G：保护 Active Directory 中的 Administrators 组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含帮助保护林中每个域中的内置 Administrators 组的准则。|  
|[附录 H：保护本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含一些指导原则，可帮助保护已加入域的服务器和工作站上的本地管理员帐户和管理员组。|  
|[附录 I：为 Active Directory 中受保护的帐户和组创建管理帐户](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供有关创建帐户的信息，这些帐户具有有限的权限，并且可以得到控制，但当需要临时提升时，可用于填充 Active Directory 中的特权组。|  
|[附录 L：要监视的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出应该在环境中监视的事件。|  
|[附录 M：文档链接和建议的读物](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含建议阅读的列表。 还包含指向外部文档及其 Url 的链接的列表，以便此文档的硬拷贝读者可以访问此信息。|  
  


