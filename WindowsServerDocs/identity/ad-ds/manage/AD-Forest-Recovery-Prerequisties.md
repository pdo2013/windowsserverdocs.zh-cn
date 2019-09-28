---
title: 规划 Active Directory 林恢复的先决条件
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 12c34afc497131bfe6abdc78c636e6d3b784877e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390366"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 林恢复先决条件

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

以下文档讨论了在设计林恢复计划或尝试恢复之前应熟悉的先决条件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南的假设

1. 你曾经使用过 Microsoft 支持部门专业人员和：
   - 确定林范围的故障原因。 本指南不建议故障原因或建议使用任何过程来防止失败。
   - 评估任何可能的补救措施。  
   - 结束时，请咨询 Microsoft 支持部门，将整个林还原到发生故障之前的状态，这是从故障中恢复的最佳方法。 在许多情况下，林恢复应为最后一个选项。

2. 已遵循有关使用 Active Directory 集成的域名系统（DNS）的 Microsoft 最佳实践建议。 具体而言，每个 Active Directory 域都应有一个 Active Directory 集成的 DNS 区域。 
   - 如果不是这种情况，你仍可以使用本指南的基本原则执行林恢复。 但是，你将需要根据自己的环境对 DNS 恢复采取特定的措施。 有关使用 Active Directory 集成的 DNS 的详细信息，请参阅[创建 Dns 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

3. 尽管本指南旨在作为林恢复的通用指南，但并不是所有可能的方案都涵盖在内。 例如，从 Windows Server 2008 开始，有一个服务器核心版本，它是 Windows Server 的完整版本，但没有完整的 GUI。 尽管当然可以恢复只包含运行服务器核心的 Dc 的林，但本指南没有详细说明。 但是，根据此处所述的指南，你将能够自行设计所需的命令行操作。  

> ![!NOTE]
> 尽管本指南的目的是恢复林并维护或还原完整的 DNS 功能，但恢复可能会导致在发生故障之前从配置中更改的 DNS 配置。 在恢复林后，可以恢复到原始 DNS 配置。 本指南中的建议不介绍如何配置 DNS 服务器以执行企业命名空间的其他部分的名称解析，其中存在未存储在 AD DS 中的 DNS 区域。  

## <a name="concepts-for-using-this-guide"></a>使用本指南的概念

在开始规划 Active Directory 林的恢复之前，应熟悉以下内容：  
  
- 基本 Active Directory 概念  
- 操作主机角色（也称为灵活单主机操作或 FSMO）的重要性。 这些角色包括：  
   - 架构主机
   - 域命名主机
   - 相对 ID （RID）主机
   - 主域控制器（PDC）仿真器主机
   - 基础结构主机

此外，应定期在实验室环境中备份和还原 AD DS 和 SYSVOL。 有关详细信息，请参阅[备份系统状态数据](AD-Forest-Recovery-Procedures.md)和[执行 Active Directory 域服务的非权威还原](AD-Forest-Recovery-Procedures.md)。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复 - 先决条件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复 - 过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复多域林中的单个域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-通过 Windows Server 2003 域控制器恢复林](AD-Forest-Recovery-Windows-Server-2003.md)  
