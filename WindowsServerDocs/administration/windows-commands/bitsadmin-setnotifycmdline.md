---
title: bitsadmin setnotifycmdline
description: Windows 命令主题 * * *-bitsadmin setnotifycmdlineSets 将在作业完成传输数据时或当作业进入状态时运行的命令行命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859068"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

设置将在作业完成传输数据时或当作业进入状态时运行的命令行命令。

**1.2 及更早版本的 BITS**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ProgramName|作业完成时要运行该命令的名称。|
|ProgramParameters|要传递给所需的参数*ProgramName*。|

## <a name="remarks"></a>备注

可以指定为空， *ProgramName*并*ProgramParameters*。 如果*ProgramName*为 NULL， *ProgramParameters*必须为 NULL。

> [!IMPORTANT]
> 如果*ProgramParameters*为 NULL，则在第一个参数不是*ProgramParameters*必须与匹配*ProgramName*。

## <a name="BKMK_examples"></a>示例

下面的示例设置该服务用于运行时的作业名为记事本的命令行命令*myDownloadJob*完成。
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)