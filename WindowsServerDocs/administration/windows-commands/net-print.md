---
title: Net print
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 241f9d74cb537924cf69c1e0bb5fd73a422c4b23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373299"
---
# <a name="net-print"></a>Net print

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关指定打印机队列或指定的打印作业的信息，或控制指定的打印作业。
有关如何使用此命令的示例，请参阅本文档的 "[示例](#BKMK_examples)" 部分。
> [!NOTE]
> 此命令已在 Windows 7 和 Windows Server 2008 R2 中弃用。 但是，可以使用 prnjobs、Windows Management Instrumentation （WMI）或 Windows PowerShell cmdlet 执行许多相同的任务。 有关详细信息，请参阅[prnjobs](prnjobs.md)， [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991) ， [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426) ， 和 [TechNet 脚本中心库](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635) 。
> ## <a name="syntax"></a>语法
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ## <a name="parameters"></a>Parameters
> 
> |               Parameters               |                                                                                                                                                                                                                     描述                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            按名称指定要显示其信息的计算机和打印队列。                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 指定（按名称）承载要控制的打印作业的计算机。 如果未指定计算机，则假定为本地计算机。 需要 @no__t 参数。                                                                                                                                  |
> |              <JobNumber>               |                                             指定要控制的打印作业的编号。 此编号由承载打印作业的打印队列的计算机分配。 计算机将编号分配给打印作业后，该数字不会分配给该计算机所承载的任何队列中的任何其他打印作业。 使用 \\ @ no__t-1 @ no__t-2 参数时是必需的。                                             |
> | [/hold &#124; /release &#124; /delete] | 指定要对打印作业执行的操作。<br /><br />- **/Hold**参数将延迟作业，允许其他打印作业绕过它。<br />- **/Release**参数将释放已延迟的打印作业。<br />- **/Delete**参数从打印队列中删除打印作业。<br /><br />如果指定了作业编号，但未指定任何操作，则会显示有关打印作业的信息。 |
> |                  help                  |                                                                                                                                                                                                     显示**Net print**命令的帮助。                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>备注
> - **Net print** \\ @ no__t-2 @ no__t 显示有关共享打印机队列中打印作业的信息。 下面的示例显示了名为激光器的共享打印机的队列中所有打印作业的报表：
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - 下面是打印作业报表的示例：
>   ```
>   Job #            35
>   Status           Waiting
>   Size             3096
>   remark
>   Submitting user  USER2
>   Notify           USER2
>   Job data type
>   Job parameters
>   additional info
>   ```
>   ## <a name="BKMK_examples"></a>示例
>   此示例演示如何在 \\ \ 生产计算机上列出 Dotmatrix 打印队列的内容：
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   此示例显示了如何显示 \\ \ 生产计算机上的作业编号35的相关信息：
>   ```
>   Net print \\Production 35 
>   ```
>   此示例显示了如何在 \\ \ 生产计算机上延迟作业编号263：
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   此示例演示如何在 \\ \ 生产计算机上发布作业编号263：
>   ```
>   Net print \\Production 263 /release 
>   ```
>   #### <a name="additional-references"></a>其他参考
>   [命令行语法关键字](command-line-syntax-key.md)
>   [打印命令参考](print-command-reference.md)
