---
title: shutdown
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8a5170fa214d4ed639ff3b817cf949a9f44ebd6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383895"
---
# <a name="shutdown"></a>shutdown



可让你一次关闭或重启一台本地或远程计算机。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|i|显示 "**远程关机" 对话框**。 **/I**选项必须是命令后面的第一个参数。 如果指定为 **/i** ，则忽略所有其他选项。|
|/l|立即注销当前用户，没有超时期限。 不能将 **/l**与 **/m**或 **/t**一起使用。|
|/s|关闭计算机。|
|/r|关闭后重新启动计算机。|
|/a|中止系统关闭。 仅在超时期限内有效。 若要使用 **/a**，还必须使用 **/m**选项。|
|/p|仅关闭本地计算机（不是远程计算机），无超时期限或警告。 只能将 **/p**与 **/d**或 **/f**一起使用。 如果你的计算机不支持电源关闭功能，则在你使用 **/p**时，它将关闭，但计算机的电源将保持打开状态。|
|/h|如果启用了休眠，则将本地计算机置于休眠状态。 只能将 **/h**与 **/f**一起使用。|
|/e|使您能够记录目标计算机上意外关闭的原因。|
|/f|强制关闭正在运行的应用程序，而不发出警告用户。</br>注意：使用 **/f**选项可能会导致丢失未保存的数据。|
|/m \\ @ no__t-1 @ no__t-2ComputerName >|指定目标计算机。 不能与 **/l**选项一起使用。|
|/t \<XXX >|设置重新启动或关机之前的超时时间或延迟时间为*XXX*秒。 这会导致在本地控制台上显示警告。 可以指定0-600 秒。 如果不使用 **/t**，则默认情况下超时期限为30秒。|
|/d [p @ no__t-0u：] \<XX >： \<YY >|列出系统重新启动或关机的原因。 以下是参数值：</br>**p**表示计划重新启动或关闭。</br>**u**指示原因是用户定义的。</br>注意:如果未指定**p**或**u** ，则重新启动或关机是未计划的。</br>*XX*指定主要原因号（小于256的正整数）。</br>*YY*指定次要原因号（小于65536的正整数）。|
|/c "\<Comment >"|让你可以对关闭原因作详细注释。 必须首先使用 **/d**选项提供原因。 必须用引号将注释引起来。 最多可使用 511 个字符。|
|/?|在命令提示符下显示帮助，其中包含在本地计算机上定义的主要原因和次要原因的列表。|

## <a name="remarks"></a>备注

-   必须为用户分配 "**关闭系统**用户" 权限，以便关闭使用**shutdown**命令的本地或远程管理的计算机。
-   用户必须是 Administrators 组的成员，才能批注本地或远程管理的计算机意外关闭。 如果目标计算机已加入域，则 Domain Admins 组的成员也许能够执行此过程。 有关详细信息，请参阅：  
    -   [默认本地组](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [默认组](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   如果希望一次关闭多台计算机，则可以使用脚本为每台计算机调用 "**关闭**"，也可以使用**shutdown** **/I**来显示 "远程关机" 对话框。
-   如果指定主要和次要原因代码，则必须先在计划使用原因的每台计算机上定义这些原因代码。 如果目标计算机上未定义原因代码，则关闭事件跟踪程序无法记录正确的原因文本。
-   请记住，使用**p：** 参数指示关闭已计划。 省略**p：** 指示关闭是不计划的。 如果键入**p：** ，后跟计划外关机的原因代码，则该命令不会执行关闭。 相反，如果省略**p：** 并键入计划关闭的原因代码，则该命令不会执行关闭。

## <a name="BKMK_examples"></a>示例

在一分钟的延迟后，强制应用程序关闭并重新启动本地计算机，原因如下： "应用程序：维护（已计划） "和注释" 重新配置 myapp "类型：
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
若要重新启动远程计算机 \\ @ no__t-1ServerName 具有相同的参数，请键入：
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
