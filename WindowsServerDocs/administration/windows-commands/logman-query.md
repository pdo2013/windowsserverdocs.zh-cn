---
title: logman query
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e00e1ca7e6e090fd618af5b0ca2307bb573ab8c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437718"
---
# <a name="logman-query"></a>logman query

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

查询数据收集器或数据收集器设置的属性。  

## <a name="syntax"></a>语法  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Parameters  

|     参数      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       显示上下文相关帮助。                       |
| -s <computer name> |            指定远程计算机上执行的命令。             |
|  -config <value>   |           指定包含命令选项的设置文件。            |
|    [-n] <name>     |                          目标对象的名称。                          |
|        -ets        | 将命令发送到事件跟踪会话中，直接而不保存或计划。 |

## <a name="BKMK_examples"></a>示例  
以下命令列出了在目标系统上配置的所有数据收集器集。  
```  
logman query  
```  
以下命令将列出包含在名为 perf_log 的数据收集器集的数据收集器。  
```  
logman query "perf_log"  
```  
以下命令将列出所有可用的提供程序的目标系统上的数据收集器。  
```  
logman query providers  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
