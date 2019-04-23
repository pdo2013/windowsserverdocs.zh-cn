---
title: logman delete
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00473b5178eca93e9644888fbaa4c4c96c0dd683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842058"
---
# <a name="logman-delete"></a>logman delete

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除现有的数据收集器。  
  
## <a name="syntax"></a>语法  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/?|显示上下文相关帮助。|  
|-s <computer name>|指定远程计算机上执行的命令。|  
|-config <value>|指定包含命令选项的设置文件。|  
|[-n] <name>|目标数据收集器的名称。|  
|-ets|将命令发送到事件跟踪会话中，直接而不保存或计划。|  
|-[-]u <user [password]>|指定为运行方式的用户。 输入 * 对于密码生成提示输入密码。 在密码提示符下键入时，不会显示密码。|  
## <a name="BKMK_examples"></a>示例  
以下命令将删除数据收集器 perf_log。  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
