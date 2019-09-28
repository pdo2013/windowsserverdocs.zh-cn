---
title: wbadmin 获取状态
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0270a29e557ec135301753dd66c1f5f2404a8acc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362391"
---
# <a name="wbadmin-get-status"></a>wbadmin 获取状态



报告当前正在运行的备份或恢复操作的状态。

若要使用此子命令，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

## <a name="syntax"></a>语法

```
wbadmin get status
```

## <a name="parameters"></a>Parameters

此子命令没有参数。

## <a name="remarks"></a>备注

-   在当前的备份或恢复操作完成之前，此子命令不会停止，即使关闭命令窗口，子命令也会继续运行。
-   如果要停止当前的备份或恢复操作，请使用**wbadmin stop job**子命令。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet