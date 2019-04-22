---
title: logman start |停止
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 47d5bda20c79ac069c8c51e51535de49b5ca380d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821888"
---
# <a name="logman-start--stop"></a>logman start |停止

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

启动数据收集器并将开始时间设置为手动或停止数据收集器设置和结束时间设置为手动。  
  
## <a name="syntax"></a>语法  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|-?|显示上下文相关帮助。|  
|-s <computer name>|指定远程计算机上执行的命令。|  
|-config <value>|指定包含命令选项的设置文件。|  
|[-n] <name>|目标对象的名称。|  
|-ets|将命令发送到事件跟踪会话中，直接而不保存或计划。|  
|-as|以异步方式执行请求的操作。|  
## <a name="BKMK_examples"></a>示例  
以下命令在远程计算机 server_1 上启动数据收集器 perf_log。  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
