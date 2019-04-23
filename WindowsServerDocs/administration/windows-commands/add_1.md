---
title: 添加
description: Windows 命令主题**add_1** -将卷添加到要进行卷影复制，卷的集或将别名添加到别名环境。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889948"
---
# <a name="add"></a>添加


将卷添加到要进行卷影复制，卷的集或将别名添加到别名环境。 如果不带子命令，使用**添加**列出了当前卷和别名。

> [!NOTE]
> 创建卷影副本之前，别名不会添加到别名环境。 应使用添加所需级别的别名**将别名添加**。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>添加子命令

|子命令|描述|
|----------|-----------|
|音量|将卷添加到卷影副本集，它是要进行卷影复制的卷组。 请参阅[添加卷](add-volume.md)语法和参数。|
|alias|将给定的名称和值添加到别名环境。 请参阅[添加别名](add-alias.md)语法和参数。|
|/?|显示在命令行帮助。|

## <a name="BKMK_examples"></a>示例

若要显示添加该卷并将当前在环境中的别名，请键入：
```
add
```
下面的输出显示驱动器 C，已添加到卷影副本设置：
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)