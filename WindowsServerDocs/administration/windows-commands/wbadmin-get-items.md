---
title: Wbadmin get 项
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842878"
---
# <a name="wbadmin-get-items"></a>Wbadmin get 项



列出特定备份中包含的项。

若要使用此子命令，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-version|指定的备份版本以 MM/DD/YYYY-hh: mm 格式。 如果不知道版本信息，请键入**wbadmin 获取版本**。|
|-backupTarget|指定包含想要为其详细信息的备份的存储位置。 用于列出存储在该目标位置的备份。 备份目标位置可以是本地连接的磁盘驱动器或远程共享的文件夹。 如果**wbadmin 获取项**运行的同一计算机上创建备份时，不需要此参数。 但是，需要此参数来获取有关从另一台计算机创建的备份信息。|
|-计算机|指定所需的备份详细信息的计算机的名称。 如果多台计算机已备份到同一位置非常有用。 应何时 **-backupTarget**指定。|

## <a name="BKMK_examples"></a>示例

从上午 9:00，类型在运行于 2013 年 3 月 31 日备份的列表项：
```
wbadmin get items -version:03/31/2013-09:00
```
从该备份的 server01 在 2013 年 4 月 30 日运行的上午 9:00 的列表项 和存储在\\ \\servername\share，类型：
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet