---
title: bitsadmin getreplyfilename
description: Windows 命令主题 for **bitsadmin getreplyfilename** -获取包含服务器回复的文件的路径。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b77e9bd19cdc094e6b025e143b05aff7bc60d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381275"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

获取包含服务器答复的文件的路径。

**BITS 1.2 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

仅对上传-答复作业有效。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的答复文件名。
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)