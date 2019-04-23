---
title: Wbadmin start systemstatebackup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 591ff7caa554a892bda0bc0e888bd89a87d8b0ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863548"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



创建本地计算机的系统状态备份，并将其存储在指定的位置上。

> [!NOTE]
> Windows Server Backup 不备份或恢复注册表用户配置单元 (HKEY_CURRENT_USER) 作为系统状态备份或系统状态恢复的一部分。

若要执行与此子命令的系统状态备份，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

有关如何使用此子命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-backupTarget|指定你想要存储备份的位置。 存储位置需要驱动器号或基于 GUID 的格式的卷： \\ \\？ \Volume {*GUID*}。</br>运行 Windows Server 2008 的计算机上不支持系统状态备份到共享的网络文件夹。 如果你的服务器正在运行 Windows Server 2008 R2 或更高版本可以使用命令 **-backuptarget:\\\\servername\sharedFolder\** 来存储系统状态备份。|
|-quiet|向用户使用无提示运行子命令。|

## <a name="remarks"></a>备注

有关，为卷保存系统状态备份的信息又包含系统状态文件，请参阅 Microsoft 知识库中相应的文章 944530 ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439))。

## <a name="BKMK_examples"></a>示例

若要创建系统状态备份，并将其存储在卷 f 上，键入：
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-wbbackup](https://technet.microsoft.com/library/jj902459.aspx) cmdlet