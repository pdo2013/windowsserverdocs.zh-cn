---
title: Diskpart 脚本和示例
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c40ce79664795297af4369e35cbda7422617e6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377851"
---
# <a name="diskpart-scripts-and-examples"></a>Diskpart 脚本和示例

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用 Diskpart @no__t 运行自动执行磁盘 @ no__t 1related 任务的脚本，例如创建卷或将磁盘转换为动态磁盘。 如果使用无人参与安装程序或 Sysprep 工具（不支持创建启动卷以外的卷）部署 Windows，则编写这些任务的脚本非常有用。  
  
-   若要创建一个 Diskpart 脚本，请创建一个包含要运行的 Diskpart 命令的文本文件，其中每行一个命令，而不是空行。 您可以启动一个 `rem` 的行，使该行成为注释。  
  
    例如，下面的脚本将擦除磁盘，然后为 Windows 恢复环境创建 300 MB 的分区：  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   若要运行 DiskPart 脚本，请在命令提示符下键入以下命令，其中*scriptname*是包含脚本的文本文件的名称。  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   若要将 DiskPart 的脚本输出重定向到文件，请键入以下命令，其中*logfile*是 DiskPart 写入其输出的文本文件的名称。  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> 将**diskpart**命令用作脚本的一部分时，建议你将所有 DiskPart 操作一起作为一个 diskpart 脚本的一部分来完成。 您可以运行连续的 DiskPart 脚本，但必须在每个脚本之间至少使用15秒，才能完全关闭以前的执行，然后在后续脚本中再次运行**DiskPart**命令。 否则，后续脚本可能会失败。 可以通过将 `timeout /t 15` 命令添加到批处理文件中，并将其添加到 DiskPart 脚本，在连续的 DiskPart 脚本之间添加暂停。  
  
启动 DiskPart 时，DiskPart 版本和计算机名称将显示在命令提示符下。 默认情况下，如果在尝试执行脚本任务时，DiskPart 遇到错误，则 DiskPart 将停止处理脚本，并显示错误代码 @no__t 指定**noerr**参数 @ no__t-2。 但是，无论使用的是**noerr**参数，DiskPart 始终会在遇到语法错误时返回错误。 通过**noerr**参数，你可以执行一些有用的任务，例如，使用单个脚本删除所有磁盘上的所有分区，而不考虑磁盘的总数量。  
  
## <a name="see-also"></a>请参阅  
[示例：使用 Windows PE 和 DiskPart @ no__t-2 配置 UEFI @ no__t-0gpt @ no__t-1Based 硬盘驱动器分区  
[示例：使用 Windows PE 和 DiskPart @ no__t 配置 BIOS @ no__t-0MBR @ no__t-1Based 硬盘分区  
[Windows PowerShell 中的存储 Cmdlet](https://technet.microsoft.com/library/hh848705.aspx)  
  

