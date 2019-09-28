---
title: logman 导入 |先导
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 309274b5288bd1c17259e01cf563ae8685a2094e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374459"
---
# <a name="logman-import--export"></a>logman 导入 |先导

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从 XML 文件导入数据收集器集，或将数据收集器集导出到 XML 文件。  

## <a name="syntax"></a>语法  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Parameters  

|        参数        |                                                                        描述                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             显示区分上下文的帮助。                                                              |
|   -s <computer name>    |                                                   在指定的远程计算机上执行命令。                                                   |
|     -config <value>     |                                                  指定包含命令选项的设置文件。                                                  |
|       [-n] <name>       |                                                                目标对象的名称。                                                                 |
|       -xml <name>       |                                                         要导入或导出的 XML 文件的名称。                                                         |
|          -ets           |                                       直接将命令发送到事件跟踪会话，无需保存或计划。                                        |
| -[-] u < user [password] > | 要以其身份运行的用户。 输入密码 @no__t 0 会生成密码提示。 在密码提示符下键入密码时，不会显示密码。 |
|           -y            |                                                      在不提示的情况下回答 "是"。                                                       |

## <a name="BKMK_examples"></a>示例  
以下命令将 XML 文件 c:\windows\perf_log.xml 从计算机 server_1 导入为名为 perf_log 的数据收集器集。  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>其他参考  
[logman](logman.md)  
