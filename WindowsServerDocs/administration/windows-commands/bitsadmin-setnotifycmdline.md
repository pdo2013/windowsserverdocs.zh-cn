---
title: bitsadmin setnotifycmdline
description: 适用于 * * * * bitsadmin setnotifycmdlineSets 的 Windows 命令主题：在作业完成传输数据或作业进入状态时将运行的命令行命令。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7a307fe552e7d8ec5852de953a3a439cb02246ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380479"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

设置在作业完成传输数据或作业进入状态时将运行的命令行命令。

**BITS 1.2 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|ProgramName|作业完成时要运行的命令的名称。|
|ProgramParameters|要传递给*ProgramName*的参数。|

## <a name="remarks"></a>备注

可以为*ProgramName*和*ProgramParameters*指定 NULL。 如果*ProgramName*为 null，则*PROGRAMPARAMETERS*必须为 null。

> [!IMPORTANT]
> 如果*ProgramParameters*不为 NULL，则*ProgramParameters*中的第一个参数必须与*ProgramName*匹配。

## <a name="BKMK_examples"></a>示例

下面的示例在名为*myDownloadJob*的作业完成时，将服务使用的命令行命令设置为运行记事本。
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)