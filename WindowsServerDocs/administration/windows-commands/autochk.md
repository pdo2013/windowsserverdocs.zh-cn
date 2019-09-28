---
title: autochk
description: 用于**autochk**的 Windows 命令主题-在计算机启动时或在 Windows Server 开始验证文件系统的逻辑完整性之前运行。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76e54d14879cefd4661a1ca7f1c3b8ee7ec58de2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382349"
---
# <a name="autochk"></a>autochk



在计算机启动时运行，在 Windows Server® 2008 R2 之前运行，以验证文件系统的逻辑完整性。

**Autochk**是仅在 NTFS 磁盘上运行且仅在 Windows Server 2008 R2 启动之前运行的**Chkdsk**版本。 不能从命令行直接运行**Autochk** 。 而在下列情况下， **Autochk**会运行：
-   如果尝试在启动卷上运行**Chkdsk**
-   如果**Chkdsk**无法获取卷的独占使用
-   如果卷标记为已更新

## <a name="remarks"></a>备注

> -   [!WARNING]
>     不能从命令行直接运行**Autochk**命令行工具。 请改用**Chkntfs**命令行工具来配置在启动时要运行**Autochk**的方式。
> -   可以将**Chkntfs**与 **/x**参数一起使用，以防止**Autochk**在特定卷或多个卷上运行。
> -   使用带有 **/t**参数的**Chkntfs**命令行工具将 Autochk 延迟从0秒更改为最多3天（259200秒）。 不过，长时间延迟意味着计算机在超时或按下某个键以取消**Autochk**之前，不会启动。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[磁盘和文件系统疑难解答](https://go.microsoft.com/fwlink/?LinkId=4527)