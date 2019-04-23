---
title: Wbadmin delete 目录
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841408"
---
# <a name="wbadmin-delete-catalog"></a>Wbadmin delete 目录



删除备份存储在本地计算机的目录。 使用此命令时备份目录已损坏且无法还原使用**wbadmin 还原目录**。

若要删除与此子命令备份目录，您必须**Backup Operators**组或**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

## <a name="syntax"></a>语法

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-quiet|向用户使用无提示运行子命令。|

## <a name="remarks"></a>备注

如果删除计算机的备份目录，你将不能访问该计算机使用 Windows Server Backup 管理单元的创建的备份。 在这种情况下，如果可以访问另一个备份的位置，请使用**wbadmin 还原目录**从该位置还原备份的目录。 删除备份目录后，应创建新的备份。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)