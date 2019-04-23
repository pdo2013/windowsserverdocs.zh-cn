---
title: prncnfg
description: 了解如何使用 prncfg 命令对打印机进行配置。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859148"
---
# <a name="prncnfg"></a>prncnfg

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

配置或显示有关打印机的配置信息。

## <a name="syntax"></a>语法
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-g|显示有关打印机的配置信息。|
|-t|配置打印机。|
|-x|重命名打印机。|
|-S \<ServerName\>|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|
|-P \<printerName\>|指定你想要管理的打印机的名称。 必需。|
|-z \<NewprinterName\>|指定新的打印机名称。 需要 **-x**并 **-P**参数。|
|-u \<UserName\> -w \<Password\>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|
|-r \<PortName\>|指定打印机已连接的端口。 如果这是一种并行或串行端口，然后使用该端口 （例如，LPT1 或 COM1） 的 ID。 如果这是一个 TCP/IP 端口，使用已添加端口时指定的端口名称。|
|-l\<位置\>|指定打印机的位置，例如"复制的空间"。|
|-h\<共享名\>|指定打印机的共享名称。|
|-m\<注释\>|指定打印机的注释字符串。|
|-f \<SeparatorFileName\>|指定包含在分隔符页上显示的文本的文件。|
|-y\<数据类型\>|指定打印机可接受的数据类型。|
|-st \<starttime\>|配置可用性受限的打印机。 指定打印机是可用的时间。 如果将文档发送到打印机不可用时，文档将保存 （进行后台处理），直到打印机变得可用。 您必须指定时间为 24 小时制。 例如，若要指定晚上 11:00，请键入**2300年**。|
|-ut\<结束时间\>|配置可用性受限的打印机。 指定打印机不再可用的时间。 如果将文档发送到打印机不可用时，文档将保存 （进行后台处理），直到打印机变得可用。 您必须指定时间为 24 小时制。 例如，若要指定晚上 11:00，请键入**2300年**。|
|-o\<优先级\>|指定后台处理程序使用路由到打印队列的打印作业的优先级。 具有较高优先级的打印队列接收任何优先级较低的队列之前的所有作业。|
|-i \<DefaultPriority\>|指定分配给每个打印作业的默认优先级。|
|{+&#124;-}shared|指定是否在网络上共享该打印机。|
|{+&#124;-}direct|指定是否应将文档直接向打印机发送没有正在进行后台处理。|
|{+&#124;-}published|指定是否应在 active directory 中发布此打印机。 如果发布的打印机，其他用户可以搜索将基于其位置和功能 （例如彩色打印和装订）。|
|{+&#124;-}hidden|保留的函数。|
|{+&#124;-}rawonly|指定是否在此队列中，可以后台处理原始数据打印作业。|
|{+ &#124; -}queued|指定打印机不应以打印直到后文档的最后一页将处于假脱机。 打印程序将不可用，直到完成打印文档。 但是，使用此参数可确保整个文档可供打印机。|
|{+ &#124; -}keepprintedjobs|指定后在打印后台处理程序是否应保留的文档。 启用此选项允许用户从打印队列而不是从打印程序重新将文档提交到打印机。|
|{+ &#124; -}workoffline|指定用户是否可以将打印作业发送到打印队列，如果计算机未连接到网络。|
|{+ &#124; -}enabledevq|指定与打印机的设置 （例如 PostScript 文件假脱机保存到非 PostScript 打印机） 不匹配的打印作业是否应保留在队列中而不是打印。|
|{+ &#124; -}docompletefirst|指定后台处理程序是否应发送已完成发送具有较高优先级尚未完成后台处理的打印作业之前的后台处理优先级较低的打印作业。 如果启用此选项，且没有文档已完成后台处理，后台处理程序将在较小的之前发送较大的文档。 如果你想要最大化打印机的效率，但代价是作业优先级，应启用此选项。 如果禁用此选项，则后台处理程序始终会发送较高优先级作业到它们各自的队列第一次。|
|{+ &#124; -}enablebidi|指定打印机是否将状态信息发送到后台处理程序。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Prncnfg**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 prncnfg 文件或将目录更改为适当的文件夹的完整路径。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。

## <a name="BKMK_examples"></a>示例
若要显示 colorprinter_2 指定与远程计算机名为 HRServer 承载打印队列的打印机的配置信息，请键入：
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

若要配置名为 colorprinter_2，以便在远程计算机名为 HRServer 后台处理程序的打印作业将后将保留已打印的打印机，请键入：
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

若要更改远程计算机上的打印机名称名为 HRServer colorprinter_2 colorprinter 3，类型为：
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
