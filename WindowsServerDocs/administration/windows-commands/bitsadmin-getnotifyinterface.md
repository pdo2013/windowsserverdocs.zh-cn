---
title: bitsadmin getnotifyinterface
description: Windows 命令主题**bitsadmin getnotifyinterface** -确定另一个程序已注册 COM 回调接口来指定的作业。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8316721a20cc477f9e8e15fc57b5d1c861da3ff4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868038"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

确定另一个程序是否已注册 COM 回调接口 （通知接口） 来指定的作业。

## <a name="syntax"></a>语法

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

显示已注册或未注册。

> [!NOTE]
> 不能确定注册的回调接口的程序。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的通知接口 *myDownloadJob*。
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)