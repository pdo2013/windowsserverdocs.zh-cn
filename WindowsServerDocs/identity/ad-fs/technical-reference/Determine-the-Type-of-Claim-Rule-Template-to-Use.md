---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: 确定要使用的声明规则模板的类型
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1d38e23d7f1671f729b03b7e6f8000471d2e9f9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188650"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>确定要使用的声明规则模板的类型


设计 Active Directory 联合身份验证服务的重要组成部分\(AD FS\)基础结构确定完整的声明规则集 — 和的相应声明规则模板应使用来创建它们，每个伙伴将参与与你的组织的联合身份验证。 通过在 AD FS 管理管理单元中使用声明规则模板创建规则\-中。  
  
你配置的每组声明规则只能与一个联合信任相关联。 这意味着，你不能在一个信任上创建一组规则，但将其用于联合身份验证服务中的其他信任。 不过，你可以根据声明规则模板轻松地创建规则，从而有助于更快地生成一组所需的声明，并且这些声明得到每位联盟伙伴和组织的一致同意。  
  
有关规则和规则模板的详细信息，请参阅 [The Role of Claim Rules](The-Role-of-Claim-Rules.md)。  
  
在开始确定应使用的声明规则模板类型之前，请考虑以下问题：  
  
-   你的可信声明提供程序将提供哪些声明？  
  
-   你信任每个声明提供程序的哪些声明？  
  
-   信任此联合身份验证服务的信赖方需要哪些声明？  
  
-   你愿意向每个信赖方公开哪些声明？  
  
-   哪些用户将有权访问每个信赖方？  
  
回答这些问题将有助于制定一个可靠的声明规则设计。 此外，还有助于创建平稳的授权和访问控制策略，让你的部署团队在产品推出期间更高效。  
  
在下一部分中，你可以了解要根据业务需求为环境选择的规则模板类型。  
  
## <a name="claim-rule-template-types"></a>声明规则模板类型  
下表描述了所有类型的声明规则模板，可用于使用 AD FS 管理管理单元创建规则\-在中，并使用而不是另一种模板类型的好处。  
  
|规则模板类型|描述|优点|缺点|  
|----------------------|---------------|--------------|-----------------|  
|通过或筛选传入声明|用于创建一项规则，该规则将传递所选声明类型的所有声明值，或者根据声明值筛选声明，以便仅传递所选声明类型的某些声明值。<br /><br />有关详细信息，请参阅 [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)。|-可用于选择要在接受或发出的特定声明不变|的不能更改类型和值声明|  
|转换传入声明|用于创建一项规则，该规则可以选择一个传入声明并将其映射到不同的声明类型或将其声明值映射到新的声明值。<br /><br />有关详细信息，请参阅 [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md)。|-可以用于规范化声明类型或值<br />-可以替换电子\-传入声明的邮件后缀|-更复杂的字符串替换需要自定义规则|  
|以声明方式发送 LDAP 属性|用于创建一项规则，该规则将从 LDAP 属性存储中选择要以声明方式发送给信赖方的属性。<br /><br />有关详细信息，请参阅 [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)。|-可以从发出声明任何 AD DS\/AD LDS 属性存储<br />的可以使用单个规则发出多个声明|-性能 — 因帐户查找而速度缓慢<br />-不能使用自定义 LDAP 筛选器进行查询|  
|以声明方式发送组成员身份|用于创建一项规则，当用户是 Active Directory 安全组的成员时，该规则可以发送指定的声明类型和值。 根据所选择的组，将使用此规则仅发送一个声明。<br /><br />有关详细信息，请参阅 [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。|-发出组声明-无帐户查找快速性能|用户必须是本地 Active Directory 组的成员|  
|使用自定义规则发送声明|用于创建一项自定义规则，该规则将提供比标准规则模板更高级的选项。 您编写自定义规则与 AD FS 声明规则语言。<br /><br />有关详细信息，请参阅 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。|-可用于从 SQL 属性存储发出声明<br />-可以用于指定自定义 LDAP 筛选器<br />-可以用于发出 PPID<br />-可用于自定义属性存储<br />-可以使用仅向输入的声明集添加声明<br />-可以用于发送基于多个传入声明的声明|-更难配置\-开始接触声明规则语言可能需要一些技能强化时间|  
|根据传入声明允许或拒绝用户|用于创建创建一项规则，该规则将根据传入声明的类型和值，允许或拒绝用户访问信赖方。<br /><br />有关详细信息，请参阅 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-简化了授权过程|-要求只有一个声明类型，另一个声明值指定<br />-不支持声明值的模式匹配|  
|允许所有用户|用于创建一项规则，该规则将允许所有用户访问信赖方。<br /><br />有关详细信息，请参阅 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-简单配置|-比使用允许或拒绝用户基于传入声明模板不太安全，|  
  

