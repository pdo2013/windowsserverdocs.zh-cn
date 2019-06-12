---
title: typeperf
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfcbac82b88c0c8d8bcc706ebfd807f96e359de7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440785"
---
# <a name="typeperf"></a>typeperf



**Typeperf**命令将性能数据写入命令窗口或日志文件。 若要停止**typeperf**，按 CTRL + C。

有关如何使用的示例**typeperf**，请参阅[示例](#BKMK_EXAMPLES)。

## <a name="syntax"></a>语法

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<计数器 [计数器 [...]]>|指定要监视的性能计数器。|

> [!NOTE]
> **\<计数器 >** 是中的性能计数器的完整名称 *\\ \\Computer\Object （实例） \Counter*格式，例如 **\\ \\Server1\Processor(0)\%用户时间**。

## <a name="options"></a>选项

|                   Option                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               显示上下文相关帮助。                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL> |                                    指定输出文件格式。 默认值为 CSV。                                     |
|              -cf \<filename>               |              指定包含要使用每行一个计数器监视性能计数器的列表的文件。               |
|             -si < [[hh:] mm:] ss >             |                                  指定采样间隔。 默认值为 1 秒。                                   |
|               -o \<filename>               |     指定输出文件或 SQL 数据库的路径。 默认值为 STDOUT （写入到命令窗口）。      |
|                -q [object]                 | 显示已安装的计数器 （没有实例） 的列表。 若要列出一个对象的计数器，包括对象名称。 \*\*\*示例 |
|                -qx [object]                |        显示已安装实例的计数器的列表。 若要列出一个对象的计数器，包括对象名称。        |
|               -sc\<示例 >               |             指定要收集样本的数。 默认值是收集数据，直到按下 CTRL + C。              |
|            -config \<filename>             |                                    指定包含命令选项的设置文件。                                     |
|            -s \<computer_name>             |                   指定要监视的计算机指定计数器路径中的远程计算机。                    |
|                     -y                     |                                        回答是对所有问题而不提示。                                        |

## <a name="BKMK_EXAMPLES"></a>示例

- 下面的示例将值写入本地计算机的性能计数器 **\\ \\processor （_total)\%处理器时间百分比**到命令窗口在默认的示例时间间隔为 1 秒直到按下 CTRL + C 为止。  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- 下面的示例文件中写入的计数器列表的值**counters.txt**制表符分隔文件**domain2.tsv**在 5 秒钟，直到收集了 50 的样本的采样间隔。  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- 下面的示例查询安装计数器计数器对象的实例，并用**PhysicalDisk**并将所得到的列表写入到文件**counters.txt**。  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
