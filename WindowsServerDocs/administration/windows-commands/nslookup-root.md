---
title: nslookup root
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc2952bdbf709c31d720a7fb57430429edf9feb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871908"
---
# <a name="nslookup-root"></a>nslookup root

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改应用到域名系统 (DNS) 域命名空间的根服务器的默认服务器。
## <a name="syntax"></a>语法
```
root 
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|{help &#124; ?}|显示的短摘要**nslookup**子命令。|
## <a name="remarks"></a>备注
-   目前，使用 ns.nic.ddn.mil 名称服务器。 此命令为 lserver ns.nic.ddn.mil 的同义词。 你可以使用的根服务器的名称**集根**命令。
## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[nslookup 设置根](nslookup-set-root.md)
