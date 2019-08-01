---
title: logman 更新警报
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fddc654d0cfe37a167c0337dcc451cdb1755e43
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "66437702"
---
# <a name="logman-update-alert"></a>logman 更新警报

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

更新现有警报数据收集器的属性。  

## <a name="syntax"></a>语法  
```  
logman update alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|                 参数                  |                                                                               描述                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    显示区分上下文的帮助。                                                                     |
|             -s<computer name>             |                                                          在指定的远程计算机上执行命令。                                                          |
|              -config <value>               |                                                         指定包含命令选项的设置文件。                                                         |
|                [-n]<name>                 |                                                                       目标对象的名称。                                                                        |
|          -[-] u < user [password] >           | 指定要以其身份运行的用户。 \*输入密码将生成密码提示。 在密码提示符下键入密码时, 不会显示密码。 |
| -m < [start] [stop] [[start] [stop] [...]]> |                                                更改为手动启动或停止, 而不是计划的开始或结束时间。                                                 |
|             -rf < [[hh:] mm:] ss >             |                                                        在指定的时间段内运行数据收集器。                                                         |
|     -b < M/d/yyyy h:mm: ss [AM&#124;PM] >      |                                                              开始在指定时间收集数据。                                                               |
|     -e < M/d/yyyy h:mm: ss [AM&#124;PM] >      |                                                               在指定的时间结束数据收集。                                                                |
|             -si < [[hh:] mm:] ss >             |                                                 指定性能计数器数据收集器的采样间隔。                                                  |
|           -o < 路径&#124;dsn! 日志 >           |                                              指定 SQL 数据库中的输出日志文件或 DSN 和日志集名称。                                               |
|                   -[-] r                    |                                                  每日在指定的开始和结束时间重复数据收集器。                                                  |
|                   -[-] a                    |                                                                     追加到现有日志文件。                                                                     |
|                   -[-] o                   |                                                                     覆盖现有的日志文件。                                                                     |
|        -[-] v < nnnnnn&#124;mmddhhmm >        |                                                   将文件版本信息附加到日志文件名称的末尾。                                                   |
|               -[-] rc<task>                |                                                         运行每次关闭日志时指定的命令。                                                          |
|              -[-] 最大值 <value>               |                                                 最大日志文件大小 (MB) 或 SQL 日志的最大记录数。                                                  |
|           -[-] .cnf < [[hh:] mm:] ss >           |     指定 time 后, 在指定的时间已过后创建新的文件。 如果未指定时间, 则在超出最大大小时创建新文件。     |
|                     -y                     |                                                             在不提示的情况下回答 "是"。                                                              |
|               -cf<filename>               |                       指定列出要收集的性能计数器的文件。 该文件应包含每行一个性能计数器名称。                        |
|                   -[-] el                   |                                                                启用或禁用事件日志报告。                                                                 |
|     -th < 阈值 [阈值 [...]]>      |                                                        为警报指定计数器及其阈值。                                                        |
|              -[-]rdcs<name>               |                                                     指定在触发警报时要启动的数据收集器集。                                                      |
|               -[-] tn<task>                |                                                             指定触发警报时要运行的任务。                                                              |
|            -[-]targ<argument>             |                                               指定要与使用-tn 指定的任务一起使用的任务参数。                                                |

## <a name="remarks"></a>备注  
其中列出了 [-], 额外-会对选项求反。  
## <a name="BKMK_examples"></a>示例  
下面的示例将更新现有的数据收集器 new_alert, 并将 "处理器 (_Total)" 计数器组中 "计数器% Processor time" 的阈值设置为 40%。  
```  
logman update alert new_alert -th "\Processor(_Total)\% Processor time>40"  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
[logman 创建警报](logman-create-alert.md)  
