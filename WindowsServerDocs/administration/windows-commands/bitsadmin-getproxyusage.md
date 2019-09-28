---
title: bitsadmin getproxyusage
description: 适用于**bitsadmin getproxyusage**的 Windows 命令主题-检索指定作业的代理使用情况设置。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381289"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



检索指定作业的代理使用情况设置。

## <a name="syntax"></a>语法

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="remarks"></a>备注

可能的值为：
-   预配置—使用所有者的 Internet Explorer 默认值。
-   NO_PROXY —不要使用代理服务器。
-   替代-使用显式代理列表。
-   自动检测-自动检测代理设置。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的代理使用情况。
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)