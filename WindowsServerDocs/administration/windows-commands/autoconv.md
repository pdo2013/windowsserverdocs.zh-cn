---
title: autoconv
description: 适用于**autoconv**的 Windows 命令主题-将文件分配表（Fat）和 Fat32 卷转换为 NTFS 文件系统。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf36be6bcf3dd8f6c61c6ab0d8780ed77dd8903a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383455"
---
# <a name="autoconv"></a>autoconv

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在运行**autochk**后，将文件分配表（Fat）和 Fat32 卷转换为 NTFS 文件系统，以使现有文件和目录在启动时保持不变。 转换为 NTFS 文件系统的卷无法转换回 Fat 或 Fat32。
## <a name="remarks"></a>备注
不能在命令行上运行**autoconv** 。 这只会在启动时运行，前提是通过**convert**进行设置。
## <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
[autochk](autochk.md)
[转换](convert.md)
[使用文件系统](https://go.microsoft.com/fwlink/?LinkId=4509)
