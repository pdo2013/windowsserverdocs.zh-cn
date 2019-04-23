---
title: bitsadmin geterror
description: Windows 命令主题**bitsadmin geterror** -检索的详细错误消息指定的作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a3373c0c8f290ff1f5f26ef38531fbc7745890
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889968"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



检索错误的详细信息将指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的错误信息 *myDownloadJob*。
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)