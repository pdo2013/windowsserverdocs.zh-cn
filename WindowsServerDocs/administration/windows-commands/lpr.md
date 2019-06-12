---
title: lpr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afc8790b-8b52-45c4-acdf-be0ffa9da534 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc64f958dcb38129d81becfc8950059e055d8e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437487"
---
# <a name="lpr"></a>lpr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将文件发送到计算机或打印机共享中用于打印准备运行行式打印机后台程序 (LPD) 服务的设备。  

## <a name="syntax"></a>语法  
```  
lpr [-S <ServerName>] -P <printerName> [-C <BannerContent>] [-J <JobName>] [-o | "-o l"] [-x] [-d] <filename>  
```  
## <a name="parameters"></a>Parameters  

|     参数      |                                                                                                           描述                                                                                                           |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  -S <ServerName>   |                                    指定 （按名称或 IP 地址） 的计算机或打印机共享 LPD 打印队列中你想要显示的状态的设备。 必需。                                    |
|  -P <printerName>  |                                                              （按名称指定） 的打印队列的打印机与你想要显示的状态。 必需。                                                              |
| -C <BannerContent> |                指定要打印的打印作业的标题页上的内容。 如果未包括此参数，从其发送打印作业的计算机的名称将显示标题页。                 |
|    -J <JobName>    |                           指定将在标题页打印的打印作业名称。 如果未包括此参数，标题页上会显示正在打印的文件的名称。                            |
| [-o&#124; "-o l"]  | 指定要打印的文件的类型。 将参数 **-o**指定你想要打印的文本文件。 将参数 **"-o l"** 指定你想要打印的二进制文件 （例如，PostScript 文件）。 |
|         -d         |              指定数据文件必须在之前的控制文件发送。 如果你的打印机需要首先发送的数据文件，请使用此参数。 有关详细信息，请参阅打印机文档。               |
|         -x         |                               指定的**lpr**命令必须与 Sun Microsystems 操作系统 （称为 SunOS） 版本达并包括 4.1.4_u1 兼容。                                |
|     <FileName>     |                                                                                      （按名称） 中指定要打印的文件。 必需。                                                                                      |
|         /?         |                                                                                              在命令提示符下显示帮助。                                                                                               |

## <a name="remarks"></a>备注  
- 若要查找的打印机名称，请打开打印机文件夹。  
- **-S**， **-P**， **-C**，以及 **-J**参数区分大小写，必须以大写形式键入。  
  ## <a name="BKMK_examples"></a>示例  
  此示例演示如何在 LPD 主机上的 10.0.0.45 打印到 Laserprinter1 打印机队列的"文档.txt"文本文件：  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 -o Document.txt  
  ```  
  此示例演示如何在 LPD 主机上的 10.0.0.45 打印到 Laserprinter1 打印机队列的"PostScript_file.ps"PostScript 文件：  
  ```  
  lpr -S 10.0.0.45 -P Laserprinter1 "-o l" PostScript_file.ps  
  ```  

#### <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
-   [打印命令参考](print-command-reference.md)  
