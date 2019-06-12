---
title: bitsadmin replaceremoteprefix
description: Windows 命令主题**bitsadmin replaceremoteprefix** -在作业中的所有文件的远程 URL 开头*OldPrefix*更改为使用*NewPrefix*。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 25c0f997ea0b9f97051baa291bdf87c84b6b1cbb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811296"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

在作业中的所有文件的远程 URL 开头*OldPrefix*更改为使用*NewPrefix*。

## <a name="syntax"></a>语法

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|OldPrefix|现有的 URL 前缀|
|NewPrefix|新的 URL 前缀|

## <a name="examples"></a>示例

下面的示例更改名为作业中的所有文件*myDownloadJob*其远程 URL 开头 *http://stageserver* 到 *http://prodserver* 。

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他信息

[命令行语法项](command-line-syntax-key.md)