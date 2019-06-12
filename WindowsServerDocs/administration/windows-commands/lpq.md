---
title: lpq
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 18ff1ff3ecbc2df0a437ec8a465dec9a12123ede
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437510"
---
# <a name="lpq"></a>lpq

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示运行行式打印机后台程序 (LPD) 的计算机上的打印队列的状态。  

## <a name="syntax"></a>语法  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Parameters  

|    参数     |                                                                        描述                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | 指定 （按名称或 IP 地址） 的计算机或打印机共享 LPD 打印队列中你想要显示的状态的设备。 必需。 |
| -P <printerName> |                           （按名称指定） 的打印队列的打印机与你想要显示的状态。 必需。                           |
|        -l        |                                      指定你想要显示有关打印队列的状态的详细信息。                                      |
|        /?        |                                                           在命令提示符下显示帮助。                                                            |

## <a name="remarks"></a>备注  
**-S**并 **-P**参数区分大小写，必须以大写形式键入。  
## <a name="BKMK_examples"></a>示例  
此示例演示如何在 10.0.0.45 LPD 主机上显示的 Laserprinter1 打印机队列状态：  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
[打印命令参考](print-command-reference.md)  
