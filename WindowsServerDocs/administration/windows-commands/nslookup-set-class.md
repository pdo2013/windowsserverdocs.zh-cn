---
title: nslookup set class
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e02a6f473a7c2dc6d6b66eb5b23fded930bc817
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856348"
---
# <a name="nslookup-set-class"></a>nslookup set class



更改查询类。 类指定的协议组的信息。

## <a name="syntax"></a>语法

```
set class=<Class>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<类 >|默认类为 in。 下面列出了此命令有效的值。</br>-在：指定 Internet 类。</br>-混沌测试：指定的混沌测试类。</br>-赫西奥德：指定 MIT Athena 赫西奥德类。</br>-任何：指定任何以前列出的通配符。|
|{帮助 | ?}|显示的短摘要**nslookup**子命令。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)