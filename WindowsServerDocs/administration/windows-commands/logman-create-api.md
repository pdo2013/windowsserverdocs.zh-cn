---
title: logman 创建 api
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: feabb946c6dd492f38d9d6e2e1dc69795839e211
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840628"
---
# <a name="logman-create-api"></a>logman 创建 api

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建的 API 跟踪数据收集器。  
  
## <a name="syntax"></a>语法  
```  
logman create api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|/?|显示上下文相关帮助。|  
|-s <computer name>|指定远程计算机上执行的命令。|  
|-config <value>|指定包含命令选项的设置文件。|  
|[-n] <name>|目标对象的名称。|  
|-f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql>|指定数据收集器日志格式。|  
|-[-]u <user [password]>|指定为运行方式的用户。 输入 * 对于密码生成提示输入密码。 在密码提示符下键入时，不会显示密码。|  
|-m <[start] [stop] [[start] [stop] [...]]>|将更改为手动启动或停止而不是计划的开始或结束时间。|  
|-rf < [[hh:] mm:] ss >|指定的时间段内运行数据收集器。|  
|-b <M/d/yyyy h:mm:ss[AM&#124;PM]>|开始在指定的时间收集数据。|  
|-e <M/d/yyyy h:mm:ss[AM&#124;PM]>|结束数据收集在指定的时间。|  
|-si < [[hh:] mm:] ss >|指定性能计数器数据收集器在采样间隔。|  
|-o <path&#124;dsn!log>|指定输出日志文件或 DSN 和日志在 SQL 数据库中设置名称。|  
|-[-]r|重复数据收集器每天在指定的开始和结束时间。|  
|-[-]a|将添加到现有日志文件。|  
|-[-]ow|覆盖现有日志文件。|  
|-[-]v <nnnnnn&#124;mmddhhmm>|将文件版本控制信息附加到日志文件名称的末尾。|  
|-[-]rc <task>|运行指定的命令关闭每个时间。|  
|-[-]max <value>|最大日志文件大小，单位为 MB 或最大 SQL 日志记录数。|  
|-[-] cnf < [[hh:] mm:] ss >|指定时间时，创建一个新文件时指定的时间已过。 当未指定时间时，创建一个新文件，当超过最大大小。|  
|-y|回答是对所有问题而不提示。|  
|-模式 < 路径 [路径 [...]]>|指定要记录的 API 调用的模块列表。|  
|-inapis < 模块 ！ api [模块 ！ api [...]]>|指定要包括在日志记录中的 API 调用的列表。|  
|-exapis < 模块 ！ api [模块 ！ api [...]]>|指定 API 调用，以便从日志记录中排除的列表。|  
|-[-]ano|日志 (-ano) API 仅，名称或不只记录 (-ano) API 名称。|  
|-[-]recursive|日志 (-递归) 或不记录 (-递归) 之外的第一层的 Api 以递归方式。|  
|-exe <value>|指定用于 API 跟踪的可执行文件的完整路径。|  
## <a name="remarks"></a>备注  
列出 [-]，其中一个额外的求反的选项。  
## <a name="BKMK_examples"></a>示例  
以下命令创建计数器为可执行文件 c:\windows\notepad.exe 调用 trace_notepad API 跟踪，并将结果输出到文件 c:\notepad.etl。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl  
```  
以下命令创建计数器为收集生成的模块 c:\windows\system32\advapi32.dll 值可执行文件 c:\windows\notepad.exe 调用 trace_notepad API 跟踪。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll  
```  
以下命令创建计数器为不包括 API 调用生成的模块 kernel32.dll TlsGetValue 可执行文件 c:\windows\notepad.exe 调用 trace_notepad API 跟踪。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
