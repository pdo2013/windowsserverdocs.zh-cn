---
title: logman 开始 |stop
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395d325b31ee596e1394e7ed796a444f159d15fc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374406"
---
# <a name="logman-start--stop"></a>logman 开始 |stop

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

启动数据收集器并将开始时间设置为 "手动"，或停止数据收集器集，并将结束时间设置为 "手动"。  

## <a name="syntax"></a>语法  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|     参数      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       显示区分上下文的帮助。                       |
| -s <computer name> |            在指定的远程计算机上执行命令。             |
|  -config <value>   |           指定包含命令选项的设置文件。            |
|    [-n] <name>     |                          目标对象的名称。                          |
|        -ets        | 直接将命令发送到事件跟踪会话，无需保存或计划。 |
|        -as         |               异步执行请求的操作。                |

## <a name="BKMK_examples"></a>示例  
以下命令在远程计算机 server_1 上启动数据收集器 perf_log。  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
