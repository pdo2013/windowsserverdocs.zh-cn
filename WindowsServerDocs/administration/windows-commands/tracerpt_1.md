---
title: tracerpt
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4d943da40793be0680d6ea6a4d9de0960f65c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861698"
---
# <a name="tracerpt"></a>tracerpt



**Tracerpt**命令可用于分析事件跟踪日志，性能监视器和实时事件跟踪提供程序生成的日志文件。 它生成转储文件、 报表文件和报表架构。

有关如何使用的示例**tracerpt**，请参阅[示例](#BKMK_EXAMPLES)。

## <a name="syntax"></a>语法

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>选项

|选项标志|描述|
|-----------|-----------|
|-?|显示上下文相关帮助。|
|-config \<filename>|加载包含命令选项的设置文件。|
|-y|回答是对所有问题而不提示。|
|-f \<XML | HTML>|定义报表格式。|
|-的\<CSV | EVTX | XML>|定义转储格式。 默认值为 XML。|
|-df \<filename>|创建特定于 Microsoft 的计数/报告架构文件。|
|-int\<文件名 >|转储到指定的文件的解释型的事件结构。|
|-rts|事件跟踪标头中的报表原始时间戳。 仅使用与-o，-报表或-摘要。|
|-tmf \<filename>|指定的跟踪消息格式定义文件。|
|-tp \<value>|指定 TMF 文件搜索路径。 可以使用多个路径，以分号 （;） 分隔。|
|-i \<value>|指定的提供程序图像路径。 匹配的 PDB 将位于符号服务器。 可以使用多个路径，以分号 （;） 分隔。|
|-pdb \<value>|指定符号服务器路径。 可以使用多个路径，以分号 （;） 分隔。|
|-gmt|WPP 负载时间戳转换为格林威治标准时间。|
|-rl \<value>|定义从 1 到 5 的系统报表级别。 默认值为 1。|
|-摘要 [文件名]|生成摘要报告文本文件。 如果未指定的文件名为 summary.txt。|
|-o [filename]|生成文本输出文件。 如果未指定的文件名为 dumpfile.xml。|
|-report [filename]|生成文本输出报告文件。 如果未指定的文件名是 workload.xml。|
|-lr|指定"限制较少。" 这将使用与事件架构不匹配的事件的最大努力。|
|-export [filename]|生成的事件架构导出文件。 如果未指定的文件名是 schema.man。|
|[-l]。\<值 [值 [...]]>|指定要处理的事件跟踪日志文件。|
|-rt \<session_name [session_name […]]>|指定实时事件跟踪会话数据源。|

## <a name="BKMK_EXAMPLES"></a>示例

-   此示例将创建两个事件日志所基于的报表**logfile1.etl**并**logfile2.etl** ，并创建转储文件**logdump.xml**以 XML 格式。  
    ```
    tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
    ```  
-   此示例将创建基于事件日志的报表**logfile.etl**，创建转储文件**logdmp.xml**以 XML 格式，使用最大努力来标识事件不在架构中，将生成一个摘要报告文件**logdump.txt**，并生成报告文件**logrpt.xml**。  
    ```
    tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
    ```  
-   此示例使用两个事件日志**logfile1.etl**并**logfile2.etl**以生成一个转储文件和报表文件的默认文件名。  
    ```
    tracerpt logfile1.etl logfile2.etl -o -report
    ```  
-   此示例使用事件日志**logfile.etl**和性能日志**counterfile.blg**以生成报表文件**logrpt.xml**和特定于 Microsoft 的 XML 架构文件**schema.xml**。  
    ```
    tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
    ```  
-   此示例读取实时事件跟踪会话"NT 内核记录器"，并生成转储文件**logfile.csv**以 CSV 格式。  
    ```
    tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
    ```