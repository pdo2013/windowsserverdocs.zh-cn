---
title: Wbadmin get 状态
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863408"
---
# <a name="wbadmin-get-status"></a>Wbadmin get 状态



报告当前运行的备份或恢复操作的状态。

若要使用此子命令，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

## <a name="syntax"></a>语法

```
wbadmin get status
```

## <a name="parameters"></a>Parameters

此子命令没有任何参数。

## <a name="remarks"></a>备注

-   此子命令不会停止直到当前备份或恢复操作完成，子命令将继续运行，即使关闭命令窗口。
-   如果你想要停止当前备份或恢复操作，使用**wbadmin 停止作业**子命令。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet