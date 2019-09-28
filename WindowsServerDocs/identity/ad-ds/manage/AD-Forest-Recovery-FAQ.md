---
title: AD 林恢复 - 常见问题解答
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 49cd12621c6ddf89393f0463e4856555ca241491
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369114"
---
# <a name="ad-forest-recovery---faq"></a>AD 林恢复 - 常见问题解答

>适用于：Windows Server 2016，Windows Server 2012 和 2012 R2，Windows Server 2008 和 2008 R2，Windows Server 2003

本文档包含有关林恢复的常见问题（Faq）：  

## <a name="general-recovery"></a>常规恢复

**:可以执行哪些操作来加速恢复？**

尽管恢复速度不是本指南的主要目标，但你可以通过以下方式实现较短的恢复时间：  
  
- 创建详细林恢复计划、定期更新，并在合理大小的模拟测试环境中将其在每年至少进行一次  
- 使用虚拟化域控制器（DC）克隆  
   - 在从每个域中的备份还原一个 DC 后，虚拟化的 DC 克隆可以加速获取其他 Dc 运行的过程。 可以克隆额外的虚拟化 Dc，而不是等待可能需要长 AD DS 安装完成，以及在安装之后完成非关键复制。  
   - 如果林中的虚拟 Dc 托管在数量相对较少的连接的数据中心，则在恢复过程中克隆可能会带来最大的好处。 但是，对于同一域中的多个虚拟化 Dc 的任何环境，都必须在同一个虚拟机监控程序主机上共存。  
- 部署只读域控制器（Rodc）  
   - Rodc 可以在恢复过程中提供业务连续性，因为它们不必与可写域控制器的网络断开连接。 Rodc 不执行出站复制。 因此，它们不会带来在将损坏的数据复制回已恢复环境时可写 Dc 所带来的风险。  
  
影响林恢复过程持续时间的其他因素包括：  
  
- 从备份还原 Dc 时，需要花费一段时间来执行以下操作：  
   - 找到物理备份介质，如磁带。  
   - 重新安装操作系统。  
   - 从备份媒体中还原数据。  
      - 可以通过执行完全服务器恢复（而不是系统状态还原）来减少重新安装操作系统所需的时间，并从备份还原数据。 由于完全服务器恢复基于二进制，因此完成的速度要快于系统状态还原。  
      - 但是，如果服务器包含不希望还原的系统状态数据中的数据，则完全服务器恢复可能不是系统状态还原的可行替代方法。 请考虑对服务器执行完整服务器恢复而不是系统状态还原的优点，并通过执行计划稍后要还原的适当类型的备份来进行相应的准备。  
- 重新生成 Dc 时，复制数据以进行基于网络的升级需要花费时间。  
   - 可以通过执行以下步骤来缩短还原 Dc 所需的时间：  
- 缩短检索备份介质的时间：  
   - 使用 Active Directory 数据库装载工具（Dsamain.exe）来确定要用于还原操作的最佳备份。 有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=132577)（ https://go.microsoft.com/fwlink/?LinkId=132577) 。  
   - 用一种方便、安全的位置，以一种方便、安全的位置将媒体标记为备份介质，并将其存储在方便的位置。  
   - 使用带有存储区域网络（SAN）的卷影复制服务维护不同时间点的备份。 有关详细信息，请参阅[Windows Server 2003 Active Directory 快速恢复卷影复制服务和虚拟磁盘服务](https://go.microsoft.com/fwlink/?LinkId=70781)（ https://go.microsoft.com/fwlink/?LinkId=70781) 。  
- 强制从 Dc 删除 AD DS，而不是重新安装操作系统。 如果在林范围内发生故障的原因已确定为完全位于 AD DS 范围内，则无需在 Dc 上重新安装操作系统。  
   - 有关强制从运行 Windows Server 2008 或更高版本的 DC 中删除 AD DS 的详细信息，请参阅[强制删除 Windows server 2008 域控制器](https://go.microsoft.com/fwlink/?LinkId=132627)（@no__t 为1。 有关强制从运行 Windows Server 2003 的 DC 中删除 AD DS 的详细信息，请参阅 Microsoft 知识库中的[文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780) （ https://go.microsoft.com/fwlink/?LinkId=70780) 。  
- 使用更快的磁带设备或磁盘备份来减少还原操作所需的时间。  
  
还可以使用 "从介质安装" （IFM）功能，在每个域中重新生成 Dc，从而帮助加速 AD DS 安装。 IFM 减少了在每个域中重建 Dc 时产生的复制延迟。  
  
具有更严格的服务级别协议（SLA）的企业可能会考虑改变林恢复过程，以加快恢复速度。  
  
**:能否自动执行林恢复过程？**

由于林恢复过程的复杂而关键，当前没有它的端到端自动化。 林恢复过程比处理自动化的技术问题更是一种后勤和组织的挑战。 因此，管理该环境的人员应创建一个特定于该环境的林恢复计划，然后自动执行可自动成功完成的部分。  
  
您可以使用命令行工具执行大多数林恢复步骤。 因此，大部分步骤都可以编写脚本。 例如，Ntdsutil.exe 是林恢复过程中最常使用的工具之一。  
  
尽管脚本可以加快恢复速度，但在实际环境中应用这些脚本之前，必须全面测试这些脚本。 此外，你必须根据 Active Directory 环境中的更改（例如添加新域或 DC）或 Active Directory 的新版本更新这些更改。

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
