---
title: nslookup set search
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf952a0337e23c0426265c6c0a4a8387a6ab45e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816988"
---
# <a name="nslookup-set-search"></a>nslookup set search



将 DNS 域搜索列表中的域名系统 (DNS) 域名称追加到请求，直到得到答复。 适用于一组，并且该查找请求包含至少一个句点，但不是以尾随句点结尾。

## <a name="syntax"></a>语法

```
set [no]search
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|**nosearch**|停止将 DNS 域搜索列表中的域名系统 (DNS) 域名称追加到该请求。|
|**search**|将 DNS 域搜索列表中的域名系统 (DNS) 域名称追加到请求，直到得到答复。 默认语法**搜索**。|
|{帮助 | ?}|显示的短摘要**nslookup**子命令。|

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)