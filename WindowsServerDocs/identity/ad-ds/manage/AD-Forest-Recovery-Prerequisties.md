---
title: "先决条件规划 Active Directory 森林恢复"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 森林恢复先决条件

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

下面的文档讨论你应在寻找森林恢复套餐，或在尝试恢复前熟悉的先决条件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南假设 

1.  Microsoft 支持专业人员使用过和：

    - 确定树林失败的原因。 本指南不建议失败的原因，也不建议任何过程，以防止失败。  
    - 评估任何可能补救方法。  
    - 总结的那样，在 Microsoft 支持咨询该还原整个森林到状态之前中出现故障是恢复从故障的最佳方式。 在许多情况下，森林恢复应该是最后一个选项。  </br></br>

2. 按照 Microsoft 最佳建议，使用 Active Directory – 集成域名系统 (DNS)。 具体来说，都应该有 Active Directory – 集成的 DNS 为每个 Active Directory 的域的区域。 如果这不是这种情况，你仍然可以使用本指南中的基本原则执行森林恢复。 但是，你将需要采取具体措施 DNS 恢复根据自己的环境。 有关使用 Active Directory 的详细信息-集成的 DNS，请参阅[创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。
3. 本指南专为森林恢复一般指南，尽管涵盖并非所有可能的情况。 例如，Windows Server 2008 从开始，没有服务器核心版，这是完整版的 Windows Server，但不完整 GUI。 尽管绝对可以恢复树林包含只运行服务器 Core 的域控制器，本指南有任何详细的说明进行操作。 但是，根据此处讨论指导你将能够自己设计所需的命令行操作。  
 
>![!NOTE]
> 本指南中的目标是恢复森林和维护或还原完整 DNS 功能，尽管恢复可能会导致从之前故障配置更改 DNS 配置。 恢复森林之后，你可以恢复为原始 DNS 配置。 本指南中的建议不介绍如何配置 DNS 服务器执行名称分辨率的公司命名空间的其他部分有不会存储广告 DS 中的 DNS 区域。  

## <a name="concepts-for-using-this-guide"></a>使用本指南概念
 在开始规划的 Active Directory 森林恢复之前，你应该熟悉以下：  
  
-   基本概念，Active Directory  
  
-   操作主机角色（也称为灵活一个主机操作或 FSMO）的重要性。 这些角色如下所示：  
  
    -   方案母版  
  
    -   域名主机  
  
    -   相对 ID (RID) 母版  
  
    -   主域控制器 (PDC) 模拟器主机  
  
    -   基础结构母版  
  
 此外，你应该拥有备份和还原定期实验环境中的广告 DS 和 SYSVOL。 有关详细信息，请参阅[数据备份系统状态](AD-Forest-Recovery-Procedures.md)和[执行权威的 Active Directory 域服务还原](AD-Forest-Recovery-Procedures.md)。

## <a name="next-steps"></a>后续步骤
-   [广告森林恢复-先决条件](AD-Forest-Recovery-Prerequisties.md)  
-   [广告森林恢复-在寻找自定义森林恢复套餐](AD-Forest-Recovery-Devising-a-Plan.md)  
- [广告森林恢复-找出问题](AD-Forest-Recovery-Identify-the-Problem.md)
-   [广告森林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [广告森林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  
-   [广告森林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
-   [广告森林恢复-恢复单个域中多域森林](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [广告森林恢复-森林恢复与 Windows Server 2003 该域控制器](AD-Forest-Recovery-Windows-Server-2003.md)  
