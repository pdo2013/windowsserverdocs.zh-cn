---
title: bitsadmin util 和版本
description: 适用于**bitsadmin util 和版本**的 Windows 命令主题显示 BITS 服务的版本。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380196"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

显示 BITS 服务的版本（例如，2.0）。

**BITSAdmin 1.5 及更早版本**： 不受支持。

## <a name="syntax"></a>语法

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>备注

**详细**开关执行以下操作：
-   显示每个与 BITS 相关的 DLL 的文件版本
-   验证是否可以启动 BITS 服务
-   显示位组策略值（仅适用于 Windows Vista）

## <a name="BKMK_examples"></a>示例

下面的示例是 BITS 服务的版本。
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)