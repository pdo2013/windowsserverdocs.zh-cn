---
title: logman 创建计数器
description: 'Windows 命令主题 * * *- '
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437808"
---
# <a name="logman-create-counter"></a>logman 创建计数器

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建计数器数据收集器。  

## <a name="syntax"></a>语法  
```  
logman create counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|                    参数                     |                                                                               描述                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    显示上下文相关帮助。                                                                     |
|                -s <computer name>                |                                                          指定远程计算机上执行的命令。                                                          |
|                 -config <value>                  |                                                         指定包含命令选项的设置文件。                                                         |
|                   [-n] <name>                    |                                                                       目标对象的名称。                                                                        |
| -f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql> |                                                            指定数据收集器日志格式。                                                             |
|             -[-]u <user [password]>              | 指定为运行方式的用户。 输入\*对于密码生成提示输入密码。 在密码提示符下键入时，不会显示密码。 |
|    -m <[start] [stop] [[start] [stop] [...]]>    |                                                将更改为手动启动或停止而不是计划的开始或结束时间。                                                 |
|                -rf < [[hh:] mm:] ss >                |                                                        指定的时间段内运行数据收集器。                                                         |
|        -b <M/d/yyyy h:mm:ss[AM&#124;PM]>         |                                                              开始在指定的时间收集数据。                                                               |
|        -e <M/d/yyyy h:mm:ss[AM&#124;PM]>         |                                                               结束数据收集在指定的时间。                                                                |
|                -si < [[hh:] mm:] ss >                |                                                 指定性能计数器数据收集器在采样间隔。                                                  |
|              -o <path&#124;dsn!log>              |                                              指定输出日志文件或 DSN 和日志在 SQL 数据库中设置名称。                                               |
|                      -[-]r                       |                                                  重复数据收集器每天在指定的开始和结束时间。                                                  |
|                      -[-]a                       |                                                                     将添加到现有日志文件。                                                                     |
|                      -[-]ow                      |                                                                     覆盖现有日志文件。                                                                     |
|           -[-]v <nnnnnn&#124;mmddhhmm>           |                                                   将文件版本控制信息附加到日志文件名称的末尾。                                                   |
|                  -[-]rc <task>                   |                                                         运行指定的命令关闭每个时间。                                                          |
|                 -[-]max <value>                  |                                                 最大日志文件大小，单位为 MB 或最大 SQL 日志记录数。                                                  |
|              -[-] cnf < [[hh:] mm:] ss >              |     指定时间时，创建一个新文件时指定的时间已过。 当未指定时间时，创建一个新文件，当超过最大大小。     |
|                        -y                        |                                                             回答是对所有问题而不提示。                                                              |
|                  -cf <filename>                  |                       指定列出要收集的性能计数器的文件。 该文件应包含每行一个性能计数器名称。                        |
|               -c < 路径 [路径 []] >               |                                                              指定要收集的性能计数器。                                                               |
|                   -sc <value>                    |                                      指定要使用性能计数器数据收集器收集样本的最的大数。                                      |

## <a name="remarks"></a>备注  
列出 [-]，其中一个额外的求反的选项。  
## <a name="BKMK_examples"></a>示例  
以下命令将创建一个计数器名为 perf_log 使用 %Processor time 计数器 processor （_total） 计数器类别中的。  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time"  
```  
以下命令将创建一个计数器名为 perf_log 使用 %Processor time 计数器 processor （_total） 计数器类别中的，创建一个日志文件具有最大大小为 10 MB 和 1 分 0 秒收集数据。  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time" -max 10 -rf 01:00  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
