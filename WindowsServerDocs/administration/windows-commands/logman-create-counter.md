---
title: logman create 计数器
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d9099fa4540a1d9c91a714ada8a1dbba13f051e
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "66437808"
---
# <a name="logman-create-counter"></a>logman create 计数器

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

创建计数器数据收集器。  

## <a name="syntax"></a>语法  
```  
logman create counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|                    参数                     |                                                                               描述                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    显示区分上下文的帮助。                                                                     |
|                -s<computer name>                |                                                          在指定的远程计算机上执行命令。                                                          |
|                 -config <value>                  |                                                         指定包含命令选项的设置文件。                                                         |
|                   [-n]<name>                    |                                                                       目标对象的名称。                                                                        |
| -f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql > |                                                            指定数据收集器的日志格式。                                                             |
|             -[-] u < user [password] >              | 指定要以其身份运行的用户。 \*输入密码将生成密码提示。 在密码提示符下键入密码时, 不会显示密码。 |
|    -m < [start] [stop] [[start] [stop] [...]]>    |                                                更改为手动启动或停止, 而不是计划的开始或结束时间。                                                 |
|                -rf < [[hh:] mm:] ss >                |                                                        在指定的时间段内运行数据收集器。                                                         |
|        -b < M/d/yyyy h:mm: ss [AM&#124;PM] >         |                                                              开始在指定时间收集数据。                                                               |
|        -e < M/d/yyyy h:mm: ss [AM&#124;PM] >         |                                                               在指定的时间结束数据收集。                                                                |
|                -si < [[hh:] mm:] ss >                |                                                 指定性能计数器数据收集器的采样间隔。                                                  |
|              -o < 路径&#124;dsn! 日志 >              |                                              指定 SQL 数据库中的输出日志文件或 DSN 和日志集名称。                                               |
|                      -[-] r                       |                                                  每日在指定的开始和结束时间重复数据收集器。                                                  |
|                      -[-] a                       |                                                                     追加到现有日志文件。                                                                     |
|                      -[-] o                      |                                                                     覆盖现有的日志文件。                                                                     |
|           -[-] v < nnnnnn&#124;mmddhhmm >           |                                                   将文件版本信息附加到日志文件名称的末尾。                                                   |
|                  -[-] rc<task>                   |                                                         运行每次关闭日志时指定的命令。                                                          |
|                 -[-] 最大值 <value>                  |                                                 最大日志文件大小 (MB) 或 SQL 日志的最大记录数。                                                  |
|              -[-] .cnf < [[hh:] mm:] ss >              |     指定 time 后, 在指定的时间已过后创建新的文件。 如果未指定时间, 则在超出最大大小时创建新文件。     |
|                        -y                        |                                                             在不提示的情况下回答 "是"。                                                              |
|                  -cf<filename>                  |                       指定列出要收集的性能计数器的文件。 该文件应包含每行一个性能计数器名称。                        |
|               -c < 路径 [path []] >               |                                                              指定要收集的性能计数器。                                                               |
|                   -sc <value>                    |                                      指定要使用性能计数器数据收集器收集的样本的最大数目。                                      |

## <a name="remarks"></a>备注  
其中列出了 [-], 额外-会对选项求反。  
## <a name="BKMK_examples"></a>示例  
下面的命令使用处理器 (_Total) 计数器类别中的% Processor time 计数器创建名为 perf_log 的计数器。  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time"  
```  
以下命令使用 "处理器 (_Total)" 计数器类别中的 "处理器时间百分比" 计数器创建名为 perf_log 的计数器, 创建最大大小为 10 MB 的日志文件, 并收集1分钟到0秒的数据。  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time" -max 10 -rf 01:00  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
