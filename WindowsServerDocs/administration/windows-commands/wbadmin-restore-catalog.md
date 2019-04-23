---
title: Wbadmin 还原目录
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851708"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin 还原目录



从指定的存储位置恢复本地计算机的备份目录。

若要恢复备份编录与此子命令，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-backupTarget|当时点处创建备份后，请指定系统备份目录的位置。|
|-计算机|指定你想要恢复的备份目录的计算机的名称。 当多个计算机的备份存储在同一位置时使用。 应何时 **-backupTarget**指定。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="remarks"></a>备注

如果你在其中存储你的备份的位置 （磁盘、 DVD 或远程共享的文件夹） 损坏或丢失，不能用于还原备份的目录，请使用**wbadmin 删除目录**删除损坏的目录。 在这种情况下，应创建新的备份后删除备份目录。

## <a name="BKMK_examples"></a>示例

若要从存储在 d： 磁盘上的备份还原目录，请键入：
```
wbadmin restore catalog -backupTarget:d
```
若要从共享文件夹中存储的备份还原目录\\ \\servername\share server01，类型：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [还原 WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet