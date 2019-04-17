---
title: "广告森林恢复-常见问题"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>广告森林恢复-常见问题

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2、Windows Server 2003

本文包含常见有关森林恢复的问题（常见问题解答）：  
 
## <a name="general-recovery"></a>常规恢复
  
 
**问：我怎样才能加快恢复？** 
 
尽管恢复速度并非本指南主要目标，可以获得更短恢复的时间，方法是：  
  
-   创建详细的森林恢复计划，应定期，对其进行更新并练习它在合理大小的模拟的测试环境至少运行一次一年  
  
-   使用复制虚拟化的域控制器 (DC)  
  
     虚拟化 DC 复制加快以获取其他 Dc 某个从备份中的每个域还原直流后运行的过程。 可以复制其他虚拟化域控制器，而不是为可能漫长 AD DS 等待安装完成和完成安装后非关键复制。  
  
     森林了虚拟 Dc 承载较少的连接良好数据中心中可能带来复制恢复过程最大好处。 但是，应受益相同的域的多个虚拟化的 Dc 同一个虚拟机监控程序主机的联合位于何处任何环境。  
  
-   部署仅阅读域控制器 (Rodc)  
  
     Rodc 可以恢复过程提供企业连续性，因为它们一定要断开网络可写 Dc 一样。 Rodc 不要执行站复制。 因此，它们不会提供相同可写 Dc 带来回恢复环境中复制破坏性的数据的风险。  
  
 影响森林恢复过程的持续时间其他因素如下所示：  
  
-   当你从备份还原 Dc 时，需要到时间：  
  
    -   找到物理备份媒体，例如磁带。  
  
    -   重新安装操作系统。  
  
    -   从备份的媒体还原数据。  
  
     你可以减少重新安装操作系统和通过执行而不是系统状态还原完整的服务器恢复从备份还原数据所需的时间。 完全服务器恢复是基于二进制文件，因为它会比系统状态还原更快地完成。  
  
     但是，如果该服务器包含已从你不想要还原的系统状态数据中排除的数据，完全服务器恢复可能无法系统状态还原到可行。 请考虑执行你服务器特别是，而不是系统的状态还原完整的服务器恢复优点和准备相应地通过执行相应类型的计划来稍后还原的备份。  
  
-   当你重新生成域控制器时，它会拍摄时间复制网络基于促销活动的数据。  
  
 你可以减少通过执行以下步骤来恢复 Dc 所需的时间：  
  
-   减少检索通过备份的媒体的时间：  
  
    -   使用 Active Directory 数据库装载工具 (Dsamain.exe) 识别要用于恢复操作的最佳备份。 有关使用 Active Directory 数据库装载工具的详细信息，请参阅[Active Directory 数据库装载工具 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=132577)(https://go.microsoft.com/fwlink/?LinkId=132577)。  
  
    -   清楚标记备份的媒体，并存储在方便的方式整理媒体尚未安全，使 fast 检索的位置。  
  
    -   使用与存储区域网络 (SAN) 卷影副本服务的时间维护备份从不同的点。 有关详细信息，请参阅[Windows Server 2003 Active Directory Fast 恢复与卷影副本服务和虚拟磁盘服务](https://go.microsoft.com/fwlink/?LinkId=70781)(https://go.microsoft.com/fwlink/?LinkId=70781)。  
  
-   从而不是在重新安装操作系统 Dc 强制删除广告 DS。 如果已发现树林失败的原因是纯粹广告 DS 范围内，你不需要重新安装域控制器上的操作系统。  
  
     有关运行 Windows Server 2008 或更高版本 DC 从强制删除广告 DS 的详细信息，请参阅[强制 Windows Server 2008 删除域控制器](https://go.microsoft.com/fwlink/?LinkId=132627)(https://go.microsoft.com/fwlink/?LinkId=132627)。 有关运行 Windows Server 2003 DC 从强制删除广告 DS 的详细信息，请参阅[文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780)在知识 Microsoft 基 (https://go.microsoft.com/fwlink/?LinkId=70780)。  
  
-   使用更快的录像带设备，或者磁盘减少所需的时间备份还原操作。  
  
 你还可以帮助通过使用安装媒体 (IFM) 功能从重新生成域控制器在每个域加快广告 DS 安装。 IFM 减少时都重新生成 Dc 每个域中的，将发生复制延迟。  
  
 具有更大服务级协议 (SLA) 的企业可能要考虑变更速度恢复到森林恢复过程。  
  

**问：可以自动进行森林恢复过程？**  
 由于的性质复杂和关键森林恢复过程，目前尚不能它的端到端自动。 森林恢复过程是自动化的更多后勤和组织挑战还原比技术问题进程业务连续性。 因此，管理环境的个人应该创建森林恢复计划特定于该环境，然后自动化可以成功自动它的各部分。  
  
 使用命令行工具，你可以执行的大多数森林恢复步骤。 因此，这些步骤大多数都脚本控制。 例如，Ntdsutil.exe 是一款森林恢复过程中最常使用的工具。  
  
 尽管脚本可以速度恢复，您必须全面测试这些脚本之前适用于实际环境。 此外，你必须根据 Active Directory 环境，如添加新的域或直流或 Active Directory 的新版本中的更改更新它们。

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
