---
title: PowerShell_ise
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5619396e29b446dbc6804ece7444f355dae4c0a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436298"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell 集成脚本环境 (ISE) 是图形的主机应用程序，可用于读取、 编写、 运行、 调试和测试脚本与模块图辅助环境中。 主要功能，如 IntelliSense、 显示命令、 代码段、 tab 自动补全、 语法颜色设置、 visual 调试和上下文相关帮助提供丰富的脚本编写体验。

**PowerShell_ISE.exe**工具启动 Windows PowerShell ISE 会话。 当你使用**PowerShell_ISE.exe**，可以使用其可选参数，以在 Windows PowerShell ISE 中打开文件或没有配置文件或以多线程单元启动 Windows PowerShell ISE 会话。

**PowerShell_ISE.exe**已在 Windows PowerShell 2.0 中引入并在 Windows PowerShell 3.0 中显著扩展。

## <a name="using-powershelliseexe"></a>使用 PowerShell_ISE.exe

可以使用**PowerShell_ISE.exe**开始和结束的 Windows PowerShell 会话，如下所示：
- 若要启动 Windows PowerShell ISE 会话，在命令提示符窗口中，在 Windows PowerShell 中，或在开始菜单中，键入：  
  ```
  PowerShell_Ise
  ```  
- 若要在 Windows PowerShell ISE 中打开脚本 (.ps1)、 脚本模块 (.psm1)、 模块清单 (.psd1)、 XML 文件或任何其他受支持的文件，请使用以下命令格式：  
  ```
  PowerShell_Ise <FilePath>
  ```  
  在 Windows PowerShell 3.0 中，可以使用可选**文件**参数，如下所示：  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- 若要启动 Windows PowerShell ISE 会话而无需 Windows PowerShell 配置文件，请使用**NoProfile**参数。 ( **NoProfile**参数在 Windows PowerShell 3.0 中引入的。)  
  ```
  PowerShell_Ise -NoProfile
  ```  
- 若要查看**PowerShell_ISE.exe**帮助文件中的命令提示符窗口，请使用以下命令格式：  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  有关完整列表**PowerShell_ISE.exe**命令行参数，请参阅[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)。

## <a name="start-windows-powershell-ise-in-other-ways"></a>启动 Windows PowerShell ISE 中的其他方法

有关启动 Windows PowerShell ISE 的其他方法的信息，请参阅[启动 Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>备注

Windows PowerShell 在 Windows Server 操作系统的服务器核心安装选项上运行。 但是，由于 Windows PowerShell ISE 需要图形用户界面，它不运行服务器核心安装上。

## <a name="additional-references"></a>其他参考

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[使用 Windows 编写脚本PowerShell](https://technet.microsoft.com/scriptcenter/dd742419)另请参阅