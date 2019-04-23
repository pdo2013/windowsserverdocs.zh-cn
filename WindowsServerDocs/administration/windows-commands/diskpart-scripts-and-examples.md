---
title: Diskpart 脚本和示例
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862788"
---
# <a name="diskpart-scripts-and-examples"></a>Diskpart 脚本和示例

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用 Diskpart`/s`运行脚本自动执行磁盘\-相关的任务，例如创建卷或将磁盘转换为动态磁盘。 这些任务的脚本是在部署 Windows 使用无人参与的安装或 Sysprep 工具不支持创建除启动卷以外的卷的情况下很有用。  
  
-   若要创建的 Diskpart 脚本，创建包含你想要运行与每行，一个命令，并没有空行的 Diskpart 命令的文本文件。 可以启动一个行，其中`rem`来对行进行注释。  
  
    例如，此处 s 擦除磁盘，然后创建 Windows 恢复环境留出 300 MB 分区的脚本：  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   若要运行 DiskPart 脚本，在命令提示符处，键入以下命令，其中*scriptname*是包含脚本的文本文件的名称。  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   若要将 DiskPart 的脚本输出重定向到一个文件中，键入以下命令，其中*日志文件*DiskPart 写入其输出的位置的文本文件的名称。  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> 使用时**DiskPart**命令为部分脚本，我们建议你完成所有 DiskPart 操作一起作为单个 DiskPart 脚本的一部分。 您可以运行连续的 DiskPart 脚本，但必须允许至少 15 秒，然后再运行上一次执行以完成关闭每个脚本之间**DiskPart**命令，再次在后续的脚本。 否则，连续脚本可能会失败。 您可以添加通过添加连续的 DiskPart 脚本之间暂停`timeout /t 15`命令与 DiskPart 脚本一起了批处理文件。  
  
启动 DiskPart 时，DiskPart 版本和计算机名称显示在命令提示符处。 默认情况下，如果 DiskPart 在尝试执行脚本的任务时遇到错误 DiskPart 停止处理脚本并显示错误代码\(除非指定了**noerr**参数\)。 但是，DiskPart 始终返回错误时遇到语法错误，而不管是否使用**noerr**参数。 **Noerr**参数使你可以执行有用的任务，例如，使用单个脚本删除而不考虑的磁盘总数的所有磁盘上的所有分区。  
  
## <a name="see-also"></a>请参阅  
[示例：配置 UEFI\/gpt\-基于的硬盘驱动器分区使用 Windows PE 和 DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[示例：配置 BIOS\/MBR\-基于通过使用 Windows PE 和 DiskPart 将硬盘分区](https://technet.microsoft.com/library/hh825677.aspx)  
[Windows PowerShell 中的存储 Cmdlet](https://technet.microsoft.com/library/hh848705.aspx)  
  

