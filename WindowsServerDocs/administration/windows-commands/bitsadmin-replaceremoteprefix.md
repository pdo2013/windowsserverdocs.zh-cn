---
title: bitsadmin replaceremoteprefix
description: 适用于**bitsadmin replaceremoteprefix**的 Windows 命令主题-作业中其远程 URL 以*OldPrefix*开头的所有文件都将更改为使用*NewPrefix*。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380796"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

作业中其远程 URL 以*OldPrefix*开头的所有文件都将更改为使用*NewPrefix*。

## <a name="syntax"></a>语法

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|OldPrefix|现有 URL 前缀|
|NewPrefix|新 URL 前缀|

## <a name="examples"></a>示例

下面的示例将名为*myDownloadJob*的作业中的所有文件（其远程 URL 以 *http://stageserver 开头）* 更改为 *http://prodserver* 。

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他信息

[命令行语法项](command-line-syntax-key.md)