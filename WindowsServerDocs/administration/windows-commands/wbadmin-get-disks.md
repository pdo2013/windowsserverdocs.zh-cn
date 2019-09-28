---
title: wbadmin 获取磁盘
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 320edef1-df11-446b-a183-9f81811ef938
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3440e061a97e54c32179ef7d71f469093e9fae00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362424"
---
# <a name="wbadmin-get-disks"></a>wbadmin 获取磁盘



列出本地计算机当前处于联机状态的内部和外部磁盘。

若要列出使用此子命令联机的磁盘，您必须是**Backup Operators**组或**Administrators**组的成员，或者您必须被委派了适当的权限。 此外，必须在提升的命令提示符下运行**wbadmin** 。 （若要打开提升的命令提示符，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"。）

## <a name="syntax"></a>语法

```
wbadmin get disks
```

## <a name="parameters"></a>Parameters

此子命令没有参数。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBDisk](https://technet.microsoft.com/library/jj902446.aspx) cmdlet