---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: "简介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>简介

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

只要有计算机存在计算基础结构，无论简单，也可以复杂的攻击。 但是，在过去十，越来越多的组织的世界上的所有部分中的大小，所有已攻击和威胁显著已更改威胁发展的方式。 录制的速度提高了网络战争犯罪。 "Hacktivism，"攻击通过 activist 持仓，在其中的促使已对大量的违规动机预期的公开组织的机密信息，以创建拒绝服务宣称甚至销毁基础结构。 针对目的是为了 exfiltrating 公开并专用机构攻击组织的知识属性 (IP) 越来越普遍。  
  
没有组织提供的信息 (IT) 技术基础结构是免受攻击，但如果相应策略、进程，并控制实现保护主要段组织的计算基础结构，升级到完全受损从穿透攻击的可能可以预防。 因为的数目和组织外部源自攻击的比例的已近年担忧预览体验成员威胁，本文档经常讨论外部攻击者，而不是不恰当使用的环境授权的用户。 不过，原则和本文中提供建议旨在帮助保护你免受外部攻击者和被误导或恶意预览体验成员的环境。  
  
信息和建议本文档中提供绘制从多个源中，并即派生自措施，旨在保护 Active Directory 安装免受威胁。 尽管不是可以阻止攻击，很可能降低 Active Directory 攻击 surface，而且实现控制，可以使受损更加困难的目录攻击。 本文档提供的最常见的我们所拥有的漏洞观察中受到威胁的环境和的最常见的建议，我们已对客户以提高其 Active Directory 安装的安全性。  
  
## <a name="account-and-group-naming-conventions"></a>帐户和组命名约定  
下表中提供用于本文档中的组和帐户引用整个文档命名约定指南。 在表中包含是每个帐户月组，项目名称时，如何将这些帐户月组引用本文档中的位置。  
  


|**帐户月组位置**|**帐户月组的名称**|**如何本文档中提到**|
| --- | --- | --- |   
|Active Directory 的每个域|管理员|内置的管理员帐户|  
|Active Directory 的每个域|管理员|管理员（栏）的内置组|  
|Active Directory 的每个域|域管理员|域管理员 (DA) 组|  
|Active Directory-森林根域|企业管理员|企业管理员 (EA) 组|  
|不可域控制器在运行 Windows Server 和工作站计算机上的本地计算机安全帐户管理器（三千）数据库。|管理员|本地管理员帐户|  
|不可域控制器在运行 Windows Server 和工作站计算机上的本地计算机安全帐户管理器（三千）数据库。|管理员|本地|  
  
## <a name="about-this-document"></a>有关本文档  
Microsoft 的信息安全和是一部分的 Microsoft 信息技术 (MSIT)，适用于内部业务设备、外部的客户和 industry 等收集的风险管理 (ISRM) 组织传播和定义策略、做法和控件。 Microsoft 和我们的客户可以使用此信息以增加安全性，并减少攻击 surface 其 IT 基础结构。 本文中提供的建议均基于大量的信息来源和做法 MSIT 和 ISRM 内使用。 以下各部分展示国度本文的详细信息。  
  
### <a name="microsoft-it-and-isrm"></a>MicrosoftIT 和 ISRM  
已 MSIT 和 ISRM 来保护 Microsoft 广告 DS 林和域中开发许多惯例和控件。 这些控件都适用，在他们已集成到此文档。 安全 T（新兴的技术解决方案加权）是的 ISRM 其特许是找出新兴技术，并定义的安全要求和控件，以加快其 adoption 团队。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全评估  
在 Microsoft ISRM，评估咨询，，工程 (ACE) 团队适用于内部 Microsoft business 单位和外部客户应用程序和基础结构安全评估并提供战略和战略指导提高组织的安全状态。 一个 ACE 服务提供是 Active Directory 安全评估 (ADSA)，这是从整体评估组织广告 DS 环境，评估人脉、进程和技术并生成特定于客户的建议。 客户提供了基于组织的唯一特征、做法和风险探索胃口的建议。 对于 Active Directory 的安装在 Microsoft 除客户进行了 ADSAs。 随着时间的推移大量建议找到要跨不同的大小和行业的客户适用。  
  
### <a name="content-origin-and-organization"></a>内容的原始和组织  
许多本文档中的内容被从 ADSA 和受损的客户和不遇到重大受损的客户执行其他 ACE 团队评估。 尽管个别客户数据不用于创建本文档，我们已收集了最常见的利用的漏洞，我们已标识在我们评估和建议我们已对客户以提高其广告 DS 安装的安全性。 并非所有漏洞都都适用于所有环境，也不都是推荐的所有可行要在每一个组织中实现。  
  
本文档按如下方式组织：  
  
## <a name="executive-summary"></a>摘要  
行政摘要，其中可以读取作为独立的文档或结合完整的文档，提供了高级摘要本文档。 包含在行政摘要是最常见的攻击源我们已注意到用于危害客户环境，摘要保护 Active Directory 安装和基本目标安全于计划部署新的广告 DS 林现在，或者在以后的客户的建议。  
  
### <a name="introduction"></a>简介  
这是你正在阅读的部分。  
  
### <a name="avenues-to-compromise"></a>危害的途径  
此部分中提供了对某些最信息通常利用的漏洞我们发现了用于攻击者会危及客户的基础结构。 此部分中的漏洞中，如何它们使用最初穿透客户的基础结构，在更多系统传播危害和最终定向广告 DS 和域控制器，若要获取组织的林完全控制常规类别开头。  
  
此部分中不提供有关解决每种类型的漏洞，尤其是在的区域中的漏洞不能用来直接目标 Active Directory 详细的建议。 但是，为了每种类型的安全漏洞，我们提供可用于开发对策，并减少你的组织攻击 surface 的其他信息的链接。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>减少 Active Directory 攻击 Surface  
这一部分开始通过提供有关特权的帐户和 Active Directory 提供的信息，可帮助阐明保护和管理特权的组和帐户的后续建议的原因中的组背景信息。 然后，我们可以讨论的方法来降低需要日常管理，不需要的授予组如企业管理员 (EA)、域管理员 (DA) 和内置管理员（栏）组中的 Active Directory 的权限级别为使用帐户高权限。 接下来，我们提供指南保护特权的组和帐户和实现安全管理惯例和系统。  
  
尽管此部分中提供了有关这些配置设置的详细的信息，我们也有包含附录为每个建议，提供了有关组织的需求配置的分步说明，可以使用"是"，也可以进行修改。 此部分中完成通过提供牢固部署和管理域控制器，应成为基础结构中严格最安全的系统的信息。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>危害的迹象监视 Active Directory  
是否已强大的安全信息和事件监视 (SIEM)，您的环境中实现，或使用其他机制监视器基础结构安全于，此部分中提供了可用于确定的则可能被攻击组织的 Windows 系统事件的信息。 我们讨论传统和高级审核策略，包括在 Windows 7 和 Windows Vista 操作系统中的有效的审核子类别中的配置。 此部分中包含的完整列表的对象和系统审核，并关联的附录列出的目标是检测危害尝试如果应监视器的事件。  
  
### <a name="planning-for-compromise"></a>对于危害计划  
此部分中，首先"单步执行返回"从技术的详细信息，可重点关注原则和流程，这可以实现来识别用户、应用和系统的最重要仅次于 IT 基础结构，但的业务。 确定后是什么最重要的稳定性和你的组织的操作，你可以专注于分离和保护措施这些资产，它们是否知识财产发件人或系统。 在某些情况下，分离并保护资产可能执行你现有的广告 DS 环境中在其他情况下，你应该考虑执行小、单独"单元格"，允许你建立周围关键资产和监视器的安全边界这些资产严格比不太重要的组件。 讨论了一个名为"创意销毁，"这是一种可通过创建新的解决方案依据消除旧版应用和系统机制的概念，并部分结束有助于保持通过集中业务和 IT 构建详细的图片的正常操作的状态的信息更安全的环境的建议。 了解什么是正常现象组织，可以更轻松地识别可能指示攻击和危害的异常情况。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳实践建议摘要  
此部分中提供表，对汇总在本文中所做的建议，并通过相对优先级，除了提供文档和其附录可以找到有关每个建议的详细信息的链接进行排序。  
  
### <a name="appendices"></a>附录  
附录纳入该文档，以增加文档正文中包含的信息。 附录和每个简短描述列表将包括下表。  
  
 
|**附录**|**描述**|
| --- | --- | 
|[附录 b：特权的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供可帮助你找出的用户和组你应该以此为重点保护因为以供攻击者甚至销毁 Active Directory 安装和危害的背景信息。|  
|[附录 c：受保护的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含有关 Active Directory 中的组受保护的信息。 它还包含有限自定义 （移除） 算是受保护的组并受 AdminSDHolder 和 SDProp 组的信息。|  
|[附录 d：保护 Active Directory 中的内置管理员帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含指南来帮助保护森林中的每个域中的管理员帐户。|  
|[附录 e：保护企业管理员中的组 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含来帮助保护企业管理员组森林中的指南。|  
|[附录 f：保护域的 Active Directory 的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含来帮助保护域管理员组森林中的每个域中的指南。|  
|[附录 g：保护中 Active Directory 的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含帮助安全森林中的每个域中的内置管理员组指南。|  
|[附录 h：保护措施的本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含帮助安全的本地管理员帐户，管理员组已加入域的服务器和工作站上的指南。|  
|[附录 i：创建管理帐户中的 Active Directory 的受保护的帐户和组](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供信息创建帐户，仅受有限权限和可以来严格控制，但可用于需要临时提升时填充特权 Active Directory 中的组。|  
|[附录 l：事件到监视器](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出在您的环境中应监视为其的事件。|  
|[附录 m：文档链接，建议阅读](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含推荐的阅读的列表。 此外包含一组链接到外部文档和它们的 Url，以便阅读器的硬盘副本本文可以访问此信息。|  
  


