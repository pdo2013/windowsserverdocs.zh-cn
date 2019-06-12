---
title: logman 更新警报
description: 'Windows 命令主题 * * *- '
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437702"
---
# <a name="logman-update-alert"></a>logman 更新警报

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更新现有的警报数据收集器的属性。  

## <a name="syntax"></a>语法  
```  
logman update alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|                 参数                  |                                                                               描述                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    显示上下文相关帮助。                                                                     |
|             -s <computer name>             |                                                          指定远程计算机上执行的命令。                                                          |
|              -config <value>               |                                                         指定包含命令选项的设置文件。                                                         |
|                [-n] <name>                 |                                                                       目标对象的名称。                                                                        |
|          -[-]u <user [password]>           | 指定为运行方式的用户。 输入\*对于密码生成提示输入密码。 在密码提示符下键入时，不会显示密码。 |
| -m <[start] [stop] [[start] [stop] [...]]> |                                                将更改为手动启动或停止而不是计划的开始或结束时间。                                                 |
|             -rf < [[hh:] mm:] ss >             |                                                        指定的时间段内运行数据收集器。                                                         |
|     -b <M/d/yyyy h:mm:ss[AM&#124;PM]>      |                                                              开始在指定的时间收集数据。                                                               |
|     -e <M/d/yyyy h:mm:ss[AM&#124;PM]>      |                                                               结束数据收集在指定的时间。                                                                |
|             -si < [[hh:] mm:] ss >             |                                                 指定性能计数器数据收集器在采样间隔。                                                  |
|           -o <path&#124;dsn!log>           |                                              指定输出日志文件或 DSN 和日志在 SQL 数据库中设置名称。                                               |
|                   -[-]r                    |                                                  重复数据收集器每天在指定的开始和结束时间。                                                  |
|                   -[-]a                    |                                                                     将添加到现有日志文件。                                                                     |
|                   -[-]ow                   |                                                                     覆盖现有日志文件。                                                                     |
|        -[-]v <nnnnnn&#124;mmddhhmm>        |                                                   将文件版本控制信息附加到日志文件名称的末尾。                                                   |
|               -[-]rc <task>                |                                                         运行指定的命令关闭每个时间。                                                          |
|              -[-]max <value>               |                                                 最大日志文件大小，单位为 MB 或最大 SQL 日志记录数。                                                  |
|           -[-] cnf < [[hh:] mm:] ss >           |     指定时间时，创建一个新文件时指定的时间已过。 当未指定时间时，创建一个新文件，当超过最大大小。     |
|                     -y                     |                                                             回答是对所有问题而不提示。                                                              |
|               -cf <filename>               |                       指定列出要收集的性能计数器的文件。 该文件应包含每行一个性能计数器名称。                        |
|                   -[-]el                   |                                                                启用或禁用事件日志报告。                                                                 |
|     阶 < 阈值 [阈值 [...]]>      |                                                        指定计数器和其警报的阈值。                                                        |
|              -[-]rdcs <name>               |                                                     指定将数据收集器集开始时触发警报。                                                      |
|               -[-]tn <task>                |                                                             指定要在触发警报时运行的任务。                                                              |
|            -[-]targ <argument>             |                                               指定要用于使用-tn 指定该任务的任务参数。                                                |

## <a name="remarks"></a>备注  
列出 [-]，其中一个额外的求反的选项。  
## <a name="BKMK_examples"></a>示例  
下面的示例更新现有的数据收集器 new_alert，设置为 40 %processor （_total） 计数器组中的计数器 %处理器时间的阈值。  
```  
logman update alert new_alert -th "\Processor(_Total)\% Processor time>40"  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
[logman 创建警报](logman-create-alert.md)  
