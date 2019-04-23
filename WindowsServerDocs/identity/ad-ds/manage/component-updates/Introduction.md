---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: 简介
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831868"
---
# <a name="introduction"></a>简介

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

只要计算机具有已存在于针对计算基础结构，无论是简单还是复杂的攻击。 但是，在过去十年中，世界上所有地区的所有规模的组织中有越来越多的组织受到攻击和破坏，这些攻击和破坏的方式显著改变了威胁态势。 网络战争和网络犯罪以创记录的速度在增长。 "黑客主义"在其中攻击推动了 activist 位置已声明为一个违规次数的动机，旨在公开组织的机密信息，以创建拒绝服务或甚至销毁基础结构。 公共和私有机构的目标是窃取对攻击组织的知识产权 (IP) 已成为普遍。  
  
信息技术 (IT) 基础结构的任何组织不都会遭受攻击，但如果相应的策略、 流程和控件实现来保护组织的计算基础结构，受到攻击的升级的关键段渗透严重危害可以预防。 数字和小数位数源自组织外部的攻击大关内部威胁在最近几年中，因为本文档经常讨论外部攻击者而不是获得授权的用户环境的误用。 但是，原则和本文档中提供的建议旨在帮助保护环境防范外部攻击者以及受到误导或恶意内部人员。  
  
信息和本文档中提供的建议是来自多个源并源自旨在保护 Active Directory 安装免遭破坏的做法。 尽管不能防止攻击，但有以减少 Active Directory 攻击面并实现控件，使攻击者要困难得多的目录的折衷。 本文档提供了最常见类型的漏洞，我们观察到在受破坏的环境和最常见建议我们进行到客户以改进其 Active Directory 安装的安全性。  
  
## <a name="account-and-group-naming-conventions"></a>帐户和组命名约定  
下表提供了用于本文档中的组和帐户，本文通篇引用的命名约定的指南。 表中包含的是每个帐户/组、 名称和如何在本文档中引用这些帐户/组的位置。  
  


|**帐户/组位置**|**帐户/组的名称**|**本文档中的引用方式**|
| --- | --- | --- |   
|Active Directory-每个域|管理员|内置管理员帐户|  
|Active Directory-每个域|Administrators|内置管理员 (BA) 组|  
|Active Directory-每个域|Domain Admins|域管理员 (DA) 组|  
|Active Directory 的目录林根域|Enterprise Admins|Enterprise Admins (EA) 组|  
|在运行 Windows Server 和工作站的计算机上的本地计算机安全帐户管理器 (SAM) 数据库不是域控制器|管理员|本地管理员帐户|  
|在运行 Windows Server 和工作站的计算机上的本地计算机安全帐户管理器 (SAM) 数据库不是域控制器|Administrators|本地管理员组|  
  
## <a name="about-this-document"></a>有关本文档  
Microsoft 信息安全和风险管理 (ISRM) 组织，是一部分的 Microsoft 信息技术 (MSIT)，适用于内部业务部门、 外部客户和业内同行来收集、 传播和定义策略，实践和控件。 由 Microsoft 和我们的客户可以使用此信息以提高安全性并减少其 IT 基础结构的受攻击面。 本文档中提供的建议基于大量的信息源和 MSIT 和 ISRM 内的做法。 以下各节提供本文档的详细信息的来源。  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM  
MSIT 和 ISRM 来保护 Microsoft AD DS 林和域中已开发的实践和控件数。 这些控件的广泛应用，它们已集成到本文档。 安全-T （新兴技术的解决方案加速器） 是中 ISRM 来识别新兴技术，并定义需要加速其采用的安全要求和控件是一个团队。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全性评估  
在与内部 Microsoft 业务部门和外部客户来评估应用程序和基础结构安全性并提供战术和战略指导以提高 Microsoft ISRM、 评估、 咨询和工程 (ACE) 团队的工作原理组织的安全状况。 一个 ACE 服务产品是活动的 Directory 安全性评估 (ADSA)，这是组织的 AD DS 环境的人员、 流程和技术评估，并生成特定于客户的建议的全面评估。 客户提供基于组织的独特特征、 做法和风险偏好的建议。 除了我们的客户在 Microsoft 的 Active Directory 安装进行了 ADSAs。 随着时间推移，具有已经发现了许多建议应用于不同大小和行业的客户。  
  
### <a name="content-origin-and-organization"></a>内容来源和组织  
本文档的内容大部分被派生自 ADSA 和为遭到入侵的客户和没遇到重大破坏的客户执行其他 ACE 团队评估。 尽管个人客户数据未用于创建此类文档，我们已收集通常利用我们已识别的漏洞在我们的评估和建议我们进行到客户以改进其 AD DS 的安全安装。 并非所有漏洞都适用于所有环境，也并非所有建议都可在每个组织中实现。  
  
本文档的结构，如下所示：  
  
## <a name="executive-summary"></a>执行摘要  
执行摘要，可作为独立的文档或结合使用完整的文档中读取，提供本文档的高级摘要。 执行摘要包括最常见的攻击媒介，我们观察到从而危及客户环境，用于保护对于打算部署新的 AD DS 的客户的 Active Directory 安装和基本目标的摘要建议现在或将来的林。  
  
### <a name="introduction"></a>简介  
这是您正在阅读的部分。  
  
### <a name="avenues-to-compromise"></a>危及系统安全的途径  
本部分提供有关的一些信息通常利用漏洞我们发现，要用于攻击者危及客户的基础结构。 本部分开头的漏洞和如何它们可用于最初入侵客户的基础结构，在其他系统之间传播泄漏和最终目标 AD DS 和域控制器，以获取完整的常规类别组织的林的控件。  
  
本节不提供详细的建议如何解决每种类型的漏洞，尤其是在漏洞不在其中用于直接指向 Active Directory 的区域。 但是，对于每个类型的漏洞，我们提供了可用于开发对策和降低组织的攻击面上的其他信息的链接。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>减少 Active Directory 攻击面  
本节首先提供了有关权限的帐户和 Active Directory 来提供帮助说明保护和管理特权的组的后续建议的原因的信息中的组的背景信息和帐户。 然后，我们介绍的方法来降低需要使用高特权的帐户进行日常的管理，不需要的如 Enterprise Admins (EA)、 域管理员 (DA) 和内置的组授予的权限级别Active Directory 中的管理员 (BA) 组。 接下来，我们提供指导，用于保护特权的组和帐户以及实现安全的管理做法和系统。  
  
虽然本部分提供了有关这些配置设置的详细的信息，但我们还包括提供了分步配置说明，可以使用"按原样"也可以修改的每个建议的附录组织的需求。 本部分中完成，从而安全地部署和管理域控制器，应在基础结构中的最严格安全系统之间的信息。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>监视 Active Directory 遭到破坏的迹象  
是否已实施了功能强大的安全信息和事件监视您的环境中 (SIEM) 或使用其他机制来监视基础结构的安全性，本部分提供可以用于标识 Windows 上的事件的信息可能表示组织受到攻击的系统。 我们将讨论传统和高级审核策略，包括在 Windows 7 和 Windows Vista 操作系统中的审核子类别的有效配置。 本部分包含的对象和要审核的系统的全面列表并关联的附录列出了应为其监视如果目标是检测入侵尝试的事件。  
  
### <a name="planning-for-compromise"></a>规划泄露  
本部分的"单步执行后"技术的详细信息，可以专注于原则和流程，可以通过实现来识别用户、 应用程序和系统的最关键的 IT 基础结构，不仅从一开始，但业务。 确定什么最重要的稳定性和组织的操作之后, 您可以集中精力分离和保护这些资产，无论它们是知识产权、 人员或系统。 在某些情况下，分离和保护资产可能会执行在现有 AD DS 环境中，在其他情况下，应考虑实现小型的单独"单元格"可用于建立各地关键资产的安全边界和监视这些资产不太重要的组件更严格地。 讨论名为"creative 析构，"这是一种机制，可以通过创建新的解决方案依据消除旧版应用程序和系统的概念，和部分结尾的可帮助维护更安全的环境的建议组合业务和 IT 信息来构造详细的描述什么是正常的操作状态。 通过了解什么正常的组织，可以更轻松地标识可能表示攻击和破坏的异常情况。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳做法建议的摘要  
本部分提供了表总结了本文档中的建议，并进行排序的相对优先级，除了提供文档和其附录中可以找到有关每个建议的详细信息的链接。  
  
### <a name="appendices"></a>附录  
附录会参与此文档来加强对文档正文中包含的信息。 附录和每个的简要说明的列表是包含下表。  
  
 
|**附录**|**说明**|
| --- | --- | 
|[附录 b:权限的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供可帮助你标识的用户和组应重点保护因为便可供攻击者能够破坏，甚至会破坏您的 Active Directory 安装的背景信息。|  
|[附录 c:受保护的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含有关 Active Directory 中的受保护组的信息。 它还包含有限的自定义项 （删除） 将被视为受保护的组和 AdminSDHolder 和 SDProp 会影响的组的信息。|  
|[附录 d:确保在 Active Directory 中的内置 Administrator 帐户的安全](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含一些指导原则来帮助保护在林中每个域中的管理员帐户。|  
|[附录 e:保护 Active Directory 中的 Enterprise Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含一些指导原则来帮助保护林中的 Enterprise Admins 组。|  
|[附录 f:保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含一些指导原则来帮助保护在林中每个域中的 Domain Admins 组。|  
|[附录 g:保护 Active Directory 中的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含一些指导原则来帮助保护林中每个域中的内置管理员组。|  
|[附录 h:保护本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含有助于安全的本地管理员帐户和已加入域的服务器和工作站上的管理员组的指导原则。|  
|[附录 i:为受保护的帐户和组在 Active Directory 中的创建管理帐户](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供用于创建具有有限的特权和可以进行严格控制，但可以用于需要临时提升时填充 Active Directory 中的特权的组的帐户信息。|  
|[附录 l:监视的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出了应为其监视您的环境中的事件。|  
|[附录 m:文档链接和建议的读物](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含一系列建议阅读。 此外包含指向外部文档的链接列表和其 Url，以便将硬拷贝本文档的读取器可以访问此信息。|  
  


