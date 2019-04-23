---
title: ntbackup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 783f73eba2aeaf9f30c5c1e12a623f1f87f24ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846338"
---
# <a name="ntbackup"></a>ntbackup



**Ntbackup**命令不是在 Windows Vista 或 Windows Server 2008 中可用。 相反，应使用**wbadmin**命令和子命令来备份和还原您的计算机和文件从命令提示符。

您不能恢复使用创建的备份**ntbackup**通过使用**wbadmin**。 但是，新版**ntbackup**可为想要恢复使用它们创建的备份 Windows Server 2008 和 Windows Vista 用户下载**ntbackup**。 此可下载版本的**ntbackup**使您可以执行的旧备份，并且仅恢复不能用于运行 Windows Server 2008 或 Windows Vista 的计算机上创建新的备份。 若要下载此版本的**ntbackup**，请参阅[ https://go.microsoft.com/fwlink/?LinkId=82917 ](https://go.microsoft.com/fwlink/?LinkId=82917)。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)