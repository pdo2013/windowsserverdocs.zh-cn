---
title: nslookup set vc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e9232c92-cd8d-4eff-8ae5-0647bd03bdcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdbfe48397620dd11719c02facc5d5c9d6c6fe50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853488"
---
# <a name="nslookup-set-vc"></a>nslookup set vc



指定要使用或不使用虚拟线路时发送请求到的服务器。

## <a name="syntax"></a>语法

```
set [no]vc
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|**novc**|将请求发送到服务器时，指定永远不会使用虚拟线路。 默认值是**novc**。|
|**vc**|指定要向服务器发送请求时始终使用虚拟线路。|
|{帮助 | ?}|显示的短摘要**nslookup**子命令。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)