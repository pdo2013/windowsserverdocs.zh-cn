---
title: wbadmin 获取项
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c3cc532381321655bbd3d5549b3c9b1896b9280
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362410"
---
# <a name="wbadmin-get-items"></a>wbadmin 获取项



列出特定备份中包含的项。

若要使用此子命令，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

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
|-版本|以 MM/DD/YYYY-HH： MM 格式指定备份的版本。 如果不知道版本信息，请键入**wbadmin get 版本**。|
|-backupTarget|指定包含要查看其详细信息的备份的存储位置。 用于列出存储在该目标位置的备份。 备份目标位置可以是本地连接的磁盘驱动器或远程共享文件夹。 如果在创建备份的同一台计算机上运行**wbadmin get items**，则不需要此参数。 但是，若要获取有关从另一台计算机创建的备份的信息，需要使用此参数。|
|-计算机|指定要获取其备份详细信息的计算机的名称。 当多台计算机备份到同一位置时，此方法非常有用。 当指定 **-backupTarget**时，应使用。|

## <a name="BKMK_examples"></a>示例

若要列出在2013年3月31日上午9:00 运行的备份中的项目，请键入：
```
wbadmin get items -version:03/31/2013-09:00
```
列出在2013年4月30日上午9:00 的 server01 备份中运行的项目。 并存储在 \\ @ no__t-1servername\share 上，请键入：
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet