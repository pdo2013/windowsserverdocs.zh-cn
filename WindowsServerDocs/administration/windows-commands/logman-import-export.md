---
title: logman 导入 |导出
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d68f5f340476bbb783c47f9c3fe9c060105b4e4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437711"
---
# <a name="logman-import--export"></a>logman 导入 |导出

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从导入的数据收集器集的 XML 文件，或导出到 XML 文件的数据收集器集。  

## <a name="syntax"></a>语法  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|        参数        |                                                                        描述                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             显示上下文相关帮助。                                                              |
|   -s <computer name>    |                                                   指定远程计算机上执行的命令。                                                   |
|     -config <value>     |                                                  指定包含命令选项的设置文件。                                                  |
|       [-n] <name>       |                                                                目标对象的名称。                                                                 |
|       -xml <name>       |                                                         要导入或导出的 XML 文件的名称。                                                         |
|          -ets           |                                       将命令发送到事件跟踪会话中，直接而不保存或计划。                                        |
| -[-]u <user [password]> | 用户到运行方式。 输入\*对于密码生成提示输入密码。 在密码提示符下键入时，不会显示密码。 |
|           -y            |                                                      回答是对所有问题而不提示。                                                       |

## <a name="BKMK_examples"></a>示例  
以下命令从导入 XML 文件 c:\windows\perf_log.xml 计算机 server_1 作为数据收集器集调用的 perf_log。  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
