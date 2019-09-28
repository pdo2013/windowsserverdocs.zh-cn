---
title: PowerShell
description: 了解如何从命令提示符中打开 PowerShell 控制台。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2c43c71fce9bb25efcf3f03284160d5534475a8a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372201"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell 是一种基于任务的命令行 shell 和脚本语言，专为系统管理而设计。 在 .NET Framework 的基础上构建的 Windows PowerShell 可帮助 IT 专业人士和高级用户控制和自动执行 Windows 操作系统以及在 Windows 上运行的应用程序的管理。

**Ngen.exe**命令行工具在命令提示符窗口中启动 Windows PowerShell 会话。 使用**PowerShell**时，可以使用它的可选参数自定义会话。 例如，可以启动使用特定执行策略或排除 Windows PowerShell 配置文件的会话。 否则，该会话与 Windows PowerShell 控制台中启动的任何会话相同。

## <a name="using-powershellexe"></a>使用 ngen.exe

您可以使用**ngen.exe**命令行工具在命令提示符窗口中启动 Windows PowerShell 会话。

- 若要在命令提示符窗口中启动 Windows PowerShell 会话，请键入 `PowerShell`。 将**PS**前缀添加到命令提示符，以指示你处于 Windows PowerShell 会话中。

- 若要使用特定执行策略启动会话，请使用**set-executionpolicy**参数。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要在不使用 Windows PowerShell 配置文件的情况下启动 Windows PowerShell 会话，请使用**NoProfile**参数。

    ```
    PowerShell.exe -NoProfile
    ```
  
- 若要启动会话，请使用**set-executionpolicy**参数。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- 若要查看 ngen.exe 帮助文件，请使用以下命令格式。  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- 若要在命令提示符窗口中结束 Windows PowerShell 会话，请键入 `exit`。 典型的命令提示符返回。

有关**ngen.exe**命令行参数的完整列表，请参阅[about_PowerShell](https://go.microsoft.com/fwlink/?LinkID=113439)。

## <a name="other-ways-to-start-windows-powershell"></a>启动 Windows PowerShell 的其他方法

有关启动 Windows PowerShell 的其他方法的信息，请参阅[启动 Windows powershell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>备注

Windows PowerShell 在 Windows Server 操作系统的服务器核心安装选项上运行。 但是，需要图形用户界面的功能（如[Windows PowerShell 集成脚本环境（ISE））](https://technet.microsoft.com/library/hh849182)和[Out](https://go.microsoft.com/fwlink/?LinkID=113364)和[Show Command](https://go.microsoft.com/fwlink/?LinkID=217448) cmdlet 不能在服务器核心安装上运行。

## <a name="additional-references"></a>其他参考

[about_PowerShell](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise](https://go.microsoft.com/fwlink/?LinkId=256512)
[windows Powershell](https://go.microsoft.com/fwlink/?LinkID=107116)
[通过 windows powershell 编写脚本](https://technet.microsoft.com/scriptcenter/dd742419)的另请参阅