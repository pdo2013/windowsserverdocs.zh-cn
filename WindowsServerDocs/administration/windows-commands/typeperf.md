---
title: typeperf
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "66440785"
---
# <a name="typeperf"></a>typeperf



**Typeperf**命令将性能数据写入命令窗口或日志文件。 若要停止**typeperf**, 请按 CTRL + C。

有关如何使用**typeperf**的示例, 请参阅[示例](#BKMK_EXAMPLES)。

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
|\<计数器 [counter [...]]>|指定要监视的性能计数器。|

> [!NOTE]
> counter > 是 *\\Computer\Object (实例) \Counter \\* 格式 **\<**  **\\ (例如\\Server1\Processor (0))\%中性能计数器的完整名称用户时间**。

## <a name="options"></a>选项

|                   Option                   |                                                         描述                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               显示区分上下文的帮助。                                               |
| -f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL > |                                    指定输出文件格式。 默认值为 CSV。                                     |
|              -cf \<filename >               |              指定一个文件, 其中包含要监视的性能计数器的列表, 每行一个计数器。               |
|             -si < [[hh:] mm:] ss >             |                                  指定采样间隔。 默认值为1秒。                                   |
|               -o \<filename >               |     指定输出文件或 SQL 数据库的路径。 默认值为 STDOUT (写入到 "命令" 窗口)。      |
|                -q [对象]                 | 显示已安装的计数器 (无实例) 的列表。 若要列出一个对象的计数器, 请包括对象名称。 \*\*\*实例 |
|                -qx [对象]                |        显示包含实例的已安装计数器列表。 若要列出一个对象的计数器, 请包括对象名称。        |
|               -sc \<示例 >               |             指定要收集的样本数。 默认情况下, 在按下 CTRL + C 之前, 将收集数据。              |
|            -config \<filename >             |                                    指定包含命令选项的设置文件。                                     |
|            -s \<程序名 >             |                   指定要监视的远程计算机是否在计数器路径中指定了计算机。                    |
|                     -y                     |                                        在不提示的情况下回答 "是"。                                        |

## <a name="BKMK_EXAMPLES"></a>示例

- 下面的示例将本地计算机的性能计数器 **\\ \\处理器 (_total\% ) 处理器时间**的值写入到命令窗口, 默认示例间隔为1秒, 直到按下 CTRL + C.  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- 下面的示例将文件**计数器**中的计数器列表的值写入到以5秒为单位的制表符分隔的**2** , 直到收集到50个样本。  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- 下面的示例查询计数器对象**PhysicalDisk**的已安装计数器, 并将生成的列表写入文件**计数器 .txt**。  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
