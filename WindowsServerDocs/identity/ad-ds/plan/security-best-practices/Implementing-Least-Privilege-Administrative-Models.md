---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: 实现最小特权的管理模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 609ea0bd796a4d3696e14c7499be047ebdd83183
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868848"
---
# <a name="implementing-least-privilege-administrative-models"></a>实现最小特权的管理模型

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

以下摘录是在从[管理员帐户安全规划指南](https://technet.microsoft.com/library/cc162797.aspx)，1999 年 4 月 1 日第一次发布：

> "最与安全相关的培训课程和文档讨论了最小特权原则的实现，但组织很少会遵循它。 该原则非常简单，并将其应用正确极大的影响会增加您的安全性和降低风险。 原则指出，所有用户应具有完成当前任务而不安装其他所需的绝对最小权限的用户帐户以都登录。 这样做可以防止恶意代码及其他攻击。 此原则也适用于计算机和这些计算机的用户。   
> "这一原则运行情况非常好的原因之一是它可以促使您进行一些内部研究。 例如，必须确定计算机或用户真正需要访问权限，并对其进行实现。 对许多组织而言，此任务可能最初看起来好像大量工作;但是，它是成功保护您的网络环境的必要步骤。
> "您应授予的所有域管理员用户的最小权限概念域特权。 例如，如果管理员使用特权帐户登录，并会无意中运行病毒程序，病毒已到本地计算机和整个域的管理访问权限。 如果管理员已改为使用非特权 （非管理员） 帐户登录，损坏的病毒的作用域只能在本地计算机因为它会作为本地计算机用户运行。
> "在另一个示例中，向其授予域级别的管理员权限的帐户必须不具有提升的权限在另一个林中，即使在林之间没有信任关系。 这种做法有助于防止大范围的损害，如果攻击者设法侵入一个受管理的林。 组织应该定期审核来防止未经授权的特权提升其网络。"  

以下摘录是在从[Microsoft Windows 安全资源工具包](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749)，第一个发布，2005年:  

> "始终将安全方面授予最少执行的任务所需的特权。 如果具有太多的权限的应用程序应受到破坏，攻击者可能能够展开超出应用程序已在最少特权可能像攻击。 例如，检查明智地打开启动病毒的电子邮件附件的网络管理员的后果。 如果使用域管理员帐户登录管理员，病毒将域中的所有计算机上具有管理员权限，并因此不受限制地访问网络上的几乎所有数据。 如果管理员登录使用本地管理员帐户，病毒将本地计算机上具有管理员权限，因此，将能够访问的计算机上的任何数据并将密钥笔划日志记录软件等恶意软件安装在在计算机中。 如果管理员登录使用普通用户帐户，病毒会只对管理员的数据的访问，并将不能安装恶意软件。 通过使用读取电子邮件，在此示例中，所需的最低权限潜在危害的范围大大降低。"  

## <a name="the-privilege-problem"></a>权限问题

在前面的摘录内容中所介绍的原则未进行更改，但在评估 Active Directory 安装，我们总是发现过多的已被授予远远超出所需的权限来执行日常的帐户工作。 环境的大小会影响原始数字的过度特权帐户，但不是属于的 proportionmidsized 目录中可以具有数十个帐户最高特权组中，而大型安装可能有成百上千。 几个例外情况之外，而不考虑完善了攻击者的技能和集中，攻击者通常遵循最轻松的路径。 仅是否以及何时更简单的机制出现故障或被阻止防守，它们会增加其工具和方法的复杂性。  

遗憾的是，在许多环境中的最轻松的路径已被证明是过度使用具有各种广泛而深入的特权的帐户。 广泛的权限是权限，并且允许跨大型横跨多个环境-例如执行特定活动的帐户的权限，技术支持人员可能会被授予权限，允许他们重置多个用户帐户的密码。  

深度权限应用于总体的窄段的强大权限，此类授予一名工程师管理员权限的服务器上，以便他们可以执行修复。 广泛特权和深度权限都不一定是危险的但当域中的多个帐户永久授予各种广泛而深入的特权，如果只有一个帐户被泄露，它可以快速可用来重新配置到环境攻击者的目的或甚至要销毁大段的基础结构。  

传递哈希攻击，这是一种类型的凭据窃取攻击，无处，因为可用于执行这些免费可用且易于使用并且许多环境中很容易受到攻击。 但是，传递哈希攻击中，不是真正的问题。 此问题的关键是双重的：  

1. 它是攻击者可以获得一台计算机上的深度特权，然后传播到其他计算机广泛该特权通常很容易。  
2. 在整个计算环境通常有太多永久高级别特权的帐户。

即使将消除传递哈希攻击，攻击者只需将使用不同策略，不不同的策略。 而不是植入包含工具的凭据被盗的恶意软件，也可能计划在日志中记录的击键，恶意软件，或利用任意数量的其他方法来捕获整个环境功能非常强大的凭据。 策略，无论目标保持不变： 具有各种广泛而深入的特权的帐户。  

额外特权授予不是仅发现在 Active Directory 中受破坏环境中。 如果组织已开发的权限授予更多的权限比需要的习惯，它通常位于整个基础结构，如以下各节中所述。  

## <a name="in-active-directory"></a>在 Active Directory 中

在 Active Directory 中很常见，若要查找的 EA、 DA 和 BA 组包含过多的帐户。 大多数情况下，组织的 EA 组包含最少的成员、 DA 组通常包含 EA 组中的用户数的比例和管理员组通常包含多个成员比其他组结合使用的填充。 这通常是由于管理员是以某种方式相信"更少特权"比 DAs 或 EAs。 尽管每个组授予的权限不同，但它们应被有效地视为同样强大组因为集合的成员可以进行自我的另外两个成员。  

## <a name="on-member-servers"></a>在成员服务器上

当我们检索许多环境中的成员服务器上的本地管理员组的成员身份时，我们发现范围从少量的本地和域帐户到数十个嵌套组的成员身份，展开时，显示成百，甚至成千的在服务器上，使用本地管理员权限的帐户。 在许多情况下，具有较大的成员身份的域组嵌套在成员服务器的本地管理员组，而无需考虑到这一事实的任何用户都可以修改这些域中的组的成员身份可以获得的所有系统的管理控制在其上具有本地管理员组中已嵌套组。  

## <a name="on-workstations"></a>在工作站上

尽管工作站通常具有明显较少的成员在其本地管理员组中比成员服务器，在许多环境中，将授予用户在其个人计算机上的本地管理员组的成员身份。 如果发生这种情况，即使启用了 UAC，这些用户就会风险到他们的工作站的完整性。  

> [!IMPORTANT]  
> 应仔细考虑是否用户需要在其工作站上的管理权限，如果是这样，更好的方法可能是管理员组的成员的计算机上创建单独的本地帐户。 如果用户需要提升，他们可能就会以进行提升，该本地帐户的凭据，但该帐户是本地的因为它不能用于危及其他计算机或访问域资源。 与任何本地帐户，但是，本地特权帐户的凭据应该是唯一的;如果多个工作站上的相同凭据创建一个本地帐户，则公开传递哈希攻击的计算机。  

## <a name="in-applications"></a>在应用程序中

在其中目标是组织的知识产权攻击中，可以针对已被授予应用程序中功能强大的特权的帐户以允许数据泄露。 尽管有权访问敏感数据的帐户可能已被授予在域或操作系统没有提升的权限，但提供了可以操作的应用程序或对信息应用程序的访问配置的帐户存在风险。  

## <a name="in-data-repositories"></a>数据存储库中

与其他目标情况一样，攻击者试图访问知识产权的文档和其他文件的形式可以以帐户，控制对该文件的访问将存储，具有直接访问文件，或甚至组或具有角色的帐户访问文件。 例如，如果文件服务器用于存储的合同文档和文档通过使用 Active Directory 组授予访问权限，攻击者可以修改组的成员身份的用户可以将遭到入侵的帐户添加到组和访问协定文档数。 在对文档的访问由 SharePoint 等应用程序的情况下，攻击者可以指定目标应用程序，如前面所述。  

## <a name="reducing-privilege"></a>减少权限

较大，就越复杂环境中，更难以管理和保护。 在小型组织中，查看并降低特权可能相对较简单的主张，但每个额外的服务器、 工作站、 用户帐户和应用程序中使用的组织中添加另一个对象必须受到保护。 因为很难或甚至无法正确地保护组织的每个方面 IT 基础结构，应其权限创建的最大的风险，这通常是内置的特权的帐户的帐户首先重点工作和Active Directory 和特权的工作站和成员服务器上的本地帐户中的组。  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>确保在工作站和成员服务器上的本地管理员帐户的安全

尽管本文档重点介绍保护 Active Directory，因为以前讨论过，大多数攻击针对目录开始，针对各个主机的攻击。 不能提供保护成员的系统上的本地组的完整指导原则，但可以使用以下建议来帮助你保护工作站和成员服务器上的本地管理员帐户。  

#### <a name="securing-local-administrator-accounts"></a>确保本地管理员帐户的安全

在所有版本的当前处于主流支持的 Windows，默认情况下，这使得该帐户无法用于传递哈希和其他凭据盗窃攻击禁用本地管理员帐户。 但是，在含有旧操作系统的域或已启用的本地管理员帐户，这些帐户可以使用如前面所述在成员服务器和工作站之间传播泄漏。 出于此原因，所有已加入域的系统上的本地管理员帐户的建议以下控件。  

中提供了用于实现这些控件的详细的说明[附录 h:保护本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)。 在实施之前这些设置，但是，确保，本地管理员帐户当前未使用的环境中的计算机上运行服务或执行其他活动为其不应使用这些帐户。 在生产环境中实施建议之前进行全面测试这些设置。  

#### <a name="controls-for-local-administrator-accounts"></a>控件的本地管理员帐户

内置管理员帐户应该永远不会用作成员服务器上的服务帐户，也不应使用它们登录到本地计算机上 （在安全模式下，即使禁用了帐户允许的除外）。 实现此处所述的设置的目标是为防止每台计算机的本地管理员帐户进行可用，除非首先撤消保护控件。 通过实现这些控件并监视管理员帐户的更改，你可以大幅降低成功攻击的可能性该目标的本地管理员帐户。  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>配置已加入域的系统上的管理员帐户进行限制的 Gpo

在创建并链接到 Ou 中的每个域的工作站和成员服务器的一个或多个 Gpo，将管理员帐户添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \ Policies\用户权限分配**:  

- 拒绝从网络访问这台计算机
- 拒绝作为批处理作业登录
- 拒绝以服务身份登录
- 拒绝通过远程桌面服务登录

当管理员帐户添加到这些用户权限时，指定是否要添加本地管理员帐户或域的管理员帐户的方式标记该帐户。 例如，若要添加到这些 NWTRADERS 域管理员帐户拒绝权限，你需要键入与帐户**NWTRADERS\Administrator**，或浏览到 NWTRADERS 域的管理员帐户。 若要确保限制的本地管理员帐户，请键入**管理员**在这些用户权限设置在组策略对象编辑器。  

> [!NOTE]  
> 即使本地管理员帐户被重命名，仍将应用策略。  

这些设置将确保的计算机的管理员帐户不能用于连接到其他计算机，即使无意或恶意启用它。 不能完全禁用本地登录使用本地管理员帐户，也不应尝试执行此操作，因为计算机的本地管理员帐户设计用于灾难恢复方案。  

应成员服务器或工作站成为脱离域使用任何其他本地帐户授予管理权限、 可以进入安全模式启动计算机，可以启用管理员帐户，和可以然后效果中使用的帐户在计算机上的修复。 完成修复后，应再次禁用管理员帐户。  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>保护本地特权的帐户和 Active Directory 中的组

*法律六个数字：计算机安全管理员值得信任。* - [十个永恒定律的安全 （版本 2.0）](https://technet.microsoft.com/security/hh278941.aspx)  

此处提供的信息旨在提供有关保护最高权限的内置帐户和 Active Directory 中的组的一般指导原则。 中也提供了详细的分步说明[附录 d:确保在 Active Directory 中的内置 Administrator 帐户的安全](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)，[附录 e:保护 Active Directory 中的 Enterprise Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)，[附录 f:保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)，然后在[附录 g:保护 Active Directory 中的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)。  

在实现任何这些设置之前，还应测试全面以确定它们是否适合您环境的所有设置。 并非所有组织将都能够实现这些设置。  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>确保在 Active Directory 中的内置 Administrator 帐户的安全

在 Active Directory 中的每个域，管理员帐户被创建为域创建的过程。 此帐户是默认情况下的域管理员和域中的管理员组的成员，并且如果域是目录林根域，帐户也是 Enterprise Admins 组的成员。 应仅对初始生成活动和灾难恢复方案，可能被保留域的本地管理员帐户的使用。 若要确保内置管理员帐户可用于影响修复，可以使用任何其他帐户，不应更改默认成员身份的林中任何域中的管理员帐户。 相反，您应按照指南来帮助保护在林中每个域中的管理员帐户。 中提供了用于实现这些控件的详细的说明[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

#### <a name="controls-for-built-in-administrator-accounts"></a>内置管理员帐户的控件

实现此处所述的设置的目标是成为可用的除非多个控件都将被恢复防止每个域的管理员帐户 （而不是组）。 通过实施这些控件和监视更改的管理员帐户，可以大大减少了成功攻击的可能性，通过利用域的管理员帐户。 在您的林中每个域中的管理员帐户，应该配置下列设置。  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>启用该帐户中的"敏感帐户，不能被委派"标志

默认情况下，可以委派 Active Directory 中的所有帐户。 委派允许计算机或服务提供的帐户具有对计算机进行身份验证的凭据或到其他计算机以获取代表的帐户服务的服务。 如果你启用**敏感帐户，不能被委派**属性上的基于域的帐户，该帐户的凭据不能提供给其他计算机或服务在网络上，这可以限制攻击利用若要使用该帐户的凭据在其他系统上的委派。  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>启用该帐户中的"智能卡是交互式登录必须"标志

如果你启用**智能卡是交互式登录必须**属性上的帐户，Windows 将该帐户的密码重置为 120 个字符的随机值。 通过内置管理员帐户上设置此标志，可确保帐户的密码不只长而复杂，但不是被透露给任何用户。 不启用此属性之前, 创建的帐户智能卡所需从技术上讲，但如果可能，应在配置的帐户限制之前每个管理员帐户创建智能卡和智能卡应存储在安全位置。  

虽然设置**智能卡是交互式登录必须**标志将重置帐户的密码，不会阻止具有权限重置为已知值设置帐户和使用的帐户的密码的用户帐户的名称和新密码来访问网络上的资源。 因此，应实现该帐户上的以下其他控件。  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>配置已加入域的系统上的域的管理员帐户进行限制的 Gpo

虽然禁用域中的管理员帐户将使无法有效地使用该帐户，应在帐户上实现其他限制，如果启用了帐户无意或恶意如此。 尽管最终可以由管理员帐户撤消这些控件，目标是创建降低攻击者的进度的控件，并限制损害该帐户可以对其造成。  

在创建并链接到 Ou 中的每个域的工作站和成员服务器的一个或多个 Gpo，将每个域的管理员帐户添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \策略 \ 用户权限分配**:  

- 拒绝从网络访问这台计算机  
- 拒绝作为批处理作业登录  
- 拒绝以服务身份登录  
- 拒绝通过远程桌面服务登录  

> [!NOTE]  
> 当你将本地管理员帐户添加到此设置时，必须指定配置本地管理员帐户或域管理员帐户。 例如，若要添加到这些 NWTRADERS 域的本地管理员帐户拒绝权限，您必须键入与帐户**NWTRADERS\Administrator**，或浏览到 NWTRADERS 域的本地管理员帐户。 如果键入**管理员**在这些用户权限设置组策略对象编辑器中，您将限制将 GPO 应用于每台计算机上的本地管理员帐户。  
>
> 我们建议将作为基于域的管理员帐户相同的方式限制成员服务器和工作站上的本地管理员帐户。 因此，您应通常添加在林中每个域的管理员帐户和本地计算机的管理员帐户对这些用户权限设置。 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>配置 Gpo 以限制域控制器上的管理员帐户

在林中每个域中，默认域控制器策略或策略链接到域控制器 OU 应修改每个域的管理员帐户添加到中的以下用户权限**计算机配置Windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配**:  

- 拒绝从网络访问这台计算机  
- 拒绝作为批处理作业登录  
- 拒绝以服务身份登录  
- 拒绝通过远程桌面服务登录  

> [!NOTE]  
> 这些设置将确保，本地管理员帐户不能用于连接到域控制器，尽管该帐户，如果启用，可以在本地登录到域控制器。 因为此帐户应仅启用和使用在灾难恢复方案中，我们已预见到的物理访问至少一个域控制器将在可用，也可以是其他帐户有权远程访问域控制器使用。  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>配置的内置 Administrator 帐户审核

已保护的每个域的管理员帐户并将其禁用，应配置审核，以对帐户的更改的监视器。 如果启用了帐户、 重置其密码，或任何其他修改都会对该帐户，则警报应发送到用户或团队负责管理 AD DS 中，除了在组织中的事件响应团队。  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>保护管理员、 Domain Admins 和 Enterprise Admins 组

#### <a name="securing-enterprise-admin-groups"></a>保护企业管理员组

Enterprise Admins 组中，该表存放在目录林根域中，应包含在日常工作中，使用的域的本地管理员帐户，可能的异常上没有用户提供保护，如前所述，在[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

需要 EA 访问时，应暂时将其帐户需要 EA 权利和权限的用户放入 Enterprise Admins 组。 尽管用户使用高特权的帐户，应审核和最好是通过一个用户执行所做的更改和观察所做的更改的另一个用户尽量减少无意滥用或配置错误的可能性来执行他们的活动. 已完成的活动，应从 EA 组删除帐户。 这可以通过手动过程，来实现，并记录进程、 第三方特权的标识/访问管理 (PIM/PAM) 软件或这两者的组合。 指南为创建帐户可用于控制在 Active Directory 中的特权组的成员身份中提供了[的帐户凭据被盗](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)中提供了详细的说明和[附录 i:创建管理帐户的受保护帐户和组在 Active Directory 中](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

默认情况下，每个林中的域中内置的 Administrators 组的成员是 Enterprise Admins。 从每个域中的管理员组中删除 Enterprise Admins 组是不适当的修改，因为在林灾难恢复情形中，将可能需要 EA 权限。 如果在林中的管理员组中删除 Enterprise Admins 组，它应添加到每个域中的管理员组，应实现以下附加控制：  

- 如前文所述，Enterprise Admins 组应包含在日常工作中，与可能的异常应如中所述的受保护的目录林根域管理员帐户的任何用户[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
- 在链接到包含成员服务器和工作站中的每个域的 Ou 的 Gpo，EA 组应添加到以下用户权限：  
   - 拒绝从网络访问这台计算机  
   - 拒绝作为批处理作业登录  
   - 拒绝以服务身份登录  
   - 拒绝本地登录  
   - 通过远程桌面服务拒绝登录。  

这将防止 EA 组的成员登录到成员服务器和工作站。 如果跳转服务器用于管理域控制器和 Active Directory，请确保跳转服务器位于限制 Gpo 未链接到 OU 中。  

- 应配置审核来发送警报，如果对属性或 EA 组的成员身份进行任何修改。 这些警报应发送，至少对用户或团队负责 Active Directory 管理和事件响应。 您还应定义用于暂时填充 EA 包括的组，通知过程时执行的组的合法填充过程和步骤。  
  
#### <a name="securing-domain-admins-groups"></a>保护域 Admins 组

Enterprise Admins 组的情况一样，应仅在生成或灾难恢复方案中需要 Domain Admins 组成员身份。 应该有没有日常用户帐户在域中的本地管理员帐户除了 DA 组中如果保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
需要 DA 访问时，应暂时将需要此级别的访问权限的帐户放在问题域的数据组。 尽管用户使用高特权的帐户，应审核，最好是使用一个用户执行所做的更改并观察所做的更改的另一个用户最大程度减少无意滥用或配置错误的可能性执行活动。 已完成的活动，应从域管理员组删除帐户。 这可以通过手动过程，来实现，并记录过程，通过第三方特权的标识/访问管理 (PIM/PAM) 软件或这两者的组合。 指南为创建帐户可用于控制在 Active Directory 中的特权组的成员身份中提供了[附录 i:创建管理帐户的受保护帐户和组在 Active Directory 中](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
域管理员的是，默认情况下，所有成员服务器和在其各自域的工作站上本地 Administrators 组的成员。 因为它会影响可支持性和灾难恢复选项，则不应修改此默认嵌套。 如果已从成员服务器上的本地管理员组中删除域管理员组，它们应添加到每个成员服务器和链接的 Gpo 中的受限制的组设置通过在域中的工作站上的管理员组中。 以下常规控件，深入介绍[附录 f:保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)还应实现。  

对于每个林中的域中 Domain Admins 组：  

1. 从具有在域中的内置管理员帐户的可能异常的 DA 组中删除所有成员提供保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
2. 在链接到包含成员服务器和工作站中的每个域的 Ou 的 Gpo，应将 DA 组添加到以下用户权限：  
   - 拒绝从网络访问这台计算机  
   - 拒绝作为批处理作业登录  
   - 拒绝以服务身份登录  
   - 拒绝本地登录  
   - 拒绝通过远程桌面服务登录  
  
   这将防止 DA 组的成员登录到成员服务器和工作站。 如果跳转服务器用于管理域控制器和 Active Directory，请确保跳转服务器位于限制 Gpo 未链接到 OU 中。  

3. 应配置审核来发送警报，如果对属性或 DA 组的成员身份进行任何修改。 这些警报应发送，至少对用户或团队负责 AD DS 管理和事件响应。 您还应定义用于暂时填充 DA 包括的组，通知过程时执行的组的合法填充过程和步骤。  

#### <a name="securing-administrators-groups-in-active-directory"></a>保护 Active Directory 中的管理员组

与 EA 和 DA 组情况一样，应仅在生成或灾难恢复方案中需要管理员 (BA) 组成员身份。 应该有没有日常用户帐户除了在域中的本地管理员帐户的管理员组中如果保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

需要管理员访问权限时，应暂时将需要此级别的访问权限的帐户放在问题域的管理员组。 尽管用户使用高特权的帐户，但活动应审核和，最好是执行与执行所做的更改的用户和观察所做的更改的另一个用户，以最大程度减少无意滥用或配置错误的可能性。 当活动完成后时，应立即从 Administrators 组删除帐户。 这可以通过手动过程，来实现，并记录过程，通过第三方特权的标识/访问管理 (PIM/PAM) 软件或这两者的组合。  

管理员是，默认情况下，大部分在其各自的域中的 AD DS 对象的所有者。 可能的所有权或取得对象的所有权的功能是必需的生成和灾难恢复方案中需要此组中的成员身份。 此外，DAs 和 EAs 继承其权限和权限按其默认成员身份的管理员组中的数字。 默认组嵌套的不应修改 Active Directory 中的特权的组，并应保护每个域的管理员组，如中所述[附录 g:保护 Active Directory 中的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)，并在下面的常规说明。  

1. 从具有在域中的本地管理员帐户的可能异常的管理员组中删除所有成员提供保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
2. 域的管理员组的成员应永远不需要登录到成员服务器或工作站。 中一个或多个 Gpo 链接到 Ou 中的每个域的工作站和成员服务器，应将管理员组添加到以下用户权限：  
   - 拒绝从网络访问这台计算机  
   - 拒绝作为批处理作业登录  
   - 拒绝以服务身份登录  
   - 这将防止管理员组的成员用于登录或连接到成员服务器或工作站 （除非首先破坏多个控件），其中他们的凭据无法进行缓存，从而破坏。 永远不应使用特权的帐户登录到较低权限系统，并强制实施这些控制提供了对多种攻击防护。  

3. 在域控制器 OU 的林，管理员组中每个域中应被授予以下用户权限 （如果他们还没有这些权限），从而允许执行所需的功能的管理员组的成员林范围的灾难恢复方案：  
   - 从网络访问此计算机  
   - 允许本地登录  
   - 允许通过远程桌面服务登录  

4. 应配置审核来发送警报，如果对属性或 Administrators 组的成员身份进行任何修改。 这些警报应发送，至少到负责 AD DS 管理团队的成员。 警报还应将发送到安全团队的成员和过程应定义用于修改管理员组的成员身份。 具体而言，这些进程应包括依据安全团队收到通知，当管理员组将要修改，以便发送警报，它们应该和不会触发警报时的过程。 此外，应实现进程 Administrators 组的使用已完成并且使用的帐户已从组中删除时通知安全小组。  

> [!NOTE]  
> 在 Gpo 中的管理员组实现限制时，Windows 将设置应用于计算机的本地 Administrators 组的域管理员组的成员。 因此，在实现上的 Administrators 组的限制时应小心。 尽管 Administrators 组的成员禁止网络、 批处理和服务的登录建议是可行来实现的任何位置，但不限制本地登录或通过远程桌面服务登录。 阻止这些登录类型可能会阻止合法管理的计算机的本地管理员组的成员。 下面的屏幕截图显示了配置设置的阻止不恰当使用内置的本地和域管理员帐户，除了不恰当使用的内置本地或域管理员组。 请注意，**拒绝通过远程桌面服务登录**用户权限不包括 Administrators 组中，因为其包括在此设置还会阻止这些成员的本地计算机的帐户登录管理员组。 如果计算机上的服务配置为在本部分中所述的特权任何的组上下文中运行，实现这些设置会导致服务和应用程序失败。 因此，与所有的此部分中的建议，一样，应全面测试适用性的设置您的环境中。  
>
> ![最低权限管理模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>Active directory 基于角色的访问控制 (RBAC)

通常情况下，基于角色的访问控制 (RBAC) 是一种机制对用户进行分组，并提供对根据业务规则的资源的访问。 对于 Active Directory 中，为 AD DS 中实现 RBAC 是创建到的权利和权限委派以允许角色的成员，而无需额外特权授予其执行日常管理任务的角色的过程。 RBAC 的 Active Directory 进行设计和实现通过本机工具和接口，通过利用的软件您可能已经拥有，通过购买第三方产品或这些方法的任意组合。 本部分并不提供对于 Active Directory，实施 RBAC 的分步说明，但改为讨论中选择您的 AD DS 安装中实现 RBAC 方法要考虑的因素。  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>Active Directory 的 RBAC 的本机方法

在最简单的 RBAC 实现中，可以实现与 AD DS 组的角色和委派权限和使其能够执行的角色的指定作用域内的日常管理的组的权限。  

在某些情况下，可以使用 Active Directory 中的现有安全组来授予权限的适当工作职能。 例如，如果特定员工在 IT 组织中的负责管理和维护的 DNS 区域和记录，委派这些职责可以是简单的每个 DNS 管理员创建帐户并将其添加到 DNSActive Directory 中的管理员组。 DNS 管理员组中的，与更高特权组不同 Active Directory 中具有几个功能强大的权限，尽管此组的成员已被授予的权限，允许它们进行管理的 DNS 是仍受泄漏和滥用行为可能会导致特权提升。

在其他情况下，可能需要创建安全组以及委派执行指定的管理任务的权限和对 Active Directory 对象、 文件系统对象和注册表对象的权限以允许组的成员。 例如，如果您的技术支持操作人员负责重置忘记的密码，协助用户连接问题和故障排除应用程序设置，您可能需要组合在 Active Directory 中用户对象上的委派设置使用允许技术支持用户远程连接到用户的计算机可以查看或修改用户的配置设置的权限。 对于你定义的每个角色，您应确定：  

1. 对日常和不太频繁地执行哪些任务执行任务的角色成员。  
2. 在哪些系统上和在哪些应用程序中的权利和权限应授予角色的成员。  
3. 哪些用户应被授予角色的成员身份。  
4. 如何将执行的角色成员身份管理。  

在许多环境中手动创建基于角色的访问控制来管理 Active Directory 环境可能很难实施和维护。 如果您已清楚地定义角色和职责的 IT 基础结构的管理，你可能想要利用其他工具，以帮助你创建可管理的本机 RBAC 部署。 例如，如果在你的环境中使用 Forefront Identity Manager (FIM)，可以使用 FIM 来自动创建和管理角色，可以轻松地进行中的管理的填充。 如果你使用 System Center Configuration Manager (SCCM) 和 System Center Operations Manager (SCOM)，可以使用特定于应用程序的角色来委托管理和监视功能，并还可以强制实施一致的配置和系统中的审核在域中。 如果已实现公钥基础结构 (PKI)，可以发出，并且需要智能卡进行 IT 人员负责管理环境。 使用 FIM 凭据管理 (FIM CM)，你甚至可以合并角色和凭据的管理为您的管理人员。  

在其他情况下，它可能更适合组织需要考虑部署提供了"框中"功能的第三方 RBAC 软件。 RBAC 的 Active Directory、 Windows，和非 Windows 目录和操作系统的现成的、 商业 (COTS) 解决方案提供的许多供应商。 当本机解决方案和第三方产品之间进行选择，应考虑以下因素：  

1. 预算：通过投资开发使用软件和工具，您可能已经拥有的 RBAC，可以减少部署解决方案时所涉及的软件成本。 但是，除非您有人员在创建和部署本机 RBAC 解决方案方面具有丰富经验，您可能需要吸引咨询资源来开发解决方案。 特别是如果你的预算有限，应仔细权衡自定义开发解决方案的成本来部署"框中"解决方案，预计的开销。  
2. IT 环境的组合：如果你的环境主要组成 Windows 系统，或已使用的非 Windows 系统和帐户管理 Active Directory，自定义本机解决方案可能会为需求提供最佳解决方案。 如果您的基础结构包含多个未运行 Windows 且不受 Active Directory 的系统，可能需要考虑的独立于 Active Directory 环境的非 Windows 系统的管理选项。  
3. 在解决方案中的特权模型：如果产品依赖于其服务帐户放置在 Active Directory 中的高特权组，并且不会向 RBAC 软件授予不需要过多的特权的产品/服务选项，您有不真正减少 Active Directory 攻击图面上你仅已更改的目录中的最高特权组的组合。 除非应用程序供应商可以提供控件的服务帐户的帐户被入侵和恶意使用的可能性降到最低，您可能要考虑其他选项。  

### <a name="privileged-identity-management"></a>特权标识管理

特权标识管理 (PIM)，有时称为特权的帐户管理 (PAM) 或特权的凭据管理 (PCM) 是一种设计，构造，并实现的方法来管理特权帐户在你基础结构。 通常情况下，PIM 提供了帐户授予临时权限所依据的机制，并执行生成或中断所需的权限修复功能，而不是离开永久附加到帐户的权限。 PIM 功能手动创建还是通过实现一个或多个以下功能的第三方软件的部署可能会提供：  
  
- 凭据"保管库，"是"签出"并分配了初始密码，然后"签入"时已完成的活动，此时密码将再次重置帐户的特权帐户的密码。  
- 使用时间限制的限制的特权凭据  
- 一个一次性使用的凭据  
- 工作流生成授予的权限具有监视和活动均完成或分配时间时执行的活动和自动删除权限的报告已过期  
- 替换硬编码的凭据，如用户名和密码与应用程序编程接口 (Api) 的脚本中，可让用于从保管库根据需要检索凭据  
- 自动管理的服务帐户凭据  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>创建无特权的帐户管理权限的帐户

在管理特权的帐户的挑战之一是，默认情况下，可以管理特权和受保护的帐户的帐户和拥有的特权组和受保护的帐户。 如果你的 Active Directory 安装实现相应的 RBAC 和 PIM 解决方案，解决方案可能包括可用于有效地 depopulate 填充组仅在目录中，最高特权组的成员身份的方法暂时并在需要时。  

如果你实现本机 RBAC 和 PIM，但是，应考虑在需要时的 Active Directory 中创建帐户没有权限且使用的填充和 depopulating 特权的唯一函数进行分组。 [附录 i:为受保护的帐户和组在 Active Directory 中的创建管理帐户](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)提供可用来实现此目的创建帐户的分步说明。  

### <a name="implementing-robust-authentication-controls"></a>实施可靠的身份验证控制

*法律六个数字：确实有有人有尝试猜测密码。* - [十个永恒定律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  

传递哈希和其他凭据被盗攻击不是特定于 Windows 操作系统，也不新。 第一次传递哈希攻击已创建于 1997 年。 从历史上看，但是，这些攻击所需自定义的工具，也在其成功后，像撞大而且所需攻击者具有相对较高程度的技能。 引入了本机中提取凭据的免费、 易于使用的工具导致在最近几年中的数量和凭据被盗攻击成功呈指数增加。 但是，凭据被盗攻击不是凭据的目标和入侵的唯一机制。  

虽然应实施控制来帮助防止凭据被盗攻击，还应确定最有可能会成为攻击者的目标环境中的帐户和对于那些实现可靠的身份验证控件帐户。 如果你拥有最高特权的帐户使用的单因素身份验证，如用户名和密码 （两者都"知道的内容，"这是一个身份验证因素），这些帐户受到弱保护。 所有这些攻击者需要是知识的用户名称和密码与帐户关联的知识并且不需要传递哈希攻击，攻击者可以以接受单个身份的凭据的任何系统对用户进行身份验证。  

尽管实现多重身份验证但不能防止您传递哈希攻击中，实现多重身份验证与受保护的系统可以结合使用。 中提供了有关实施受保护的系统的详细信息[实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)，和以下各节中讨论了身份验证选项。  

#### <a name="general-authentication-controls"></a>控制常规身份验证

如果你有未实现多重身份验证，如智能卡，请考虑执行此操作。 智能卡在阻止用户的私钥受访问和使用，除非该用户出示正确的 PIN、 密码或生物特征识别码到智能卡公钥-私钥对中实现硬件强制执行保护的私钥。 即使的受损计算机上，攻击者可以重复使用的 PIN 或密码，击键记录程序截获用户的 PIN 或密码还必须以物理方式存在卡。

在其中长而复杂的密码已被证明难以由于用户抵制而实现的情况下，在智能卡提供一种机制，通过该用户可能会实现相对简单的 Pin 或密码且让凭据不易受到暴力破解或彩虹表攻击。 智能卡 Pin 不存储在 Active Directory 中或在本地 SAM 数据库中，尽管凭据哈希可能仍存储在其已使用身份验证的智能卡的计算机上的 LSASS 保护内存中。  

#### <a name="additional-controls-for-vip-accounts"></a>有关 VIP 帐户的其他控件

实现智能卡或其他基于证书的身份验证机制的另一个好处是能够利用身份验证机制保证来保护敏感数据的 VIP 用户都可访问。 身份验证机制保证仅在域功能级别设置为 Windows Server 2012 或 Windows Server 2008 R2 中可用。 启用后，身份验证机制保证将在具有管理员指定全局组成员资格用户的凭据进行身份验证期间使用基于证书的登录方法登录时添加到用户的 Kerberos 令牌。  

这使资源管理员控制访问资源，例如文件、 文件夹和打印机，根据用户是否登录上使用基于证书的登录方法，除了使用证书的类型。 例如，当用户使用智能卡登录，对网络上的资源的用户的访问可以指定为不同从何种访问权限时，用户不使用智能卡 （即，用户在登录时通过输入用户名和密码）。 有关身份验证机制保证的详细信息，请参阅[适用于 Windows Server 2008 R2 循序渐进指南中的 AD DS 身份验证机制保证](https://technet.microsoft.com/library/dd378897.aspx)。  

#### <a name="configuring-privileged-account-authentication"></a>配置权限的帐户身份验证

在所有管理帐户的 Active Directory 中启用**交互式登录需要智能卡**特性，以及审核的更改 （至少），在属性中的任意**帐户**选项卡上的帐户 （例如，cn、 名称、 sAMAccountName、 userPrincipalName 和 userAccountControl） 管理用户对象。  

虽然设置**交互式登录需要智能卡**帐户将重置帐户的密码为 120 个字符的随机值，需要智能卡的交互式登录时，仍属性可被覆盖具有允许对其进行更改帐户和帐户的密码的权限的用户然后用于建立与仅用户名和密码的非交互式登录。  

在其他情况下，根据的配置中的帐户在 Active Directory 证书服务 (AD CS) 或第三方 PKI、 用户主体名称 (UPN) 的 Active Directory 和证书设置的属性管理或 VIP 帐户可以为目标为特定类型的攻击，如下所述。  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>UPN 劫持证书 spoofing （欺骗）

虽然超出了本文档的范围是针对公共基础结构 (Pki) 的攻击的深入讨论，但已自 2008年呈指数级增加了对公钥和私钥的 Pki 攻击。 公共 Pki 漏洞已广泛地公开度，但对组织的内部 PKI 攻击很可能是更多见。 一个此类攻击利用 Active Directory 和证书以允许攻击者欺骗的方式可能很难检测到其他帐户的凭据。  

当对已加入域的系统进行身份验证提供证书时，使用者或证书中的使用者可选名称 (SAN) 属性的内容用于将证书映射到 Active Directory 中的用户对象。 具体取决于证书和构造方式的类型，在证书中的使用者属性通常包含用户的公用名 (CN)，如以下屏幕截图中所示。  

![最低权限管理模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

默认情况下，Active Directory 将通过连接帐户的第一个名称来构造用户的 CN +""+ 最后一个名称。 但是，在 Active Directory 中用户对象的 CN 组件不是要求或不能保证是唯一的并移动到其他位置的目录中的用户帐户更改帐户的可分辨的名称 (DN)，这是中的对象的完整路径目录，如上面的屏幕截图的底部窗格中所示。  

证书使用者名称不能保证是静态的或唯一，因为使用者备用名称的内容通常用于在 Active Directory 中查找用户对象。 从企业证书颁发机构颁发给用户 （Active Directory 集成的 Ca） 的证书的 SAN 属性通常包含用户的 UPN 或电子邮件地址。 Upn 保证在 AD DS 林是唯一的因为查找按 UPN 的用户对象通常作为执行身份验证，使用或不使用证书身份验证过程中涉及的一部分。  

在身份验证证书中的 SAN 属性中的 upn，则使用便可供攻击者能够获取欺骗性证书。 如果攻击者已侵害能够读取和写入 upn，则在用户对象上的帐户，攻击的实现方式如下：  

（例如 VIP 用户） 的用户对象上的 UPN 属性暂时更改为不同的值。 SAM 帐户名属性和 CN 可还在更改这一次，尽管这通常是不必要的前面所述的原因。  

更改目标帐户的 UPN 属性时过时，已启用用户帐户或刚刚创建的用户帐户的 UPN 属性更改为最初分配给目标帐户的值。 过时，已启用用户帐户是时间的没有登录较长时间，但未禁用的帐户。 它们的目标的攻击者想要"隐藏在显而易见的"由于以下原因：  

1. 因为该帐户已启用，但最近未被使用，使用该帐户是不太可能触发警报的方法，启用已禁用的用户帐户可能。  
2. 使用现有的帐户不需要可能注意到的管理人员的新用户帐户创建。  
3. 仍处于启用状态的过时的用户帐户通常是不同的安全组的成员，并被授予了对在网络上，从而简化了访问权限和"混合"的现有用户填充的资源的访问。  

现在已在其配置目标 UPN 的用户帐户用于从 Active Directory 证书服务请求一个或多个证书。  

当攻击者的帐户获得证书时，"new"的帐户和目标帐户 Upn 将返回为其原始值。  

攻击者现在具有一个或多个可以提供对资源和应用程序进行身份验证，好像该用户是暂时修改其帐户的 VIP 用户的证书。 尽管所有可以由攻击者在其中针对证书和 PKI 的方法的完整讨论超出了本文档的范围，提供此攻击机制以说明为什么应监视特权和 VIP 帐户在 AD DS 中的更改特别是对于对任何属性上的更改**帐户**帐户 （例如，cn、 名称、 sAMAccountName、 userPrincipalName 和 userAccountControl） 的选项卡。 除了监视帐户，应该限制谁可以修改为尽可能小的帐户的一组作为可能的管理用户。 同样，管理用户的帐户应受保护和监视的未经授权的更改。  
