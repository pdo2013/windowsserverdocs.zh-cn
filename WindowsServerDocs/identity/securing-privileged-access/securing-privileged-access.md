---
title: 保护特权访问
description: 保护特权访问的分阶段方法
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: e6ff22d0563fa11aa633004966b2cd2648ba5877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357701"
---
# <a name="securing-privileged-access"></a>保护特权访问

>适用于：Windows Server

保护特权权访问是在现代组织中为企业资产提供安全保证的首要关键步骤。 IT 组织中的大多数或所有业务资产的安全性取决于用于管理、管理和开发的特权帐户的完整性。 网络攻击者通常以这些帐户和特权访问的其他元素为目标，以使用凭据盗窃攻击（例如[传递哈希和传递票证](https://www.microsoft.com/pth)）访问数据和系统。

针对确定的攻击者保护特权访问要求您采取一种完整而周密的方法将这些系统与风险隔离开来。

## <a name="what-are-privileged-accounts"></a>什么是特权帐户？

在讨论如何保护它们之前，请定义特权帐户。

特权帐户（如 Active Directory 域服务的管理员）可以直接或间接访问 IT 组织中的大多数或所有资产，这会使这些帐户遭受严重的业务风险。

## <a name="why-securing-privileged-access-is-important"></a>为什么要保护特权访问非常重要？

网络攻击者将重点放在对诸如 Active Directory （AD）等系统的特权访问，以快速获取对目标数据的所有组织的访问权限。 传统的安全方法侧重于将网络和防火墙作为主要安全外围，但网络安全的有效性已经大大降低了两个趋势：

* 组织在移动企业电脑、移动电话和平板电脑等设备上托管数据和资源，并自带设备（BYOD）
* 通过网络钓鱼和其他 Web 及电子邮件攻击，攻击者已展示了在网络边界内获取工作站访问权限的连贯且持续的能力。

这些因素需要除了传统的网络外围策略外，还需要构建身份验证和授权标识控件之外的新式安全外围。 此处的安全外围定义为资产之间的一组一致的控制和对它们的威胁。 特权帐户有效控制这一新的安全外围网络，因此保护特权访问是至关重要的。

![组织的标识层图示](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

获得管理帐户控制权限的攻击者可以使用这些权限增加其在目标组织中的影响，如下所示：

![获得管理帐户控制的攻击者如何使用这些权限以目标组织利益为代价追求自己的利益图示](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

下图描绘了两个路径：

* 一个 "蓝色" 路径，在该路径中，标准用户帐户用于对资源（如电子邮件和 web 浏览）和日常工作的非特权访问。

   > [!NOTE]
   > 稍后所述的蓝色路径项指示超出管理帐户范围的广泛环境保护。

* 一个 "红色" 路径，在强化的设备上进行特权访问以降低网络钓鱼和其他 web 和电子邮件攻击的风险。

![显示 "路径" 的单独 "路径" 的关系图，该路线图建立用于隔离高风险标准用户任务（例如 web 浏览和访问电子邮件）的特权访问任务](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>保护特权访问路线图

此路线图旨在最大程度地利用已部署的 Microsoft 技术，利用云技术增强安全性，并集成你可能已部署的任何第三方安全工具。

Microsoft 建议的路线图分为3个阶段：

* [阶段1：前30天]()
   * 具有有意义的积极影响的快速 wins。
* [阶段2：90天]()
   * 显著的增量改进。
* [阶段3：持续]()
   * 安全改进和 sustainment。

路线图基于我们应对这些攻击和解决方案实施的经验，优先计划最有效和最快速的实现方案。 

Microsoft 建议按照此路线图来保护特许访问权限免受顽固攻击者的攻击。 你可以对此路线图进行调整，以满足组织的现有功能和特定要求。

> [!NOTE]
> 保护特权访问需要广泛的元素，包括技术组件（主机防御、帐户保护、身份管理等）、更改流程及管理实践和知识。 路线图的时间线都是近似值，基于我们的客户实施经验制定。 具体时间因组织不同而不同，取决于环境和更改管理流程的复杂性。

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>第 1 阶段：最小操作复杂性的快速 wins

路线图的阶段1重点是快速减少凭据被盗和滥用的最常用攻击方法。 阶段1设计为在大约30天内实现，在此图中进行了描述：

![阶段1关系图：1. 单独的管理员帐户和用户帐户2。 只需本地管理员密码3。 管理工作站阶段1、4。 标识攻击检测](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1.单独帐户

若要帮助将 internet 风险（仿冒攻击、web 浏览）与特权访问帐户区分开来，请为具有特权访问权限的所有人员创建专用帐户。 管理员不应该浏览 web、检查电子邮件，以及通过具有高特权帐户的日常工作效率任务。 有关详细信息，请参阅参考文档的[单独管理帐户](securing-privileged-access-reference-material.md#separate-administrative-accounts)部分。

按照文章管理 Azure AD 中的 "[管理紧急访问帐户](/azure/active-directory/users-groups-roles/directory-emergency-access)" 一文中的指导，在本地 AD 和 Azure AD 环境中至少创建两个具有永久分配的管理员权限的紧急访问帐户。 这些帐户仅在传统管理员帐户无法执行所需的任务（例如在发生灾难的情况下）时使用。

### <a name="2-just-in-time-local-admin-passwords"></a>2.实时本地管理员密码

为了降低攻击者从本地 SAM 数据库盗取本地管理员帐户密码哈希并使其滥用攻击其他计算机的风险，组织应确保每台计算机都具有唯一的本地管理员密码。 本地管理员密码解决方案（LAPS）工具可以在每个工作站上配置唯一的随机密码，并将其存储在受 ACL 保护 Active Directory （AD）中。 只有符合条件的授权用户才可以读取或请求重置这些本地管理员帐户密码。 你可以从[Microsoft 下载中心](http://Aka.ms/LAPS)获取用于工作站和服务器的 LAPS。

有关使用 LAPS 和 Paw 运行环境的其他指南，请参阅[基于清洁源原则的操作标准](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle)部分。

### <a name="3-administrative-workstations"></a>3.管理工作站

作为 Azure Active Directory 和传统本地 Active Directory 管理权限的用户的初始安全措施，请确保它们使用的 Windows 10 设备配置[为高度安全的 windows 10 设备的标准](/windows-hardware/design/device-experiences/oem-highly-secure). 特权管理员帐户不应是管理工作站本地管理员组的成员。  如果需要对工作站进行配置更改，则可以使用通过用户访问控制（UAC）的权限提升。  此外，Windows 10 安全基线还应应用到工作站，以便进一步强化设备。

### <a name="4-identity-attack-detection"></a>4.标识攻击检测

[Azure 高级威胁防护（ATP）](/azure-advanced-threat-protection/what-is-atp)是一种基于云的安全解决方案，用于识别、检测和帮助你调查本地 Active Directory 的高级威胁、泄露身份和恶意有问必答操作环境.

## <a name="phase-2-significant-incremental-improvements"></a>阶段 2:重大增量改进

阶段2基于阶段1中完成的工作构建，设计为在大约90天内完成。 该阶段的步骤如此图中所示：

![阶段2关系图：1. Windows Hello 企业版/MFA，2。 PAW 推出，3。 只需时间权限4。 Credential Guard 5。 泄漏的凭据，6。 横向移动漏洞检测](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1.需要 Windows Hello 企业版和 MFA

管理员可以受益于与 Windows Hello 企业版相关联的易用性。 管理员可以将其复杂密码替换为其电脑上的强双重身份验证。 攻击者必须同时拥有设备和生物识别信息或 PIN，而不需要员工的知识就更难获取访问权限。 有关 Windows Hello 企业版和推出的路径的更多详细信息，请参阅[Windows Hello 企业版概述](/windows/security/identity-protection/hello-for-business/hello-overview)

使用 Azure MFA 在 Azure AD 中为管理员帐户启用多重身份验证（MFA）。 至少启用[基线保护条件性访问策略](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)。有关 Azure 多重身份验证的详细信息，请参阅[部署基于云的 Azure 多重身份验证](/azure/active-directory/authentication/howto-mfa-getstarted)一文

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2.将 PAW 部署到所有特权标识访问帐户持有者

继续将特权帐户与电子邮件、web 浏览和其他非管理任务中的威胁隔离开来，您应该为所有有权访问您的组织的信息系统。 有关 PAW 部署的其他指南，请参阅[特权访问工作站](privileged-access-workstations.md#paw-phased-implementation)一文。

### <a name="3-just-in-time-privileges"></a>3.实时特权

若要降低权限的暴露时间并提高其使用的可见性，请使用相应的解决方案（如下面的或其他第三方解决方案）以时间（JIT）提供实时权限：

* 对于 Active Directory 域服务 (AD DS)，请使用 Microsoft 标识管理器 (MIM) 的[特权访问管理器 (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 功能。
* 对于 Azure Active Directory，请使用 [Azure AD 特权标识管理 (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan) 功能。

### <a name="4-enable-windows-defender-credential-guard"></a>4.启用 Windows Defender Credential Guard

启用 Credential Guard 有助于保护 NTLM 密码哈希、Kerberos 票证授予票证，以及应用程序存储为域凭据的凭据。 此功能可帮助防止凭据被盗攻击（例如传递哈希或传递票证），方法是使用盗用的凭据增加在环境中进行透视的难度。 有关 Credential Guard 的工作原理和部署方式的信息，请参阅[使用 Windows Defender Credential 防护保护派生的域凭据](/windows/security/identity-protection/credential-guard/credential-guard)一文。

### <a name="5-leaked-credentials-reporting"></a>5.泄漏的凭据报告

"每天，Microsoft 会分析超过6500000000000个信号，以识别新兴威胁并保护客户"- [Microsoft 按数字](https://news.microsoft.com/bythenumbers/cyber-attacks)

启用 Microsoft Azure AD Identity Protection 来报告具有泄露凭据的用户，以便可以对其进行修正。 可以利用[Azure AD Identity Protection](/azure/active-directory/identity-protection/index)来帮助组织保护云和混合环境免受威胁。

### <a name="6-azure-atp-lateral-movement-paths"></a>6.Azure ATP 横向移动路径

请确保特权访问帐户持有者正在使用其 PAW 进行管理，以使受攻击的非特权帐户无法通过凭据盗窃攻击（例如传递哈希或传递票证）访问特权帐户。 [AZURE ATP 横向移动路径（LMPs）](/azure-advanced-threat-protection/use-case-lateral-movement-path)提供了一种易于理解的报告，用于识别有权泄露的特权帐户。

## <a name="phase-3-security-improvement-and-sustainment"></a>阶段 3:安全改进和 sustainment

路线图的阶段3基于阶段1和步骤2中执行的步骤，以增强安全状况。 此图中直观地描述了阶段3：

![阶段 3:1. 查看 RBAC，2。 减少攻击面，3。 将日志与 SEIM、4集成。 泄漏的凭据自动化](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

这些功能将基于前面几个阶段中的步骤生成，并将防御转移到更主动的状况。 此阶段没有特定时间线，可能需要更长的时间来根据你的单个组织执行。

### <a name="1-review-role-based-access-control"></a>1.查看基于角色的访问控制

使用三个分层的模型一文所述[Active Directory 管理层模型](securing-privileged-access-reference-material.md)，查看并确保较低层管理员没有管理访问权限较高层资源 （组成员身份上的 Acl用户帐户，等等...)。

### <a name="2-reduce-attack-surfaces"></a>2.减少攻击面

强化标识工作负荷（包括域、域控制器、ADFS 和 Azure AD Connect）可能会导致组织中的其他系统受损。 本文[减小了 Active Directory 攻击面](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md)以及[用于保护标识基础结构的五个步骤](/azure/security/azure-ad-secure-steps)，为保护本地和混合标识环境提供了指导。

### <a name="3-integrate-logs-with-siem"></a>3.将日志与 SIEM 集成

将日志记录集成到集中式 SIEM 工具可帮助组织分析、检测和响应安全事件。 [监视 Active Directory，以应对泄露迹象](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md)和[附录 L：要监视](../ad-ds/plan/appendix-l--events-to-monitor.md)的事件提供有关应在环境中监视的事件的指导。

这是超出计划的一部分，因为在安全信息和事件管理（SIEM）中聚合、创建和调整警报需要训练有素的分析师（与30天计划中的 Azure ATP，其中包括现成警报）

### <a name="4-leaked-credentials---force-password-reset"></a>4.泄漏的凭据-强制密码重置

通过允许 Azure AD Identity Protection 在怀疑密码泄露时自动强制重置密码，继续增强安全状况。 本文介绍了[使用风险事件触发多重身份验证和密码更改](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)一文中的指南，说明了如何使用条件性访问策略启用此操作。

## <a name="am-i-done"></a>我是否已经完成？

简短的答案是 "否"。

糟糕的家伙永远都不会停止，因此您也不能。 此路线图可帮助你的组织防范当前已知的威胁，因为攻击者会不断发展并转变。 建议你查看安全，这是一个持续的过程，侧重于提高成本并降低目标环境攻击者的成功率。

尽管这不是你组织的安全计划的唯一部分，但保护特权访问是安全策略的关键组成部分。
