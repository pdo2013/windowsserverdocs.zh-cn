---
title: bitsadmin getnotifyinterface
description: 适用于**bitsadmin getnotifyinterface**的 Windows 命令主题-确定另一个程序是否已注册指定作业的 COM 回调接口。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381473"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

确定其他程序是否已为指定作业注册 COM 回调接口（通知接口）。

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

[命令行语法项](command-line-syntax-key.md)