---
title: bitsadmin getdisplayname
description: Windows 命令主题**bitsadmin getdisplayname** -检索指定的作业的显示名称。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1ef16f54b7b825e4293a3870d8181985b83843b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857598"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



检索指定的作业的显示名称。

## <a name="syntax"></a>语法

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的显示名称*myDownloadJob*。
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
其他参考

[命令行语法解答](command-line-syntax-key.md)