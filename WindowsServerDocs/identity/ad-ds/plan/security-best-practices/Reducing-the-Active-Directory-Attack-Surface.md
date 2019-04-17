---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: "减少 Active Directory 攻击 Surface"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>减少 Active Directory 攻击 Surface

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本部分着重于 technical 控件，以实现减少攻击的 Active Directory 安装。 部分包含以下信息：  
  
-   [实现最低权限管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)重点日常管理高权限帐户的用于展示了，除了提供建议，以实现以减少特权帐户存在的风险的风险。  
  
-   [实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)有关部署的专用、安全管理系统，除了一些示例方法安全管理主机部署描述原则。  
  
-   [保护域控制器针对攻击](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)讨论策略和设置，尽管类似于实现安全管理主机的推荐包含一些域控制器特定建议，以帮助确保充分的保护，域控制器和用于管理这些系统。  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>特权的帐户和 Active Directory 中的组  
此部分中提供了有关特权帐户的背景信息，组中的 Active Directory 适合说明共性和 Active Directory 中特权的帐户和组之间的区别。 通过了解这些区别，是否实现中的建议[实现最低权限管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)逐字或选择要自定义这些针对你的组织，有的工具，你需要确保每个组中的安全和相应地帐户。  
  
### <a name="built-in-privileged-accounts-and-groups"></a>内置特权帐户和组  
Active Directory 促进管理委派，并支持最低权限原则中指定的权利和的权限。 "常规"用户帐户域中有是，默认情况下，无法读取大部分目录中存储的内容，但无法更改仅非常有限的目录中的数据集。 需要额外的权限的用户可以授予内置目录，以便他们可以执行与他们的角色相关的特定的任务，但无法执行的任务不是为其关税相关的各种"权限"组中的成员。 组织也可以创建针对特定的职责并获得精细的权利和允许执行日常管理功能，而无需授予权限和超过了有关这些功能所需的权限 IT 人员的权限的组。  
  
三种内置组内的 Active Directory，是目录中的最高权限组：企业管理员、域管理和管理员。 以下部分介绍默认配置和功能的每个组：  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>中 Active Directory 的最高权限组  
  
##### <a name="enterprise-admins"></a>企业管理员  
企业管理员 (EA) 是一组，仅存在于森林根域中，并且默认情况下，它是管理员组森林中的所有域中的成员。 森林根域中内置的管理员帐户是唯一默认的 EA 组成员。 EAs 被授予权限，使它们在更改生效树林（即影响更改森林中的所有域），如添加或删除域、建立森林信任，或者提高森林功能级别。 在正常设计和实施委派模式中，EA 会员，才能仅当首次连建造森林或进行某些树林更改，例如建立站森林信任时。 大部分的权利和授予 EA 组的权限可以委派给较小特权用户和组中。  
  
##### <a name="domain-admins"></a>域管理员  

森林中的每个域都有它自己的域的管理员组成员且组成员的本地管理员已加入域的每台计算机上的域管理员 (DA) 组。 唯一的域的 DA 组默认成员是该域内置的管理员帐户。 虽然 EAs 树林特权，DAs 是"功能全面"在他们的域。 中正确设计和实现委派模型，域管理员会员的所需应仅在"中断玻璃"的情况（例如某些情况下，在其中所需帐户高级别权限的每台计算机在域中）。 尽管本机 Active Directory 委派机制允许的范围内就可能仅在紧急情况下使用 DA 帐户的委派，构建高效委派型号可能会很耗时，和许多组织利用第三方工具，以加快此过程。  
  
##### <a name="administrators"></a>管理员  
第三个组是内置的域本地管理员（栏）组 DAs 和 EAs 嵌套到其中。 此组授予许多直接的权利和目录中和域控制器上的权限。 但是，域该组有没有权限或工作站成员服务器上。 它是通过授予权限本地计算机的本地管理员组中的成员。  
  
> [!NOTE]  
> 尽管这些这些特权组中的默认配置的三个组中的任何成员可操作的目录，以获得任何其他组中的成员。 在某些情况下，它能够轻而易举其他一个组中的会员而在其他人，它是更加困难，但从潜在特权的角度而言，这三组应该考虑相当有效。  
  
##### <a name="schema-admins"></a>方案管理员  

第四个特权组中，方案管理员（索托），存在仅在森林根域中，并且作为默认成员，类似于企业管理员组已只有该域内置管理员帐户。 架构管理员组旨在填充仅暂时和有时（当修改的广告 DS 方案，则需要）。  
  
虽然索托组可以修改 Active Directory 模式（如对象和特性的目录基本数据结构该是）仅组，但索托组的权利和权限范围是更加受限比前面所述的组。 这也很常见查找因为不频繁通常需要组中的成员，并且仅对短期内，组织了开发相应实践索托组的会员管理。 EA、DA 和栏中的组 Active Directory，以及，从技术上讲如此，但不太常见，查找组织已实现类似实践至于索托组这些组。  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>受保护的帐户和 Active Directory 中的组  
在 Active Directory 中，一组默认特权的帐户和组的名为"受保护的"帐户，并不同目录中的其他对象安全的组。 已经直接或传递会员（无论是否会员得出安全或 distribution 组）的任何保护组中的任何帐户继承此受限的安全。  

  
例如，如果用户是 distribution 组成员的就是、依次的 Active Directory 中，该用户对象受保护的组成员标记为受保护的帐户。 当某个帐户被标记为受保护的帐户时，对象上 adminCount 属性的价值是设为 1。  
  
> [!NOTE]
> 尽管传递受保护的组成员包括嵌套的 distribution 和嵌套的安全组，嵌套的 distribution 组成员的帐户不会在其访问令牌收到 SID 受保护的组。 但是，distribution 组可以转换为安全的 Active Directory，这正是受保护的组成员枚举包含 distribution 组中的组。 应受保护的嵌套的分发组有史以来转换为安全组，是前者 distribution 组成员随后将收到家长帐户保护中其访问令牌在下次登录的组 SID。  
  
下表列出的受保护的默认帐户和 Active Directory 的操作系统版本和 service pack 级别中的组。  
  
**默认保护帐户和 Active Directory 由操作系统和 Service Pack (SP) 版本中的组**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008-Windows Server 2012**|  
|管理员|帐户运营商|帐户运营商|帐户运营商|  
||管理员|管理员|管理员|  
||管理员|管理员|管理员|  
|域管理员|备份运营商|备份运营商|备份运营商|  
||证书发布者|||  
||域管理员|域管理员|域管理员|  
|企业管理员|域控制器|域控制器|域控制器|  
||企业管理员|企业管理员|企业管理员|  
||Krbtgt|Krbtgt|Krbtgt|  
||打印运营商|打印运营商|打印运营商|  
||||仅阅读域控制器|  
||复制程序|复制程序|复制程序|  
|方案管理员|方案管理员||方案管理员|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder 和 SDProp  
在每个 Active Directory 的域的系统容器，会自动创建称为 AdminSDHolder 的对象。 AdminSDHolder 对象的目的是确保您的权限保护的帐户和组始终强制，无论是在域中处的受保护的组和帐户的位置。  

（默认）每隔 60 分钟拥有的域 PDC 模拟器角色域控制器上的此过程称为安全描述符传播程序 (SDProp) 运行。 SDProp 比较上的受保护的帐户和域中的组权限的域 AdminSDHolder 对象的权限。 如果不匹配的受保护的帐户和组任何权限 AdminSDHolder 对象的权限，受保护的帐户和组权限被重置匹配的域的 AdminSDHolder 对象。  
  
禁用了权限继承受保护的组和帐户，这意味着，即使帐户或组移到的目录中的其他位置时，它们执行操作不文件沿用文件权限从其新家长对象。 以便权限的更改为家长对象不要更改的 AdminSDHolder 权限 AdminSDHolder 对象还禁用继承。  
  
> [!NOTE]
> 从受保护的组删除某个帐户后，不会再视为受保护的帐户，但如果未手动更改设置为 1 其 adminCount 特性保持。 此配置结果是由 SDProp，不会再更新的对象 Acl，但对象仍然不文件沿用文件权限其父对象。 因此，对象可能位于部门 (OU) 的已委派权限，但以前受保护的对象文件将无法沿用文件这些委派的权限。 可以在找到的脚本重置保护以前对象域中的，找到[Microsoft 支持文章 817433](https://support.microsoft.com/?id=817433)。  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder 拥有  
大多数 Active Directory 对象归的域栏组。 但是，AdminSDHolder 对象，默认情况下，由拥有的域 DA 组。 （这是情况下，在其中 DAs 不要派生其权限和通过管理员组域中的会员的权限）。  
  
在 Windows 以前的版本的 Windows Server 2008、处为物体的所有者可以更改权限的对象，包括授予自行他们最初没有的权限。 因此，在某个域 AdminSDHolder 对象的默认权限阻止栏或 EA 更改域 AdminSDHolder 对象的权限的组成员的用户。 但是，域管理员组成员可以拍摄对象的所有权和授予自行额外的权限，这意味着此保护非常重要，并且仅保护避免意外修改的不是在域中 DA 组成员的用户的对象。 此外，栏并 EA（如果适用）组有权更改地域（根域 EA）中 AdminSDHolder 对象属性。  
  
> [!NOTE]  
> 对 AdminSDHolder 的对象林，属性允许有限自定义（移除）算是受保护的组并受 AdminSDHolder 和 SDProp 组。 此自定义应该仔细考虑如果实现，尽管有有效的情况下在 AdminSDHolder 林修改可。 可以在 Microsoft 支持文章中找到上 AdminSDHolder 对象修改林特性的详细信息[817433](https://support.microsoft.com/?id=817433)和[973840](https://support.microsoft.com/kb/973840)，并在[附录 c：保护帐户和 Active Directory 中的组](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
尽管中 Active Directory 的最特权的组此处所述，有大量的其他拥有的组提升了权限的级别。 有关所有内置组中的 Active Directory 和分配给每个用户权利以及默认值的详细信息，请参阅[附录 b：特权帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。  
  


