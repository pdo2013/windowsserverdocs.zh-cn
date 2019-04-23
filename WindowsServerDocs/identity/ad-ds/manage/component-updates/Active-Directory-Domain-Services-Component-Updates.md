---
title: Active Directory 域服务组件更新
description: 本文档讨论为 Windows Server 2012 R2 的 AD DS 组件更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 300bf7dafe0ef0ede754302f2c0aa8c36a7adbfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889878"
---
# <a name="active-directory-domain-services-component-updates"></a>Active Directory 域服务组件更新

>适用于：Windows Server 2016, Windows Server 2012 R2

本模块介绍在“目录服务和标识”空间中收到较少更新的组件。  
  
|关于作者|  
|--------------------|  
|**作者：**|Justin Turner|  
|**个人简历：**|Justin 是总部在美国德克萨斯州欧文市的目录服务团队的高级支持呈报工程师。  他在过去 12 年中已创建或参与编写了 Microsoft 知识库的许多培训课程和知识库文章。 他主要讲授 Microsoft 员工和客户新产品体系结构，是特许的 Microsoft 认证大师 (MCM) Microsoft 认证培训师 (MCT) 和保存硕士 在计算机教育及认知系统的程度。|  
|**供稿人**|本培训模块包括 *Michiko Short*、*Dean Wells*、*Alan Jowett*、*Manu Pushpendran*、*Yashar Bahman*、*Anoosh Saboori*、*Rashmi Jha*、*Justin Hall* 和 *Herbert Mauerer* 提供的稿件|  
|**审阅者**|非常感谢下列人员花费自己的时间审阅和提供反馈：*Joey Seifert*、*Justin Hall*|  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
### <a name="what-you-will-learn"></a>你将学习的内容  
完成本模块后，你将能够：  
  
-   说明 Windows Server 2012 R2 在目录服务和标识技术方面所做的组件更新  
  
    -   [SPN 和 UPN 唯一性](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Winlogon 自动重新启动登录&#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [TPM 密钥证明](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [CA 备份和还原 Windows PowerShell cmdlet](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [命令行进程审核](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [凭据保护和管理](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [目录服务组件更新](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [域和林功能级别](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [弃用 NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [LDAP 查询优化程序更改](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [1644 事件改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Active Directory 复制吞吐量改进](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


