---
title: ntbackup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ebbe71fd5547311beb36747d32d695823e0f0059
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372679"
---
# <a name="ntbackup"></a>ntbackup



**Ntbackup**命令在 windows Vista 或 windows Server 2008 中不可用。 相反，应使用**wbadmin**命令和子命令从命令提示符备份和还原您的计算机和文件。

不能使用**wbadmin**恢复使用**ntbackup**创建的备份。 但是，对于要恢复使用**ntbackup**创建的备份的 windows Server 2008 和 windows Vista 用户，可以下载版本的**ntbackup** 。 使用此版本的**ntbackup** ，你可以仅执行旧备份的恢复，而不能在运行 windows Server 2008 或 Windows Vista 的计算机上使用它来创建新备份。 若要下载此版本的**ntbackup**，请参阅[https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917)。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Backup](wbadmin.md)