---
title: 添加卷
description: Windows 命令主题**添加卷**-将卷添加到卷影副本集，这是组的卷进行卷影复制。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8960ffafdf49d4512e1df2dfcc046bdfbe56e224
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819468"
---
# <a name="add-volume"></a>添加卷



将卷添加到卷影副本集，它是要进行卷影复制的卷组。 此命令将创建卷影副本所必需。 如果使用不带参数，**添加卷**在命令提示符下显示的帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<卷 >|指定要添加到卷影副本设置的卷。 卷影副本创建需要至少一个卷。|
|[提供程序\<ProviderID >]|指定要用于创建卷影副本的已注册提供程序的提供程序 ID。 如果**提供程序**未指定，则使用默认提供程序。|

## <a name="remarks"></a>备注

-   卷添加一次一个。
-   每次添加一个卷时，它将检查以确保 VSS 支持该卷的卷影副本创建。 此主检查可能会失效，但是，通过更高版本使用**设置上下文**命令。
-   创建卷影副本时，环境变量链接到的卷影 ID，别名，因此可以然后使用别名，用于编写脚本。

## <a name="BKMK_examples"></a>示例

若要查看已注册提供程序的当前列表在`DISKSHADOW>`提示符下，键入：
```
list providers
```
以下输出显示单个提供程序，将默认情况下使用：
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```
若要将驱动器 C 添加到卷影副本集并分配别名命名系统 1，键入：
```
add volume c: alias System1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)