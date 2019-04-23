---
title: relog
description: 了解如何从性能 coutner 日志文件中提取性能计数器信息。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6804c25af04907edc8180b6a37be7efcc470f259
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869358"
---
# <a name="relog"></a>relog

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

为其他格式，如文本 TSV （适用于的制表符分隔文本）、 文本 CSV （适用于逗号分隔的文本）、 二进制文件的 BIN 或 SQL，从性能计数器日志中提取性能计数器。   

## <a name="syntax"></a>语法  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parameters  

|参数|描述|
|--|--|
|*FileName* [*FileName ...*]|指定现有性能计数器日志路径的名称。 可以指定多个输入的文件。|
|-a |将追加而不是覆盖输出文件。 此选项不适用于 SQL 的默认值始终是要追加的格式。  |
|-c *path* [*path ...*]|指定要记录的性能计数器路径。 若要指定多个计数器路径，用空格分隔这些选项并将计数器路径括在引号 (例如，**"* * * Counterpath1* * Counterpath2 ***"**)|  
|-cf *FileName*|指定的文本文件，其中列出要重新记录文件中包含的性能计数器的路径名。 使用此选项将列出计数器路径的输入文件中，每行一个。 默认设置为重新记录的原始日志文件中的所有计数器。|  
|-f {bin\| csv\|tsv\|SQL}|指定输出文件格式的路径名。 默认格式是**bin**。 对于 SQL 数据库中，指定输出文件*DSN ！CounterLog*。 可以通过使用 ODBC 管理器配置 DSN （数据库系统名称） 指定的数据库位置。  |
|-t*值*|指定在采样间隔"*N*"记录。 在重新记录文件中包含的每个第 n 个数据点。 默认值为每个数据点。|  
|-o {*OutputFile* \| *"SQL:DSN ！Counter_Log*} DSN 是在系统上定义 ODMC DSN。|指定将在其中写入计数器的 SQL 数据库的输出文件的路径名。 <br>注意：您需要有关 Relog.exe 的 64 位和 32 位版本，在 ODBC 数据源中定义 DSN (64 位和 32 位分别)|
|-b \< *M*/*D*/*YYYY*> [[*HH*:]*MM*:]*SS*|指定开始从输入文件中复制第一条记录的时间。 日期和时间必须采用此确切格式*M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-e \< *M*/*D*/*YYYY*> [[*HH*:]*MM*:]*SS* |指定用于复制的输入文件的最后一条记录的结束时间。 日期和时间必须采用此确切格式*M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-config {*FileName* \| *i*}|指定包含命令行参数的设置文件的路径名。 使用 *-i*可以在命令行放置的输入文件的列表作为占位符的配置文件中。 在命令行中，但是，您不需要使用*我*。 此外可以使用通配符，如 *.blg 来指定多个输入的文件的名称。|  
|-q|显示性能计数器和时间范围的日志输入文件中指定的文件。|  
|-y|跳过提示通过对所有问题回答""。|  
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注  
计数器路径格式：  
-   计数器路径的一般格式如下所示: [\\\<计算机 >] \\\<对象 > [\<父 >\\< 实例 #Index >] \\ \<计数器 >] 父、 实例、 索引和计数器的格式的组件可能其中包含有效的名称或通配符字符。 计算机、 父、 实例和索引组件则不需要的所有计数器。  
-   你确定要使用的计数器路径基于计数器本身。 例如，逻辑磁盘对象具有实例<Index>，因此必须提供 < #index > 或通配符。 因此，可以使用以下格式： **\LogicalDisk (\*/\*#\*)\\\***  
-   在比较中，进程对象不需要实例\<索引 >。 因此，可以使用以下格式： **\Process (\*) \ID 过程**  
-   如果父名称中指定的通配符字符，则将返回指定对象的指定的实例和计数器字段匹配的所有实例。  
-   如果实例名称中指定的通配符字符，则对应于指定索引的所有实例名称都匹配的通配符字符将返回指定的对象和父对象的所有实例。  
-   如果计数器名称中指定的通配符字符，则返回指定对象的所有计数器。  
-   不支持部分计数器路径字符串匹配项 （例如，pro *）。  

计数器的文件：  
-   计数器文件是文本文件，其中列出一个或多个现有的日志中的性能计数器。 从日志复制完整的计数器名称或 **/q**中的输出\<计算机 >\\\<对象 >\\\<实例 >\\ \<计数器 > 格式。 列出每个行上的一个计数器路径。  

复制计数器：  
-   执行时，**重新记录**副本中的每个记录的计数器输入文件中指定，如有必要转换格式。 在计数器文件允许通配符路径。  
正在保存输入的文件子集：  
-   使用 **/t**参数来指定输入的文件插入到输出文件的时间间隔在每个<n>个记录。 默认情况下，数据重新从每个记录记录。  
使用 **/b**并 **/e**使用日志文件的参数  
-   您可以指定输出日志，包括开始时间之前的记录 (即**b </b**) 提供的计数器需要格式化值的计算值的数据。 输出文件将从带有时间戳的输入文件具有最后一条记录不会早于 **/e** （即，结束时间） 参数。  
使用 **/config**选项：  
-   与一起使用的设置文件的内容 **/config**选项应采用以下格式：  
    -   \<为 CommandOption >\\\<值 >，其中\<为 CommandOption > 是一个命令行选项和\<值 > 指定其值。

有关合并的详细信息**重新记录**为您的 Windows Management Instrumentation (WMI) 脚本，请参阅，"脚本编写 WMI"网址[Microsoft Windows 资源工具包网站](https://go.microsoft.com/fwlink/?LinkId=4665)。  

## <a name="BKMK_Examples"></a>示例  
若要对现有的跟踪日志重新抽样按固定时间间隔为 30，列表的计数器路径中，输出文件和格式：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
若要对现有的跟踪日志重新抽样按固定时间间隔为 30，列出的计数器路径和输出文件：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
若要对现有的跟踪日志重新抽样到数据库，请使用：
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  
  
<!---
-   The following is a list of the possible formats:  
    -   \<computer>\\\<Object>(\<Parent>/\<Instance#Index>)\<Counter>  
    -   \<computer>\<Object>(<Parent>/<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance#Index>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>\\<Counter>  
    -   \\<Object>(<Parent>/<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Parent>/<Instance>)<Counter>  
    -   \\<Object>(<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Instance>)\\<Counter>  
    -   \\<Object>\\<Counter>  
--->