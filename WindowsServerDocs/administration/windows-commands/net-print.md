---
title: Net print
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441a61756869442fb91d26bfacc64bbeb8b902f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826008"
---
# <a name="net-print"></a>Net print

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示有关指定的打印机队列或指定的打印作业的信息或控件指定打印作业。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)本文档的部分。
> [!NOTE]
> 此命令在 Windows 7 和 Windows Server 2008 R2 中已弃用。 但是，您可以执行许多相同的任务使用 prnjobs、 Windows Management Instrumentation (WMI) 或 Windows PowerShell cmdlet。 有关详细信息，请参阅[prnjobs](prnjobs.md)， [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991)， [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)， 和 [TechNet 脚本中心库](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635)。
## <a name="syntax"></a>语法
```
Net print {\\<computerName>\<Sharename> | 
\\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
```
## <a name="parameters"></a>Parameters
|Parameters|描述|
|-------|--------|
|\\\\<computerName>\\<Sharename>|（按名称） 中指定想要显示的信息的计算机和打印队列。|
|\\\\<computerName>|（按名称） 中指定你想要控制其打印作业的计算机。 如果不指定计算机，则假定本地计算机。 需要<JobNumber>参数。|
|<JobNumber>|指定你想要控制的打印作业数。 此数字由承载其中发送打印作业的打印队列的计算机分配。 后一台计算机将数字分配给打印作业数未分配给该计算机承载任何队列中的任何其他打印作业。 使用时所需\\ \\ <computerName>参数。|
|[/hold &#124; /release &#124; /delete]|指定要与打印作业执行的操作。<br /><br />- **/保存**参数会延迟打印作业，允许发布后将其忽略其他打印作业。<br />- **/发布**参数释放已延迟打印作业。<br />- **/Delete**参数从打印队列中删除打印作业。<br /><br />如果指定的作业数，但未指定任何操作，将显示有关打印作业的信息。|
|help|显示的帮助**Net 打印**命令。|
## <a name="remarks"></a>备注
-   **Net 打印** \\ \\ <computerName>共享的打印机队列中将显示有关打印作业的信息。 以下是报表的对所有打印作业在队列中名为激光打印机的共享打印机的示例：
    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
       USER1          84        93844      printing
       USER2          85        12555      Waiting
       USER3          86        10222      Waiting
    ```
-   以下是报表的打印作业的示例：
    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```
## <a name="BKMK_examples"></a>示例
此示例演示了如何列出 Dotmatrix 打印队列的内容上\\\Production 计算机：
```
Net print \\Production\Dotmatrix 
```
此示例演示如何显示有关作业编号 35 上信息\\\Production 计算机：
```
Net print \\Production 35 
```
此示例演示如何延迟上的作业数 263 \\\Production 计算机：
```
Net print \\Production 263 /hold 
```
此示例演示如何在发布作业编号 263 \\\Production 计算机：
```
Net print \\Production 263 /release 
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
