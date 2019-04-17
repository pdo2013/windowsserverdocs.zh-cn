---
ms.assetid: 7b22adfa-298d-413c-88d0-1231825b7d4d
title: "动态访问控制 Scenario 概述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f70bd138a16b23f1fbf7bfabc98ee184e3b9ab37
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="dynamic-access-control-scenario-overview"></a>动态访问控制：方案概述

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012、可以跨你文件服务器控制哪些人可以访问的信息和审核访问了信息应用数据管辖。 动态访问控制，可以：  
  
-   通过使用的文件的自动或手动分类识别数据。 例如，可能在组织中的文件服务器标记数据。  
  
-   通过将使用中心访问策略的安全网络策略应用来控制访问文件。 例如，你可能定义谁可以访问在组织内的健康信息。  
  
-   通过合规性报告和鉴定分析中心审核策略审核访问文件。 例如，你可以识别人访问过的高度敏感信息。  
  
-   通过自动 RMS 加密用于敏感 Microsoft Office 文档应用权限管理服务 (RMS) 保护。 例如，您可以配置 RMS 所有包含医疗保险并责任法案 (HIPAA) 的信息的文档进行加密。  
  
基于可以是常用的合作伙伴和业务线应用程序的基础结构投资动态访问控制功能集和功能可以为使用 Active Directory 的组织中提供出色的值。 此基础结构包括：  
  
-   Windows 可处理条件表情和中央策略新授权和审核引擎。  
  
-   用户索赔和设备索赔 Kerberos 身份验证支持。  
  
-   对文件分类基础结构 (FCI) 的改进。  
  
-   RMS 扩展性支持以便合作伙伴可以提供加密非 Microsoft 文件的解决方案。  
  
## <a name="in-this-scenario"></a>在此情况下  
以下方案和指南是设置的包含该内容:  
  
## <a name="BKMK_APP"></a>动态访问控制内容路线图  
  
|方案|评估|套餐|部署|运行|  
|------------|------------|--------|----------|-----------|  
|**方案：中心访问策略**<br /><br />对文件允许组织集中部署和管理授权策略，包括使用用户索赔、设备索赔和资源属性条件表情创建中心访问策略。 这些策略根据合规性和业务法规要求。 创建和托管 Active Directory，因此使其更易于管理和部署在这些策略。<br /><br />**森林跨部署索赔**<br /><br />在 Windows Server 2012、广告 DS 每个林和使用林中类型定义 Active Directory 森林级别的所有声明中维护索赔字典。 有很多情况下，可能需要遍历信任边界主体。 此项 scenario 描述索赔遍历信任边界的方式。|[动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)<br /><br />[森林跨部署索赔](Deploy-Claims-Across-Forests.md)|[计划：中心访问策略部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br /><br />-   [若要将映射到中心访问策略业务请求的进程](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_1)<br />-   [委派的动态访问控制管理](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.1)<br />-   [例外机制规划中心访问策略](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.2)<br /><br />使用用户索赔最佳做法<br /><br />-   [选择要启用索赔用户域中的正确配置](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DC_OP3)<br />-   [若要启用用户声明的操作](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.4.2)<br />-   [文件服务器在使用用户索赔注意事项自由 Acl，而无需使用中心访问策略](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_5)<br /><br />[使用设备的索赔和设备安全组](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DeviceClaims)<br /><br />-   [使用静态设备索赔注意事项](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.1)<br />-   [若要启用的设备索赔的操作](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.3)<br /><br />有关部署的工具<br /><br />-   [数据分类工具包](https://go.microsoft.com/fwlink/?LinkId=%20244300)|[部署中心访问策略 #40; 演示步骤 & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)<br /><br />[森林 #40; 演示步骤 & #41; 跨部署索赔](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)|-建模中心访问策略|  
|**方案：访问审核文件**<br /><br />安全审核是一款功能最强大的工具，以帮助维持企业级的安全性。 关键的安全审核的目标之一是合规性。 例如，如 Sarbanes-oxley、HIPAA 和支付卡 Industry (PCI) 行业标准要求遵守严格的一组规则与相关的数据安全和隐私的企业。 安全审核帮助建立是否此类策略。因此，这些证明与这些标准遵从。 此外，安全审核帮助检测到异常行为，识别，并减少安全策略中的缺陷，并通过创建用户活动的用于分析鉴定记录阻止可靠的行为。|[方案：访问审核文件](Scenario--File-Access-Auditing.md)|[文件计划访问审核](Plan-for-File-Access-Auditing.md)|[部署安全审核与中心审核策略 #40; 演示步骤 & #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)|-   [文件服务器的应用的中央访问策略监视器](https://technet.microsoft.com/library/jj574188.aspx)<br />-   [监视器关联的文件和文件夹的中央访问策略](https://technet.microsoft.com/library/jj574198.aspx)<br />-   [监视器上的文件和文件夹的资源属性](https://technet.microsoft.com/library/jj574208.aspx)<br />-   [监视器索赔类型](https://technet.microsoft.com/library/jj574086.aspx)<br />-   [在登录期间监视用户和设备索赔](https://technet.microsoft.com/library/jj574082.aspx)<br />-   [监视器中心访问策略和规则定义](https://technet.microsoft.com/library/jj574115.aspx)<br />-   [监视器资源特性定义](https://technet.microsoft.com/library/jj574155.aspx)<br />-   [监视的可移动存储设备使用](https://technet.microsoft.com/library/jj574128.aspx)。|  
|**方案：访问拒绝协助**<br /><br />今天，当用户尝试访问远程文件文件服务器上的，仅的迹象来指示它们不可能生长得是访问拒绝。 这将生成支持人员的请求，或需要确定该问题的 IT 管理员并且通常管理员已将很难来自用户获取合适的上下文这使得来解决问题。 <br />Windows Server 2012，在目标是尝试并帮助应对拒绝之前 IT 获取问题的访问权限的信息辅助和数据的业务所有者涉及以及何时 IT 获取参与，提供快速分辨率正确的所有信息。 在实现此目标挑战之一是，没有中心方法应对拒绝访问和每个应用程序与它不同优惠，从而在 Windows Server 2012、的目标之一是以改进 Windows 资源管理器拒绝访问体验。|[方案：访问拒绝协助](Scenario--Access-Denied-Assistance.md)|[套餐以访问拒绝获取帮助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)<br /><br />-   [确定访问拒绝协助模式](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.1)<br />-   [确定谁应处理访问请求](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_12)<br />-   [自定义访问拒绝协助消息](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_13)<br />-   [例外情况的套餐](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.4)<br />-   [确定如何访问拒绝协助部署](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.5)|[部署访问拒绝协助 #40; 演示步骤 & #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)||  
|**Office 文档的方案：基于分类的加密**<br /><br />保护敏感信息的主要是降低组织的风险。 各种法规，如 HIPAA 或支付卡 Industry 数据安全标准 (PCI-DSS) 口述加密的信息，并原因有很多业务加密业务敏感信息。 但是，加密信息较高，并且它可能会影响业务工作效率。 因此，企业倾向于不同的方法和加密其信息的优先级。 <br />为了支持此方案，Windows Server 2012 提供能够自动加密根据其分类的敏感 Windows Office 文件。 这是通过调用 Active Directory Rights Management 服务器 (广告 RMS) 保护的敏感文档几秒钟后，该文件被标识为敏感文件文件服务器上的文件管理任务。|[Office 文档的方案：基于分类的加密](Scenario--Classification-Based-Encryption-for-Office-Documents.md)|[套餐以进行分类基于的文档的加密部署](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)|[部署的 Office 文件 & #40 加密; 演示步骤 & #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)||  
|**通过使用分类的方案：获得深入地了解你的数据**<br /><br />依赖于数据和存储资源一直日益大多数组织的重要。 IT 管理员面临监督而且越来越复杂存储基础结构，同时还要使用负责确保总体拥有成本维护合理级别时日益的挑战。 管理存储资源不仅仅是关于可用性数据不再，但也有关公司策略，并了解如何使用存储启用高效利用率和合规性降低风险强制或音量。 通过自动分类进程，以便你可以更有效地管理你的数据，文件分类基础结构提供深入地了解你的数据。 以下分类方法是使用文件分类基础结构提供：手动、编程方式，并自动。 此项 scenario 侧重于自动文件分类方法。|[通过使用分类的方案：获得深入地了解你的数据](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)|[自动文件分类的套餐](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)|[部署自动文件分类 #40; 演示步骤 & #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)||  
|**文件服务器上的方案：实现保留的信息**<br /><br />保留期是的文档应该的时间保持之前它已过期。 根据组织机构保留期可能会有所不同。 你可以分类作为时遇到短、中等或长期保留期间文件夹中的文件，然后每个期间指定的时间范围。 你可能希望来将其法律按无限期保留文件。 <br />文件分类基础结构和文件 Server 资源管理器中使用文件管理任务和文件分类将应用保留期间的一组文件。 你可以将保留期间分配文件夹上，并将文件管理任务配置多长时间保留分配的时间是到最后一个。 在该文件夹中的文件即将到期时，该文件的所有者获取通知电子邮件。 你还可以将文件视为在法律按以便文件管理任务将不会过期该文件。|[文件服务器上的方案：实现保留的信息](Scenario--Implement-Retention-of-Information-on-File-Servers.md)|[文件服务器上的信息保留套餐](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)|[部署文件服务器 #40; 演示步骤 & #41; 上实现保留的信息](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)||  
  
> [!NOTE]  
> 动态访问控制 ReFS（复原文件系统）上不支持。  
  
## <a name="BKMK_LINKS"></a>请参阅  
  
|键入的内容|引用|  
|----------------|--------------|  
|**产品评估**|-   [动态访问控制审阅指南](https://go.microsoft.com/fwlink/?LinkId=244309)<br />-   [动态访问控制开发人员指南](https://go.microsoft.com/fwlink/?LinkId=245870)|  
|**计划**|-   [计划中心访问策略部署](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br />-   [文件计划访问审核](Plan-for-File-Access-Auditing.md)|  
|**部署**|-   [Active Directory 部署](https://go.microsoft.com/fwlink/p/?LinkID=238318)<br />-   [文件和存储服务部署。](https://go.microsoft.com/fwlink/?LinkID=24430)|  
|**操作**|[动态访问控制 PowerShell 参考](https://go.microsoft.com/fwlink/?LinkId=243150)|  
|**工具和设置**|[数据分类工具包](https://go.microsoft.com/fwlink/?LinkID=244300)|  
|**社区资源**|[目录服务论坛](https://social.technet.microsoft.com/Forums/winserverDS/threads)|  
  


