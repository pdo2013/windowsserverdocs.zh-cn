---
title: autochk
description: Windows 命令主题**autochk** -运行在计算机启动时，在 Windows Server 之前开始验证文件系统的逻辑完整性。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e6c26d42410e5466950ede4f9aa059e315030588
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435036"
---
# <a name="autochk"></a>autochk



当计算机启动时运行和之前的版本到 Windows Server® 2008 R2 开始验证文件系统的逻辑完整性。

**Autochk.exe**新版**Chkdsk**运行，仅在 NTFS 磁盘上并仅在 Windows Server 2008 R2 开始前运行。 **Autochk**不能直接从命令行运行。 相反， **Autochk**在以下情况下运行：
-   如果你尝试运行**Chkdsk**引导卷上
-   如果**Chkdsk**无法获得独占使用的数据量
-   如果该卷标记为已更新

## <a name="remarks"></a>备注

> -   [!WARNING]
>     **Autochk**命令行工具不能直接从命令行运行。 请改用**Chkntfs**命令行工具来配置所需的方式**Autochk**在启动时运行。
> -   可以使用**Chkntfs**与 **/x**参数，以防止**Autochk**从特定的卷或多个卷上运行。
> -   使用**Chkntfs.exe**使用的命令行工具 **/t**参数 Autochk 延迟从 0 秒更改为 3 天 （259,200 秒为单位）。 但是，长时间的延迟意味着计算机无法启动或直到时间结束直到按下键可取消**Autochk**。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[chkntfs](chkntfs.md)

[故障排除的磁盘和文件系统](https://go.microsoft.com/fwlink/?LinkId=4527)