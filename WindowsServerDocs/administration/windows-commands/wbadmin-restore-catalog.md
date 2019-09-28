---
title: wbadmin restore catalog
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b0d646440ca9b30f9fa30fb1ac3ff08458b8e44d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362335"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog



从指定的存储位置恢复本地计算机的备份目录。

若要使用此子命令恢复备份目录，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

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
|-backupTarget|指定在创建备份后的时间点的系统备份目录的位置。|
|-计算机|指定要恢复其备份目录的计算机的名称。 当多台计算机的备份存储在同一位置时使用。 当指定 **-backupTarget**时，应使用。|
|-quiet|对用户运行无提示的子命令。|

## <a name="remarks"></a>备注

如果存储备份的位置（磁盘、DVD 或远程共享文件夹）已损坏或丢失，并且无法用于还原备份目录，请使用**wbadmin delete catalog**删除损坏的目录。 在这种情况下，你应在删除备份目录后创建新备份。

## <a name="BKMK_examples"></a>示例

若要从磁盘 d：上存储的备份还原目录，请键入：
```
wbadmin restore catalog -backupTarget:d
```
若要从共享文件夹中存储的备份还原目录 \\ @ no__t-1servername\share of server01，请键入：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet