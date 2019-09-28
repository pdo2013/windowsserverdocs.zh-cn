---
title: logman 更新 cfg
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 880499048978f3a451f2ccb4e898155b49e33bcb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374360"
---
# <a name="logman-update-cfg"></a>logman 更新 cfg

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更新现有配置数据收集器的属性。  

## <a name="syntax"></a>语法  
```  
logman update cfg <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|                    参数                     |                                                                               描述                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    显示区分上下文的帮助。                                                                     |
|                -s <computer name>                |                                                          在指定的远程计算机上执行命令。                                                          |
|                 -config <value>                  |                                                         指定包含命令选项的设置文件。                                                         |
|                   [-n] <name>                    |                                                                       目标对象的名称。                                                                        |
| -f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql > |                                                            指定数据收集器的日志格式。                                                             |
|             -[-] u < user [password] >              | 指定要以其身份运行的用户。 输入密码 @no__t 0 会生成密码提示。 在密码提示符下键入密码时，不会显示密码。 |
|    -m < [start] [stop] [[start] [stop] [...]]>    |                                                更改为手动启动或停止，而不是计划的开始或结束时间。                                                 |
|                -rf < [[hh：] mm：] ss >                |                                                        在指定的时间段内运行数据收集器。                                                         |
|        -b < M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                              开始在指定时间收集数据。                                                               |
|        -e < M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                               在指定的时间结束数据收集。                                                                |
|                -si < [[hh：] mm：] ss >                |                                                 指定性能计数器数据收集器的采样间隔。                                                  |
|              -o < 路径&#124;dsn！日志 >              |                                              指定 SQL 数据库中的输出日志文件或 DSN 和日志集名称。                                               |
|                      -[-] r                       |                                                  每日在指定的开始和结束时间重复数据收集器。                                                  |
|                      -[-] a                       |                                                                     追加到现有日志文件。                                                                     |
|                      -[-] o                      |                                                                     覆盖现有的日志文件。                                                                     |
|           -[-] v < nnnnnn&#124;mmddhhmm >           |                                                   将文件版本信息附加到日志文件名称的末尾。                                                   |
|                  -[-] rc <task>                   |                                                         运行每次关闭日志时指定的命令。                                                          |
|                 -[-] 最大值 <value>                  |                                                 最大日志文件大小（MB）或 SQL 日志的最大记录数。                                                  |
|              -[-] .cnf < [[hh：] mm：] ss >              |     指定 time 后，在指定的时间已过后创建新的文件。 如果未指定时间，则在超出最大大小时创建新文件。     |
|                        -y                        |                                                             在不提示的情况下回答 "是"。                                                              |
|                      -[-] ni                      |                                                         启用（-ni）或禁用（-ni）网络接口查询。                                                          |
|             -reg < 路径 [path [...]]>             |                                                                 指定要收集的注册表值。                                                                 |
|            -"对 < 查询 [查询 [...]]">            |                                                      指定要使用 SQL 查询语言收集的 WMI 对象。                                                       |
|             -ftc < 路径 [path [...]]>             |                                                           指定要收集的文件的完整路径。                                                            |

## <a name="remarks"></a>备注  
其中列出了 [-]，额外-会对选项求反。  
## <a name="BKMK_examples"></a>示例  
以下命令将更新现有配置数据收集器 cfg_log，以收集注册表项 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion @ no__t。  
```  
logman update cfg cfg_log -reg "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\"  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
[logman 创建 cfg](logman-create-cfg.md)  
