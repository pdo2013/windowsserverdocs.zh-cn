---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: 方案文件访问审核
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37a3b17360112d958b59a7e9c3f64aed5e6f6a5b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406994"
---
# <a name="scenario-file-access-auditing"></a>场景：文件访问审核

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安全审核是可帮助维护企业安全的最强大工具之一。 安全审核的主要目标之一就是监管合规。 如萨班斯法案、健康保险携带和健康法案 (HIPAA)、以及支付卡行业 (PCI) 之类的行业标准，要求企业遵循与数据安全和隐私相关的一组严格的规则。 安全审核帮助确立此类政策的存在状态，并提供符合这些标准的证明。 另外，安全审核通过创建一个可用于司法分析的用户行为痕迹，帮助检测异常行为、识别和缓解安全策略上的漏洞、并阻止不可靠行为。  
  
审核策略要求通常在以下几个级别加以推行：  
  
-   **信息安全。** 在取证分析和入侵检测过程中，通常使用文件访问审核跟踪。 如果组织能够获得与访问高价值信息相关的目标事件，则会极大改善他们的响应时间和调查准确性。  
  
-   **组织策略。** 例如，由 PCI 标准调控的组织可能会有一个中央策略，监视对所有标记为包含信用卡信息和个人身份信息 (PII) 的文件的访问。  
  
-   **部门策略。** 例如，财务部门可能要求只有财务部门成员才能修改某些财务文档（例如季度盈利报告），因此该部门需要监视对这些文档进行更改的所有其他尝试。  
  
-   **业务策略。** 例如，业务所有者想监视所有未授权的查看其项目数据的尝试。  
  
此外，合规性部门可能需要监视所有对中央授权策略和策略结构（如用户、计算机和资源属性）的更改。  
  
安全审核最重要的考虑因素之一是收集、存储和分析审核事件的消耗。 如果审核策略过于广泛，则收集到的审核事件数量就会上升，从而增加消耗。 如果审核策略过于狭窄，则会面临错过重要事件的风险。  
  
使用 Windows Server 2012，你可以通过使用声明和资源属性来编写审核策略。 这样，所编写的审核策略会更丰富、更有针对性和更易于管理。 这能够实现至今为止无法执行或难以执行的方案。 以下是管理员可以编写的审核策略示例：  
  
-   审核每个没有高安全性许可但试图访问 HBI 文档的人。 例如，Audit | Everyone | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High。  
  
-   审核所有试图访问与其正在参与的项目无关的文档的供应商。 例如，Audit | Everyone | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project。  
  
这些策略有助于控制审核事件的数量，并将它们仅限制为相关性最高的数据或用户。  
  
在管理员创建并应用审核策略之后，他们需要考虑的下一件事就是如何在收集到的审核事件中发现有用信息。 基于表达式的审核事件有助于减少审核的数量。 但是，用户需要通过某种方式来查询这些事件以获得有意义的信息并提出问题，例如 "谁在访问我的 HBI 数据？" 或 "是否有未经授权的权限来访问敏感数据？"  
  
 Windows Server 2012 增强了现有的数据访问事件，包括用户、计算机和资源声明。 这些事件是在每个服务器基础上生成的。 为了提供整个组织的事件的完整视图，Microsoft 正同合作伙伴一起提供事件收集和分析工具，如“系统中央操作管理器”中的“审核收集服务”。  
  
图 4 展示了中心审核策略的一个概观。  
  
![解决方案指南](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**图 4** 中心审核体验  
  
设置和使用安全审核通常包括如下一般性步骤：  
  
1.  标识出要监视的数据和用户的正确集合  
  
2.  创建和应用合适的审核策略  
  
3.  收集并分析审核事件  
  
4.  管理和监控创建的策略  
  
## <a name="in-this-scenario"></a>本方案内容  
下面的主题为此方案提供了更多指导：  
  
-   [文件访问审核计划](Plan-for-File-Access-Auditing.md)  
  
-   [利用中心审核策略&#40;部署安全审核演示步骤&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了作为本方案组成部分的角色和功能，并描述了它们如何为本方案提供支持。  
  
|角色/功能|如何支持本方案|  
|-----------------|---------------------------------|  
|“Active Directory 域服务”角色|Windows Server 2012 中的 AD DS 引入了基于声明的授权平台，该平台允许创建用户声明和设备声明、复合标识、（用户加设备的声明）、新的中心访问策略（CAP）模型和文件分类的使用授权决定的信息。|  
|“文件和存储服务”角色|Windows Server 2012 中的文件服务器提供了一个用户界面，管理员可以在其中查看用户对文件或文件夹的有效权限，并对访问问题进行故障排除，并根据需要授予访问权限。|  
  


