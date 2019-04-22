---
title: 保护特权的访问
description: 保护特权的访问的分阶段的方法
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822008"
---
# <a name="securing-privileged-access"></a>保护特权的访问

>适用于：Windows Server

保护特权权访问是在现代组织中为企业资产提供安全保证的首要关键步骤。 在 IT 组织中的大多数或所有业务资产的安全性取决于用于管理、 管理和开发的特权帐户的完整性。 网络攻击者通常会以这些帐户和特权访问来访问数据和使用凭据被盗攻击，例如系统的其他元素[传递的哈希和票证传递](https://www.microsoft.com/pth)。

保护特权访问权限免受顽固需要采用完整且周密的方法来隔离这些系统与风险。

## <a name="what-are-privileged-accounts"></a>什么是特权的帐户？

我们讨论如何保护它们之前，我们定义特权的帐户。

等的 Active Directory 域服务管理员的特权的帐户可以直接或间接访问大多数或所有资产在 IT 组织中，使这些帐户的安全威胁的重要业务风险。

## <a name="why-securing-privileged-access-is-important"></a>为什么保护特权的访问很重要？

网络攻击者专注于对系统，例如 Active Directory (AD) 来快速访问到所有组织目标数据特许访问权限。 传统的安全方法致力于网络和防火墙作为主要安全边界，但以下两种趋势大大降低了网络安全的有效性：

* 组织托管数据，并且移动企业 Pc、 手机和平板电脑等设备上的传统网络边界以外的资源云服务，并将自己的设备 (BYOD)
* 通过网络钓鱼和其他 Web 及电子邮件攻击，攻击者已展示了在网络边界内获取工作站访问权限的连贯且持续的能力。

这些因素需要生成除了传统网络外围策略的标识控件的新式安全外围带身份验证和授权。 安全外围，被指一组一致的控件之间的资产并对它们的威胁。 特权的帐户可以有效地控制的此新的安全边界因此务必保护特权访问权限。

![组织的标识层图示](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

攻击者获取控制权的管理帐户可以使用这些权限以提高其对目标组织的影响，如下所示：

![获得管理帐户控制的攻击者如何使用这些权限以目标组织利益为代价追求自己的利益图示](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

下图描绘了两个路径：

* 标准用户帐户用于对电子邮件和 web 浏览和日常工作等资源的非特权访问的"蓝色"路径都已完成。

   > [!NOTE]
   > 更高版本上所述的蓝色路径项指示广泛扩展到管理帐户之外的环境保护措施。

* 特许访问权限以减少网络钓鱼和其他 web 和电子邮件的攻击的风险的强化设备发生的位置是"红色"路径。

![关系图显示路线图建立隔离特权的访问任务从高风险标准用户任务，例如 web 浏览和访问电子邮件以进行管理的单独的"路径"](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>保护特权的访问路线图

路线图旨在最大限度利用云技术，从而增强安全性，并集成任何你可能已部署的第三方安全工具的已有部署，Microsoft 技术的使用。

Microsoft 建议的路线图分为 3 个阶段：

* [第 1 阶段：前 30 天内]()
   * 使用有意义的积极影响的快速 wins。
* [阶段 2:90 天]()
   * 重要的增量改进。
* [阶段 3:正在进行]()
   * 安全性改进和 sustainment。

路线图基于我们应对这些攻击和解决方案实施的经验，优先计划最有效和最快速的实现方案。 

Microsoft 建议按照此路线图来保护特许访问权限免受顽固攻击者的攻击。 你可以对此路线图进行调整，以满足组织的现有功能和特定要求。

> [!NOTE]
> 保护特权访问需要广泛的元素，包括技术组件（主机防御、帐户保护、身份管理等）、更改流程及管理实践和知识。 路线图的时间线都是近似值，基于我们的客户实施经验制定。 具体时间因组织不同而不同，取决于环境和更改管理流程的复杂性。

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>第 1 阶段：使用最小操作复杂性快速 wins

路线图的阶段 1 侧重于快速缓解使用频率最高的攻击技术的凭据被盗和滥用。 阶段 1 设计为在大约 30 天内实现进行了描述在此关系图：

![关系图第 1 阶段：1. 单独管理员和用户帐户，2。 恰时本地管理员密码，3。 管理员工作站阶段 1，4。 标识攻击检测](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1.单独的帐户

若要将 internet 风险 (仿冒攻击、 web 浏览) 从特权访问帐户，请为具有特权访问权限的所有人员创建专用的帐户。 管理员应该不会在浏览 web、 检查其电子邮件，并使用高特权帐户的日常工作效率任务。 部分中可找到对此的更多信息[单独的管理帐户](securing-privileged-access-reference-material.md#separate-administrative-accounts)的参考文档。

遵循本文中的指导[在 Azure AD 中管理紧急访问帐户](/azure/active-directory/users-groups-roles/directory-emergency-access)创建至少两个紧急访问帐户，具有永久分配的管理员权限，在这两个本地 AD 和 Azure AD 环境. 无法执行所需的任务例如，在传统的管理员帐户时，这些帐户是仅用于在发生灾难的情况。

### <a name="2-just-in-time-local-admin-passwords"></a>2.恰时本地管理员密码

若要降低攻击者窃取本地管理员帐户密码哈希从本地 SAM 数据库和其滥用于攻击其他计算机的风险，组织应确保每台计算机具有唯一的本地管理员密码。 本地管理员密码解决方案 (LAPS) 工具可以在每个工作站上配置唯一的随机密码和服务器将其存储在 Active Directory (AD) 保护的 ACL。 只有符合条件的授权的用户可读取或请求的这些本地管理员帐户密码重置。 你可以获取在工作站和服务器上使用 LAPS [Microsoft Download Center](http://Aka.ms/LAPS)。

可以部分中找到有关操作通过 LAP 和 Paw 的环境的其他指导[操作标准基于清洁源原则](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle)。

### <a name="3-administrative-workstations"></a>3.管理工作站

作为 Azure Active Directory 和传统的本地 Active Directory 管理权限的用户的初始安全措施，确保他们正在使用 Windows 10 设备配置了[高度安全的 Windows 的标准10 个设备](/windows-hardware/design/device-experiences/oem-highly-secure)。 

### <a name="4-identity-attack-detection"></a>4.标识攻击检测

[Azure 高级威胁防护 (ATP)](/azure-advanced-threat-protection/what-is-atp)是基于云的安全解决方案，用于标识、 检测到，并可帮助你调查高级的威胁、 遭到入侵的标识和恶意内部操作定向到你的本地活动Directory 环境。

## <a name="phase-2-significant-incremental-improvements"></a>阶段 2:重要的增量改进

阶段 2 在阶段 1 中所做的工作为基础，专为在大约 90 天内完成。 该阶段的步骤如此图中所示：

![第 2 阶段关系图：1. Windows hello 企业版 / MFA，2。 PAW 推出，3。 只需在时间特权，4。 Credential Guard，5。 泄漏的凭据，6。 横向移动的漏洞检测](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1.需要 Windows Hello for Business 和 MFA

管理员可以受益于与 Windows hello 企业版关联的易用性。 管理员可以使用在其电脑上的强双因素身份验证替换其复杂的密码。 攻击者必须安装在设备和生物识别信息或 PIN，才能访问该员工的不知情的情况下困难得多。 可以在文章中找到有关 Windows Hello for Business 和要推出的路径的详细信息[Windows Hello for Business 概述](/windows/security/identity-protection/hello-for-business/hello-overview)

在使用 Azure MFA 的 Azure AD 中的管理员帐户启用多重身份验证 (MFA)。 在最小启用[基线保护条件性访问策略](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)一文中可以找到有关 Azure 多重身份验证详细信息[部署基于云的 Azure 多重身份验证](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2.将 PAW 部署到所有特权的标识的访问帐户持有者

在继续从电子邮件、 浏览 web，和其他非管理任务中发现的威胁分离特权的帐户的过程，则应实现专用特权访问工作站 (PAW) 特权访问的所有人员的应用组织的信息系统。 可以在文章中找到 PAW 部署的其他指引[特权访问工作站](privileged-access-workstations.md#paw-phased-implementation)。

### <a name="3-just-in-time-privileges"></a>3.只需在权限时

若要减少特权的曝露时间并提高其使用的可见性，提供实时 (JIT) 使用如下面的合适的解决方案或其他第三方解决方案：

* 对于 Active Directory 域服务 (AD DS)，请使用 Microsoft 标识管理器 (MIM) 的[特权访问管理器 (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) 功能。
* 对于 Azure Active Directory，请使用 [Azure AD 特权标识管理 (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan) 功能。

### <a name="4-enable-windows-defender-credential-guard"></a>4.启用 Windows Defender Credential Guard

启用 Credential Guard 可以帮助保护 NTLM 密码哈希、 Kerberos 票证授予票证和域凭据的应用程序所存储的凭据。 此功能可帮助防止凭据被盗攻击，例如传递哈希或传递票证通过增加使用窃取的凭据在环境中正在执行的切换的难度。 Credential Guard 的工作原理以及如何部署的信息可在本文[保护派生的域凭据与 Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)。

### <a name="5-leaked-credentials-reporting"></a>5.已泄漏的凭据报表

"每日，Microsoft 会分析以便识别新出现的威胁和保护客户的 6.5 1 万亿信号"- [Microsoft 的号码](https://news.microsoft.com/bythenumbers/cyber-attacks)

启用 Microsoft Azure AD Identity Protection 具有已泄漏凭据的用户，以便您可以修正这些报告。 [Azure AD Identity Protection](/azure/active-directory/identity-protection/index)可用来帮助您免受威胁保护云和混合环境的组织。

### <a name="6-azure-atp-lateral-movement-paths"></a>6.Azure ATP 横向移动路径

请确保特权访问帐户持有人使用其 PAW 管理仅以便对该遭到入侵的非特权帐户无法访问特权帐户，通过凭据被盗攻击，例如传递哈希或传递票证攻击。 [Azure ATP 横向移动路径 (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path)可以非常方便地了解报告来标识权限的帐户可能处于打开状态以破坏。

## <a name="phase-3-security-improvement-and-sustainment"></a>阶段 3:安全性改进和 sustainment

路线图的阶段 3 为基础来加强安全态势阶段 1 和 2 中所采取的步骤。 在此图中直观地描述了第 3 阶段：

![阶段 3:1. 查看 RBAC，2。 减少攻击面，3。 将日志与 SEIM，4 集成。 已泄漏的凭据自动化](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

这些功能将生成在前几个阶段中的步骤，并将您的防御措施移到更加主动的状态。 此阶段中具有任何特定的时间线，可能需要更长时间才能实现基于每个组织。

### <a name="1-review-role-based-access-control"></a>1.查看基于角色的访问控制

使用三个分层的模型一文所述[Active Directory 管理层模型](securing-privileged-access-reference-material.md)，查看并确保较低层管理员没有管理访问权限较高层资源 （组成员身份上的 Acl用户帐户，等等。...)。

### <a name="2-reduce-attack-surfaces"></a>2.减少攻击面

强化您标识工作负荷，包括域、 域控制器、 ADFS 和 Azure AD Connect 会影响这些系统中的一个可能会导致你的组织中的其他系统的受到安全威胁。 文章[减少 Active Directory 攻击面](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md)并[保护标识基础结构的五个步骤](/azure/security/azure-ad-secure-steps)指导保护本地和混合标识环境。

### <a name="3-integrate-logs-with-siem"></a>3.将日志与 SIEM 集成

将日志记录集成到集中式的 SIEM 工具可以帮助您的组织来分析、 检测和响应安全事件。 文章[泄漏符号的监视 Active Directory](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md)和[附录 l:监视的事件](../ad-ds/plan/appendix-l--events-to-monitor.md)应在你的环境中监视的事件提供指导。

这是更高版本的一部分计划聚合，因为创建，并优化中的安全信息和事件管理 (SIEM) 的警报需要熟练分析师 （不同于 Azure ATP 中 30 天的计划，其中包括带框警报)

### <a name="4-leaked-credentials---force-password-reset"></a>4.已泄漏的凭据-强制密码重置

继续通过启用 Azure AD Identity Protection 会自动强制密码重置时密码怀疑遭到破坏的增强安全状况。 本指南的文章中找到[使用触发器多重身份验证和密码更改的风险事件](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)介绍了如何启用此功能使用条件性访问策略。

## <a name="am-i-done"></a>我是否已经完成？

简短的答案是没有。

不良分子的入侵永远不会停止，因此都不可以。 此路线图可帮助防止您的组织当前已知的威胁的攻击者将不断地发展和转变。 我们建议您查看安全性作为一个持续的过程侧重于提高成本并减少攻击者以你的环境为目标的成功率。

尽管这不是保护特权的访问你组织的安全计划的唯一部分是安全策略的一个关键组件。
