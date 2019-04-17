---
title: "初始化新磁盘"
description: "本文介绍了如何初始化新磁盘"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>初始化新磁盘

> **适用于：**Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

## <a name="to-initialize-new-disks"></a>初始化新磁盘
1.  在“磁盘管理”中，右键单击想要初始化的磁盘，然后单击**初始化磁盘**。

2.  在**初始化磁盘**对话框中，选择要初始化的磁盘。 你可以选择是使用主启动记录 (MBR) 还是 GUID 分区表 (GPT) 分区样式。

## <a name="additional-considerations"></a>其他注意事项

-   新磁盘将显示为**未初始化**。 在使用磁盘之前，你必须先初始化磁盘。 如果在添加磁盘后启动磁盘管理，则将显示初始化磁盘向导，便于你初始化磁盘。

> [!NOTE]
> 新磁盘会被初始化为基本磁盘。

