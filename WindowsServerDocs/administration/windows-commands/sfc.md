---
title: sfc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db0ab81c9469c88ddb64a367a9dc98a1fd9b70c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832388"
---
# <a name="sfc"></a>sfc

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

扫描并验证所有受保护的系统的完整性文件和不正确的版本替换为正确的版本。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/scannow|扫描所有受保护的系统文件的完整性并修复文件时可能出现的问题。|
|/verifyonly|扫描所有受保护的系统文件的完整性。 会不执行任何修复操作。|
|/scanfile|扫描指定的文件的完整性并修复的文件，如果检测到问题时，在可能的情况。|
|\<file>|指定完整路径和文件名|
|/verifyfile|验证指定的文件的完整性。 会不执行任何修复操作。|
|/offwindir|指定的脱机 windows 目录，脱机修复的位置。|
|/offbootdir|脱机指定脱机启动目录的位置|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   必须为要运行的管理员组的成员身份登录**sfc.exe**。
-   如果**sfc**发现受保护的文件已被覆盖，它将检索中的文件的正确版本**systemroot\system32\dllcache**文件夹，然后替换不正确的文件。
-   有的功能区别**sfc** Windows Server 2003、 Windows Server 2008 和 Windows Server 2008 R2 上：
-   有关详细信息**sfc**在 Windows Server 2003，请参阅[一文 310747](https://go.microsoft.com/fwlink/?LinkId=227069) Microsoft 知识库中。
-   有关详细信息**sfc**在 Windows Server 2008 和 Windows Server 2008 R2，请参阅[系统文件检查器](https://go.microsoft.com/fwlink/?LinkId=227071)。

## <a name="BKMK_examples"></a>示例
若要验证是否**kernel32.dll 文件**，类型：
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
安装程序的脱机修复**kernel32.dll**文件与设置为脱机的启动目录**d:** 并设置为脱机 windows 目录**d:\windows**，类型：
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)

