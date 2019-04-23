---
title: bitsadmin util 和版本
description: Windows 命令主题**bitsadmin util 和版本**-显示 BITS 服务的版本。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882868"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

显示 BITS 服务 (例如，2.0) 的版本。

**BITSAdmin 1.5 和更早版本**: 不支持。

## <a name="syntax"></a>语法

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>备注

**Verbose**开关执行以下：
-   显示每位相关 DLL 的文件版本
-   验证可以启动 BITS 服务
-   显示位组策略值 (仅适用于 Windows Vista)

## <a name="BKMK_examples"></a>示例

下面的示例 BITS 服务的版本。
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)