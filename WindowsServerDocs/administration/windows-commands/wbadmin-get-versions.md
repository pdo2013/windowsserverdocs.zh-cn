---
title: wbadmin get 版本
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4ebbd0d78de0ffbff1ee8c658d6d9811b87df1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813528"
---
# <a name="wbadmin-get-versions"></a>wbadmin get 版本



列出有关存储在本地计算机或另一台计算机的可用备份的详细信息。 此子命令使用不带参数，它会列出本地计算机的所有备份，即使这些备份不可用。 为备份提供的详细信息包括备份时间、 备份存储位置、 版本标识符 (所需的**wbadmin 获取项**子命令并执行恢复)，并可以执行的恢复的类型。

若要获取有关使用此子命令的可用备份的详细信息，您必须**Backup Operators**组或**管理员**组，或者您必须被委派适当权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-backupTarget|指定包含所需的详细信息的备份的存储位置。 用于列出存储在该目标位置的备份。 备份目标位置可以是本地附加的磁盘驱动器、 卷、 远程共享的文件夹，如 DVD 驱动器的可移动媒体或其他光学介质。 如果**wbadmin 获取版本**运行的同一计算机上创建备份时，不需要此参数。 但是，需要此参数来获取有关从另一台计算机创建的备份信息。|
|-计算机|指定所需的备份详细信息的计算机。 当多台计算机的备份存储在同一位置时使用。 应何时 **-backupTarget**指定。|

## <a name="remarks"></a>备注

可用于从特定备份中恢复的列表项，使用**wbadmin 获取项**。

## <a name="BKMK_examples"></a>示例

若要查看存储在卷 h 的可用备份的列表，请键入：
```
wbadmin get versions -backupTarget:h:
```
若要查看可用备份存储在远程共享文件夹中的一系列\\ \\servername\share 计算机 server01，类型：
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) cmdlet