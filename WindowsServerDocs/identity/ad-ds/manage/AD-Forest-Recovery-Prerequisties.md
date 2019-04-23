---
title: 规划 Active Directory 林恢复的先决条件
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842168"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 林恢复系统必备组件

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

以下文档将讨论您应该熟悉制定林恢复计划或正在尝试恢复之前的先决条件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南的假设

1. 您使用过 Microsoft 支持专业人员和：
   - 确定全林性失败的原因。 本指南不建议失败的原因，也不建议以防止失败的任何过程。
   - 评估任何可能的补救措施。  
   - 得出的结论是，在咨询 Microsoft 技术支持，该还原整个林到其状态故障发生之前是从失败中恢复的最佳方式。 在许多情况下，林恢复应该是最后一个选项。

2. 遵循 Microsoft 最佳实践建议使用 Active Directory 集成域名系统 (DNS)。 具体而言，应为每个 Active Directory 域的 Active Directory 集成 DNS 区域。 
   - 如果这不是这种情况，您仍可以使用本指南的基本原理执行林恢复。 但是，你将需要根据自己的环境的 DNS 恢复特定度量值。 有关使用 Active Directory 集成 DNS 的详细信息，请参阅[创建 DNS 基础结构设计](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

3. 尽管本指南旨在作为林恢复的一般指南，涵盖了并不是所有可能的方案。 例如，从 Windows Server 2008 开始，没有为完整版的 Windows Server 但不完全 GUI 的服务器核心版本。 尽管肯定可以恢复包含只是运行服务器核心的 Dc 的林中，但本指南有任何详细的说明。 但是，根据此处所述的指导你将能够自己设计所需的命令行操作。  

> ！[!NOTE]
> 尽管本指南的目标是要恢复林和维护或还原完整的 DNS 功能，恢复可能导致从在出现故障之前的配置更改的 DNS 配置。 林恢复之后，您可以恢复到原始的 DNS 配置。 本指南中的建议不介绍如何配置 DNS 服务器，以执行企业的命名空间的其他部分的名称解析的 DNS 区域未存储在 AD DS 中。  

## <a name="concepts-for-using-this-guide"></a>使用本指南的概念

在开始规划 Active Directory 林恢复之前，您应熟悉以下：  
  
- Active Directory 的基本概念  
- 操作主机角色 （也称为灵活单主机操作或 FSMO） 的重要性。 这些角色包括：  
   - 架构主机
   - 域命名主机
   - 相对 ID (RID) 主机
   - 主域控制器 (PDC) 仿真器主机
   - 基础结构主机

此外，您应备份和还原 AD DS 和 SYSVOL，在定期的实验室环境中。 有关详细信息，请参阅[备份系统状态数据](AD-Forest-Recovery-Procedures.md)并[执行 Active Directory 域服务非权威还原](AD-Forest-Recovery-Procedures.md)。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复的系统必备组件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 林恢复-设计出一个自定义林恢复计划](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 林恢复-识别问题](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 林恢复-确定如何恢复](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 林恢复-执行初始恢复](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
- [AD 林恢复-方面的常见问题](AD-Forest-Recovery-FAQ.md)  
- [AD 林恢复-恢复单个的多域林中域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 林恢复-与 Windows Server 2003 域控制器的林恢复](AD-Forest-Recovery-Windows-Server-2003.md)  
