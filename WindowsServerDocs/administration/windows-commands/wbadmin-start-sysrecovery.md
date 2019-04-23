---
title: Wbadmin start sysrecovery
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8e0ff114d09d70b9e50e8c4ea6af6330c74128c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847158"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start sysrecovery



执行系统恢复 （裸机恢复），使用您指定的参数。

> [!NOTE]
> 只能从 Windows 恢复环境，可以运行此子命令和默认的使用情况文本中未列出**Wbadmin**。 有关详细信息，请参阅[Windows 恢复环境 (Windows RE) 概述](https://technet.microsoft.com/library/hh825173.aspx)。

若要执行系统恢复与此子命令，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-version|指定要恢复 MM/DD/yyyy 的备份的版本标识符-hh: mm 格式。 如果不知道的版本标识符，请键入**wbadmin 获取版本**。|
|-backupTarget|指定包含或多个你想要恢复的备份的存储位置。 不同于通常存储此计算机的备份的位置的存储位置时，此参数很有用。|
|-计算机|指定你想要恢复的计算机的名称。 当多台计算机已备份到同一位置时，此参数很有用。 应何时 **-backupTarget**指定参数。|
|-restoreAllVolumes|从所选备份中恢复所有卷。 如果未指定此参数，仅关键卷 （包含系统状态和操作系统组件的卷） 会恢复。 此参数时，你需要在系统恢复期间恢复非关键卷。|
|-recreateDisks|恢复到创建备份时存在的状态的磁盘配置。</br>警告：此参数会删除卷上的所有数据的主机操作系统组件。 它还可能会从数据卷中删除数据。|
|-excludeDisks|只有使用指定时才有效 **-recreateDisks**参数，必须输入为磁盘标识符的逗号分隔列表 (如下所示的输出**wbadmin 获取磁盘**)。 排除的磁盘未分区或格式化。 此参数有助于保留你不希望在恢复操作期间修改的磁盘上的数据。|
|-skipBadClusterCheck|跳过检查坏簇信息在恢复磁盘。 如果要还原到备用服务器或硬件，我们建议不使用此参数。 你可以手动运行**chkdsk/b**上随时检查它们不正确的群集，并相应地更新文件系统信息恢复磁盘上。</br>警告：直到你运行**chkdsk**如所述，你已恢复的系统上报告的错误分类可能不准确。|
|-quiet|运行该命令时不提示用户。|

## <a name="BKMK_examples"></a>示例

若要开始在上午 9:00，已在 2013 年 3 月 31 日上运行的备份恢复信息位于驱动器 d:，类型：
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
若要开始在上午 9:00，2013 年 4 月 30 日运行的备份恢复信息位于共享文件夹中\\ \\servername\shared: server01，键入：
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) cmdlet