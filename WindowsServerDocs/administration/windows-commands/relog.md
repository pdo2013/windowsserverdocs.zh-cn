---
title: relog
description: 了解如何从性能 coutner 日志文件提取性能计数器信息。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: daedd85f1557c191a690e7eb750559cfd268d3a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371623"
---
# <a name="relog"></a>relog

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从性能计数器日志中提取性能计数器，将其转换为其他格式（例如，制表符分隔的文本）、CSV （对于逗号分隔的文本）、二进制 BIN 或 SQL。   

## <a name="syntax"></a>语法  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parameters  

|                                         参数                                          |                                                                                                                                                                  描述                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *FileName* [*filename ...* ]                                 |                                                                                                                      指定现有性能计数器日志的路径名。 可以指定多个输入文件。                                                                                                                      |
|                                             -a                                             |                                                                                                          追加输出文件而不是覆盖。 此选项不适用于默认情况下始终追加的 SQL 格式。                                                                                                           |
|                                   -c*路径*[*path ...* ]                                   |                                                       指定要记录的性能计数器路径。 若要指定多个计数器路径，请用空格分隔它们，并将计数器路径用引号括起来（例如， **"** <em>Counterpath1</em> <em>Counterpath2</em> **"** ）                                                       |
|                                       -cf *FileName*                                       |                                            指定一个文本文件的路径，该文件列出要包含在重新记文件中的性能计数器。 使用此选项可以在输入文件中列出计数器路径，每行一个。 默认设置为 relogged 原始日志文件中的所有计数器。                                            |
|                                  -f {bin @ no__t-0 csv @ no__t-1tsv @ no__t-2SQL}                                  |                                       指定输出文件格式的路径名。 默认格式为**bin**。 对于 SQL 数据库，输出文件指定*DSN！CounterLog*。 您可以使用 ODBC 管理器配置 DSN （数据库系统名称）来指定数据库位置。                                        |
|                                         -t*值*                                         |                                                                                                           指定 "*N*" 记录中的采样间隔。 包括重新登录文件中的每个第 n 个数据点。 默认值为每个数据点。                                                                                                           |
| -o {*OutputFile* \| *"SQL： DSN！Counter_Log*} 其中，dsn 是在系统上定义的 ODMC dsn。 |                                                   指定输出文件或将写入计数器的 SQL 数据库的路径名。 <br>注意:对于64位和32位版本的 dcdiag.exe，需要在 ODBC 数据源中定义 DSN （分别为64位和32位）                                                   |
|                          -b \<*M*/*D*/*YYYY*> [[*HH*：]*MM*：]*SS*                           |                                                                          指定从输入文件复制第一条记录的开始时间。 日期和时间必须采用这一精确格式： <em>M</em> **@no__t**<em>D</em> **/** <em>YYYYHH</em> **：** <em>MM</em> **：** <em>SS</em>。                                                                          |
|                          -e \<*M*/*D*/*YYYY*> [[*HH*：]*MM*：]*SS*                           |                                                                           指定从输入文件复制最后一条记录的结束时间。 日期和时间必须采用这一精确格式： <em>M</em> **@no__t**<em>D</em> **/** <em>YYYYHH</em> **：** <em>MM</em> **：** <em>SS</em>。                                                                            |
|                                -config {*FileName* \| *i*}                                 | 指定包含命令行参数的设置文件的路径名。 在配置文件中使用 *-i*作为可放置在命令行上的输入文件列表的占位符。 但在命令行上，不需要使用*i*。 你还可以使用通配符（如 @no__t 旁1/-0）指定多个输入文件名。 |
|                                             -q                                             |                                                                                                                          显示在输入文件中指定的日志文件的性能计数器和时间范围。                                                                                                                           |
|                                             -y                                             |                                                                                                                                            通过对所有问题回答 "是"，绕过提示。                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      在命令提示符下显示帮助。                                                                                                                                                      |

## <a name="remarks"></a>备注  
计数器路径格式：  
- 计数器路径的一般格式如下： [\\ @ no__t-1computer >] \\ @ no__t-3Object > [\<Parent > \\ < 实例 # Index >] \\ @ no__t-7Counter >]，其中 parent、Instance、Index 和 counter格式的组件可以包含有效名称或通配符。 计算机、父对象、实例和索引组件对于所有计数器都不是必需的。  
- 根据计数器本身，确定要使用的计数器路径。 例如，逻辑磁盘对象的实例 @no__t 为-0，因此必须 #index > 或通配符提供 <。 因此，你可以使用以下格式： **\LogicalDisk （\* @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5） \\ @ no__t-7***  
- 相比之下，Process 对象不需要 @no__t 的实例 >。 因此，可以使用以下格式： **\Process （\*） \ID 进程**  
- 如果在父名称中指定了通配符，则将返回与指定的实例和计数器字段匹配的指定对象的所有实例。  
- 如果在实例名称中指定了通配符，则如果与指定索引对应的所有实例名称匹配通配符，则将返回指定对象和父对象的所有实例。  
- 如果在计数器名称中指定了通配符，则返回指定对象的所有计数器。  
- 不支持部分计数器路径字符串匹配项（例如，pro *）。  

计数器文件：  
-   计数器文件是一个文本文件，用于列出现有日志中的一个或多个性能计数器。 从日志或 **/q**输出的 \<computer > 中复制完整计数器名称 \\ @ No__t-3Object > \\ @ 5Instance-no__t > 格式。 在每行上列出一个计数器路径。  

复制计数器：  
-   执行时，重新记录将从输入文件中的每个**记录复制指定**的计数器，并在必要时转换格式。 计数器文件中允许使用通配符路径。  
保存输入文件子集：  
-   使用 **/t**参数指定输入文件按照每个 @no__t n-1 记录的间隔插入到输出文件中。 默认情况下，数据从每个记录 relogged。  
将 **/b**和 **/e**参数用于日志文件  
-   您可以指定您的输出日志中包含开始时间（即 **/b**）之前的记录，以提供需要格式化值的计算值的计数器的数据。 输出文件将包含来自输入文件的最后一条记录，其时间戳小于 **/e** （即结束时间）参数。  
使用 **/config**选项：  
-   用于 **/config**选项的设置文件的内容应采用以下格式：  
    -   \<CommandOption > \\ @ no__t-2Value >，其中 @no__t 3CommandOption > 是命令行选项，\<Value > 指定其值。

有关将**重新登录纳入**WINDOWS MANAGEMENT INSTRUMENTATION （WMI）脚本的详细信息，请参阅[Microsoft Windows 资源工具包网站](https://go.microsoft.com/fwlink/?LinkId=4665)上的 "脚本编写 WMI"。  

## <a name="BKMK_Examples"></a>示例  
若要按固定的时间间隔（30）对现有跟踪日志进行重新采样，请列出计数器路径、输出文件和格式：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
若要按固定的时间间隔（30）对现有跟踪日志进行重新采样，请列出计数器路径和输出文件：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
若要将现有跟踪日志重新采样为数据库，请使用：
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他参考  
-   [命令行语法项](command-line-syntax-key.md)  
