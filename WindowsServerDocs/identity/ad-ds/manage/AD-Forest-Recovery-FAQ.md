---
title: AD 林恢复 - 常见问题解答
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878448"
---
# <a name="ad-forest-recovery---faq"></a>AD 林恢复 - 常见问题解答

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2、 Windows Server 2003

本文档包含常见问题 (Faq) 就林恢复：  

## <a name="general-recovery"></a>常规恢复

**问：加快恢复速度可以做什么？**

虽然恢复速度快不是本指南的主要目标，可以实现更短恢复的时间：  
  
- 创建详细的林恢复计划上定期, 更新和练习它在模拟的测试环境中的合理的大小至少一次一年  
- 使用虚拟化的域控制器 (DC) 克隆  
   - 虚拟化 DC 克隆加快了过程即可获取 DC 从每个域中的备份中还原后运行的其他 Dc。 其他虚拟化的 Dc 可以克隆，而不是等待可能耗时较长的 AD DS 安装完成并完成安装后的非关键复制。  
   - 林虚拟 Dc 的托管位置中相对较少的连接良好的数据中心可能会从获益最多克隆在恢复过程。 但是，其中为同一域的多个虚拟化的 Dc 共同位于同一虚拟机监控程序主机任何环境应受益。  
- 部署只读域控制器 (Rodc)  
   - Rodc 可以在恢复过程中提供业务连续性，因为它们不需要可写 Dc 一样从网络断开连接。 Rodc 不执行出站复制。 因此，它们不显示同一个可写 Dc 会将具有破坏性的数据复制回已恢复环境带来的风险。  
  
其他会影响林恢复过程的持续时间的因素包括：  
  
- 当从备份中还原 Dc 时，花时间：  
   - 找到物理备份介质，例如磁带。  
   - 重新安装操作系统。  
   - 从备份媒体还原数据。  
      - 可以减少所需重新安装操作系统，并通过执行整个服务器恢复，而不是系统状态还原从备份还原数据的时间。 因为整个服务器恢复是基于二进制的它完成比系统状态还原快得多。  
      - 但是，如果服务器包含不想还原的系统状态数据从排除的数据，整个服务器恢复可能不是系统状态还原到一个可行的替代。 具体而言，为您的服务器执行整个服务器恢复而不是系统状态还原的优点，请考虑并相应地准备通过执行适当的打算以后还原的备份类型。  
- 重新生成的 Dc，花时间复制数据，对于基于网络的升级。  
   - 可以减少通过执行以下步骤来还原域控制器所需要的时间：  
- 减少检索的备份介质的时间：  
   - 使用 Active Directory 数据库装载工具 (Dsamain.exe) 来标识要用于还原操作的最佳备份。 有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=132577)(https://go.microsoft.com/fwlink/?LinkId=132577)。  
   - 清楚地标记备份介质和将该介质存放在一种方便、 有条理的方式尚未保护，允许快速检索的位置。  
   - 使用与存储区域网络 (SAN) 卷影复制服务来从不同的点的备份保留的时间。 有关详细信息，请参阅[Windows Server 2003 Active Directory 快速恢复使用卷影复制服务和虚拟磁盘服务](https://go.microsoft.com/fwlink/?LinkId=70781)(https://go.microsoft.com/fwlink/?LinkId=70781)。  
- 从而不是重新安装操作系统的 Dc 中强制删除 AD DS。 如果全林性失败的原因已被标识为纯粹的 AD DS 的范围内，你无需在域控制器上重新安装操作系统。  
   - 有关从运行 Windows Server 2008 或更高版本的 DC 中强制删除 AD DS 的详细信息，请参阅[强制删除 Windows Server 2008 域控制器](https://go.microsoft.com/fwlink/?LinkId=132627)(https://go.microsoft.com/fwlink/?LinkId=132627)。 有关从运行 Windows Server 2003 DC 强制删除 AD DS 的详细信息，请参阅[一文 332199](https://go.microsoft.com/fwlink/?LinkId=70780)在 Microsoft 知识库文章 (https://go.microsoft.com/fwlink/?LinkId=70780)。  
- 使用更快的磁带设备或磁盘备份，以减少所需的时间还原操作。  
  
您还可以帮助加快通过使用从安装介质 (IFM) 功能来重新生成每个域中的 Dc 的 AD DS 安装。 IFM 可减少重新生成每个域中的 Dc 时产生的复制延迟。  
  
具有更高的服务级别协议 (SLA) 的企业可能会考虑更改林恢复过程来加快恢复速度。  
  
**问：可以自动执行林恢复过程**

由于林恢复过程的复杂而关键性质，目前它的任何端到端自动化。 林恢复过程是更物流和组织的挑战的还原比技术问题的流程自动化的业务连续性。 因此，管理环境的个人应创建特定于该环境的林恢复计划，然后自动执行可以成功地自动执行它的各个部分。  
  
可以通过使用命令行工具执行的大多数林恢复步骤。 因此，大部分步骤都是可编写脚本。 例如，Ntdsutil.exe 是林恢复过程中最常用的工具之一。  
  
尽管脚本可以加快恢复速度，但必须全面测试这些脚本之前将其应用在真实环境中。 此外，必须根据在 Active Directory 环境中，如添加新的域或 DC 或 Active Directory 的新版本的更改更新它们。

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
