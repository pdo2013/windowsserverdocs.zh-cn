---
title: Wbadmin 禁用备份
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fcaf9e8b6ef052b01b5a3184dd8f94bba433cd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821788"
---
# <a name="wbadmin-disable-backup"></a>Wbadmin 禁用备份



停止运行的现有计划每日备份。

若要禁用计划的每日备份，您必须**管理员**组，或者您必须被委派了适当的权限。 此外，您必须运行**wbadmin**从提升的命令提示符。 (若要打开提升的命令提示符右键单击**命令提示符**，然后单击**以管理员身份运行**。)

## <a name="syntax"></a>语法

```
wbadmin disable backup
[-quiet]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|-quiet|向用户使用无提示运行子命令。|

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)