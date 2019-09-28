---
title: 添加卷
description: 用于**添加卷**的 Windows 命令主题-将卷添加到卷影副本集，这是要进行卷影复制的卷集。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c534bcc5a264fbb51d12cfd2a6fc93b4e6fbd857
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382801"
---
# <a name="add-volume"></a>添加卷



将卷添加到卷影副本集，这是要进行卷影复制的卷集。 创建卷影副本需要此命令。 如果不使用参数， **add volume 将**在命令提示符下显示帮助。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Volume >|指定要添加到卷影副本集的卷。 创建卷影副本至少需要一个卷。|
|[provider \<ProviderID >]|指定要用于创建卷影副本的已注册提供程序的提供程序 ID。 如果未指定**提供程序**，则使用默认提供程序。|

## <a name="remarks"></a>备注

-   卷一次添加一个。
-   每次添加卷时，系统都会对其进行检查，以确保 VSS 支持卷的卷影副本创建。 不过，此主要检查可能会失效，稍后会使用**set 上下文**命令。
-   创建卷影副本时，环境变量会将别名链接到卷影 ID，因此别名随后可用于脚本编写。

## <a name="BKMK_examples"></a>示例

若要查看当前已注册的提供程序列表，请在 `DISKSHADOW>` 提示符下键入：
```
list providers
```
以下输出显示一个提供程序，默认情况下将使用该提供程序：
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```
若要将驱动器 C 添加到卷影副本集并分配名为 System1 的别名，请键入：
```
add volume c: alias System1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)