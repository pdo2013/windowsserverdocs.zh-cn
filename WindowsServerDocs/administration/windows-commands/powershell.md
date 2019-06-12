---
title: PowerShell
description: 了解如何从命令提示符下打开 PowerShell 控制台。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1e2ccf6187e4480f94b30632b6f8f9f092052541
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811075"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell 是一种基于任务的命令行 shell 和脚本语言，专门为系统管理。 在 .NET Framework 的基础上构建的 Windows PowerShell 可帮助 IT 专业人士和高级用户控制和自动执行 Windows 操作系统以及在 Windows 上运行的应用程序的管理。

**PowerShell.exe**命令行工具在命令提示符窗口中启动 Windows PowerShell 会话。 当你使用**PowerShell.exe**，可以使用其可选参数来自定义会话。 例如，可以开始使用特定的执行策略或不包括 Windows PowerShell 配置文件的会话。 否则，该会话是在 Windows PowerShell 控制台中启动的任何会话相同。

## <a name="using-powershellexe"></a>使用 PowerShell.exe

可以使用**PowerShell.exe**命令行工具在命令提示符窗口中启动 Windows PowerShell 会话。

- 若要启动 Windows PowerShell 会话的命令提示符窗口中，键入`PowerShell`。 一个**PS**前缀添加到命令提示符，以表明你是在 Windows PowerShell 会话中。

- 若要使用特定的执行策略启动会话，请使用**ExecutionPolicy**参数。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要启动 Windows PowerShell 配置文件没有的 Windows PowerShell 会话，请使用**NoProfile**参数。

    ```
    PowerShell.exe -NoProfile
    ```
  
- 若要启动会话，请使用**ExecutionPolicy**参数。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- 若要查看 PowerShell.exe 帮助文件，请使用以下命令格式。  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- 若要在命令提示符窗口中结束的 Windows PowerShell 会话，请键入`exit`。 典型的命令提示符返回。

有关完整列表**PowerShell.exe**命令行参数，请参阅[about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439)。

## <a name="other-ways-to-start-windows-powershell"></a>启动 Windows PowerShell 的其他方法

有关启动 Windows PowerShell 的其他方法的信息，请参阅[启动 Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>备注

Windows PowerShell 在 Windows Server 操作系统的服务器核心安装选项上运行。 但是，需要图形用户的功能接口，如[Windows PowerShell 集成脚本环境 (ISE)](https://technet.microsoft.com/library/hh849182)，并[Out-gridview](https://go.microsoft.com/fwlink/?LinkID=113364)和[Show-command](https://go.microsoft.com/fwlink/?LinkID=217448)cmdlet，不要在服务器核心安装上运行。

## <a name="additional-references"></a>其他参考

[about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[使用 Windows 编写脚本PowerShell](https://technet.microsoft.com/scriptcenter/dd742419)另请参阅