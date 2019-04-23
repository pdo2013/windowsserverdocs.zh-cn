---
title: bitsadmin getproxyusage
description: Windows 命令主题**bitsadmin getproxyusage** -检索指定的作业的代理使用情况设置。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863878"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



检索指定的作业的代理使用情况设置。

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
-   预配置 — 使用所有者的 Internet Explorer 默认设置。
-   NO_PROXY — 不使用代理服务器。
-   重写 — 使用显式代理列表。
-   自动检测功能-自动检测代理设置。

## <a name="BKMK_examples"></a>示例

下面的示例检索名为的作业的代理用途*myDownloadJob*。
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)