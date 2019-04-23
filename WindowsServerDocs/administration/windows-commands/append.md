---
title: append
description: 'Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abf7713b3fd5bbb6172969ca1cc39cbbbbafafc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881978"
---
# <a name="append"></a>append



使程序能够在指定的目录中打开数据文件，就好像在当前目录中。 如果使用不带参数，**追加**显示附加的目录列表。

> [!NOTE]
> 在 Windows 10 中不支持此命令。
>

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[\<Drive>:]<Path>|指定了驱动器和目录追加。|
|/ x： 上|适用于文件搜索和启动应用程序的附加的目录。|
|/ x： 关闭|追加的目录仅适用于请求打开文件。</br>**/ x： 关闭**是默认设置。|
|/path:on|适用于已指定的路径的文件请求的附加的目录。 **/path： 上**是默认设置。|
|/path:off|关闭的影响 **/path： 上**。|
|/e|将附加的目录列表的副本存储在名为追加一个环境变量。 **/e**可以使用仅首次使用**追加**后启动你的系统。|
|;|清除附加的目录列表。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要清除附加的目录列表，请键入：
```
append ;
```
若要存储到名为追加的环境变量的附加目录的副本，请键入：
```
append /e
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
