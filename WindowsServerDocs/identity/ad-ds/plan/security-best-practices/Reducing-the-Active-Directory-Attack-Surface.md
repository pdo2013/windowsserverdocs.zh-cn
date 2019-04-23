---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: 减少 Active Directory 攻击面
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874148"
---
# <a name="reducing-the-active-directory-attack-surface"></a>减少 Active Directory 攻击面

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本部分重点介绍技术控件，可实现以减少 Active Directory 安装的受攻击面。 部分包含以下信息：  
  
-   [实现最小特权的管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)标识使用高特权级别的风险主要适用于帐户的日常管理提供时，除了提供建议，以实现以降低风险该特权帐户存在。  
  
-   [实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)描述部署专用的安全管理系统，除了一些示例方法的安全管理主机部署的原则。  
  
-   [保护域控制器对攻击](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)讨论了策略和设置的尽管类似于实现的安全管理主机，建议包含某些域控制器特定于建议，以帮助确保域控制器和用来对其进行管理的系统足够安全。  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>权限的帐户和 Active Directory 中的组  
本部分提供有关权限的帐户的背景信息和 Active Directory 中的组旨在解释的通用性和 Active Directory 中的特权的帐户和组之间的差异。 通过了解这些区别，是否实现中的建议[实现最小特权的管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)原义或选择要为你的组织自定义它们，具有到所需的工具适当地保护每个组和帐户。  
  
### <a name="built-in-privileged-accounts-and-groups"></a>内置特权帐户和组  
Active Directory 简化委派管理和支持中分配权利和权限的最小特权原则。 在域中具有帐户的"常规"用户在默认情况下，能够读取大量的目录中存储的内容，但可以更改仅一组非常有限的目录中的数据。 需要额外权限的用户可以授予内置目录，以便它们可能执行与其角色相关的特定任务，但不能执行其职责与不相关的任务的各种"特权"组的成员身份。 组织还可以创建专门针对于特定的工作职责并被授予了具体的权限，允许 IT 人员，而无需授予超过什么的权限执行日常管理功能的权限的组需要这些函数。  
  
Active Directory 中三个内置组是在目录中的最高特权组：企业管理员、 域管理员和管理员。 以下各节中介绍的默认配置和功能的每个组：  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Active Directory 中的最高权限组  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) 是仅在林根域中存在一个组，默认情况下，它是在林中所有域管理员组的成员。 目录林根域中的内置管理员帐户是 EA 组的唯一默认成员。 EAs 被授予权利和权限，以便实现全林性更改 （即，更改会影响到林中所有域），如添加或删除域、 建立林信任或提升林功能级别。 在正确设计和实施委派模型中，EA 成员身份是所需，仅当首次构造该林或进行某些全林性更改，例如建立出站林信任。 可以将大多数权限和 EA 组授予权限委派给各个权限较低的用户和组。  
  
##### <a name="domain-admins"></a>Domain Admins  

在林中每个域都有其自己的域管理员组的成员和加入到域的每台计算机上的本地管理员组的成员的域管理员 (DA) 组。 DA 组为域的唯一默认成员是该域的内置管理员帐户。 DAs 是"功能全面"其领域内，而 EAs 具有全林性权限。 在正确设计和实施委派模型中，应仅在"不受限"（如需要具有高级别特权的域中的每台计算机上的帐户的情况下） 的情况下需要域管理员成员身份。 尽管本机 Active Directory 委派机制允许委派的范围内就可以仅在紧急情况下使用 DA 帐户，但构造有效的委派模型会耗费大量时间和很多组织利用要加快该过程的第三方工具。  
  
##### <a name="administrators"></a>Administrators  
第三个组为 DAs 和 EAs 嵌套到其中的内置域本地管理员 (BA) 组。 将授予此组的许多直接权限和权限的目录中和域控制器上。 但是，域管理员组有没有权限在成员服务器或工作站上。 它是通过授予本地特权的计算机的本地管理员组成员身份。  
  
> [!NOTE]  
> 尽管这些是这些特权组的默认配置，但任何三个组的成员可以操作的目录以获取任何其他组中的成员身份。 在某些情况下，很容易获取其他组的成员身份，而在其他人会更加困难，但从潜在的权限的角度来看，所有三个组应考虑实际上等同。  
  
##### <a name="schema-admins"></a>Schema Admins  

第四个特权组中，架构管理员 (SA)，仅在目录林根域中存在并具有仅该域的内置管理员帐户作为默认成员，类似于 Enterprise Admins 组。 旨在仅暂时并偶尔 （当需要时修改 AD DS 架构） 填充 Schema Admins 组。  
  
尽管 SA 组是唯一的组，可以修改 Active Directory 架构 （即，该目录的基础数据结构，如对象和属性），但 SA 组的权限和权限的范围是比前面所述的限制更多组。 它也很常见查找组织具有开发的 SA 组的成员身份管理的相应方案，因为通常很少需要组的成员身份，并且仅对一段较短的时间。 从技术上讲的是这样的 EA、 DA 和 BA 组在 Active Directory 中，但目前不太常见，若要查找的组织已实现的 SA 组与这些组的类似方案。  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>受保护的帐户和 Active Directory 中的组  
Active Directory 中一组默认的特权的帐户和组名为"受保护"的帐户和组保护方式不同于目录中的其他对象。 任何帐户 （而不考虑是否从安全或通讯组的组派生的成员身份） 的任何受保护组中拥有直接或可传递的成员身份继承此受限制的安全。  

  
例如，如果用户是分发组的成员，它是，反过来，在 Active Directory，该用户对象中的受保护组的成员将标记为受保护的帐户。 当帐户标记为受保护帐户时，在对象上 adminCount 属性的值设置为 1。  
  
> [!NOTE]
> 受保护组中的可传递成员身份包含嵌套的通讯组和嵌套的安全组，但嵌套的通讯组成员的帐户不会在其访问令牌中收到受保护的组的 SID。 但是，可以将分发组转换到 Active Directory，这正是分发组包含在受保护的组成员的枚举中的安全组。 以前分发组的成员都将随后收到父级的帐户应受保护的嵌套的分发组曾经将转换为安全组，保护其在下次登录时的访问令牌中组的 SID。  
  
下表列出了默认受保护帐户和由操作系统版本和 service pack 级别的 Active Directory 中的组。  
  
**受保护的帐户和按操作系统和 Service Pack (SP) 版本的 Active Directory 中组的默认值**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4 -Windows Server 2003**|**Windows Server 2003 SP1+**|**Windows Server 2008 -Windows Server 2012**|  
|Administrators|Account Operators|Account Operators|Account Operators|  
||管理员|管理员|管理员|  
||Administrators|Administrators|Administrators|  
|Domain Admins|Backup Operators|Backup Operators|Backup Operators|  
||Cert Publishers|||  
||Domain Admins|Domain Admins|Domain Admins|  
|Enterprise Admins|域控制器|域控制器|域控制器|  
||Enterprise Admins|Enterprise Admins|Enterprise Admins|  
||Krbtgt|Krbtgt|Krbtgt|  
||打印操作员|打印操作员|打印操作员|  
||||只读域控制器|  
||Replicator|Replicator|Replicator|  
|Schema Admins|Schema Admins||Schema Admins|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder 和 SDProp  
在每个 Active Directory 域的系统容器中，将自动创建一个名为 AdminSDHolder 对象。 AdminSDHolder 对象的用途是确保在受保护的帐户和用户组的权限一致地实施，而不考虑其中的受保护的组和帐户位于域中。  

（默认情况下，每隔 60 分钟保存域的 PDC 仿真器角色的域控制器上运行该过程称为安全描述符传播程序 (SDProp)。 SDProp 比较域的 AdminSDHolder 对象上的受保护的帐户和域中的组的权限的权限。 如果在任何受保护的帐户和组权限不匹配的 AdminSDHolder 对象上的权限，对受保护的帐户和组的权限会重置以匹配这些域的 AdminSDHolder 对象。  
  
禁用了权限继承对受保护的组和帐户，这意味着，即使帐户或组移到不同位置的目录中时，他们不继承权限从其新的父对象。 AdminSDHolder 对象上也被禁用继承，以便对父对象的权限更改不会更改 AdminSDHolder 的权限。  
  
> [!NOTE]
> 从受保护组删除帐户后，它不再被视为受保护的帐户，但其 adminCount 属性仍将设置为 1，如果未手动更改。 此配置的结果是，该对象的 Acl 将不再更新 SDProp，但对象仍未继承的权限从其父对象。 因此，该对象可以驻留在组织单位 (OU) 向其已委派权限，但以前受保护的对象将继承这些委派的权限。 若要查找和重置以前受保护的域中的对象的脚本可在[Microsoft 支持文章 817433](https://support.microsoft.com/?id=817433)。  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder 所有权  
由域的 BA 组拥有在 Active Directory 中的大多数对象。 但是，AdminSDHolder 对象，默认情况下，属于域的数据组。 （这是以下情况，在其中 DAs 不是派生其权限和通过域管理员组的成员身份的权限）。  
  
在版本的 Windows 早于 Windows Server 2008，对象的所有者可更改的对象，包括最初未不具有的权限授予其自身的权限。 因此，对域的 AdminSDHolder 对象的默认权限阻止 BA 或 EA 更改域的 AdminSDHolder 对象的权限的组成员的用户。 但是，域管理员组的成员可以取得对象的所有权并向自己授予其他权限，这意味着这种保护还处于初级阶段，并仅保护针对的用户是被意外修改对象不在域中的数据组的成员。 此外，BA 和 EA （如果适用） 组有权更改本地域 （对 EA 根域） 中的 AdminSDHolder 对象的属性。  
  
> [!NOTE]  
> AdminSDHolder 对象林，属性允许有限自定义 （删除） 将被视为受保护的组和 AdminSDHolder 和 SDProp 会影响的组。 此自定义应仔细考虑如果实现，尽管一些有效的林上 AdminSDHolder 修改可的情况。 可以在 Microsoft 支持文章中找到有关对林属性的修改 AdminSDHolder 对象上的详细信息[817433](https://support.microsoft.com/?id=817433)并[973840](https://support.microsoft.com/kb/973840)，然后在[附录 c:受保护帐户和 Active Directory 中的组](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
尽管 Active Directory 中的最高特权的组此处所述，有大量的其他组已被授予提升的特权级别。 有关的所有默认和 Active Directory 和分配给每个的用户权限中的内置组的详细信息，请参阅[附录 b:特权帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。  
  


