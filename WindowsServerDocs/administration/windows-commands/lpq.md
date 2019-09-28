---
title: lpq
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3755c010c9bb4549deed08f26b5a0670fe7318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374201"
---
# <a name="lpq"></a>lpq

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示运行 Line printer Daemon （LPD）的计算机上打印队列的状态。  

## <a name="syntax"></a>语法  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parameters  

|    参数     |                                                                        描述                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | 使用要显示的状态指定（按名称或 IP 地址）承载 LPD 打印队列的计算机或打印机共享设备。 必需。 |
| -P <printerName> |                           用要显示的状态指定（按名称）打印队列的打印机。 必需。                           |
|        -l        |                                      指定您希望显示有关打印队列状态的详细信息。                                      |
|        /?        |                                                           在命令提示符下显示帮助。                                                            |

## <a name="remarks"></a>备注  
**-S**和 **-P**参数区分大小写，并且必须以大写字母形式键入。  
## <a name="BKMK_examples"></a>示例  
此示例演示如何在10.0.0.45 上显示 LPD 主机上的 Laserprinter1 打印机队列的状态：  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
[打印命令参考](print-command-reference.md)  
