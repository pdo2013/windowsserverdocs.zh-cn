---
title: shutdown
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837248"
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
|i|显示**远程关机对话框**框。 **/I**选项必须为该命令后的第一个参数。 如果 **/i**指定，则将忽略所有其他选项。|
|/l|注销当前用户立即，没有超时期限。 不能使用 **/l**与 **/m**或 **/t**。|
|/s|关闭计算机。|
|/r|关闭后重新启动计算机。|
|/a|中止系统关闭。 仅在超时期间有效。 若要使用 **/a**，则还必须使用 **/m**选项。|
|/p|关闭仅本地计算机 （不是远程计算机）-没有超时期限或警告。 可以使用 **/p**只用 **/d**或 **/f**。 如果您的计算机不支持关闭电源的功能，它将关闭使用时 **/p**，但上仍将保留在计算机的电源。|
|/h|如果启用了休眠，进入休眠状态，将本地计算机。 可以使用 **/h**只用 **/f**。|
|/e|使您能够记录在目标计算机上的意外关闭的原因。|
|/f|强制运行应用程序关闭，且不警告用户。</br>注意：使用 **/f**选项可能会导致丢失未保存的数据。|
|/m \\ \\\<计算机名 >|指定的目标计算机。 不能用于 **/l**选项。|
|/t \<XXX>|设置超时期限或延迟*XXX*秒，然后重新启动或关机。 这将导致本地控制台上显示一条警告。 您可以指定 0-600 秒。 如果不使用 **/t**，超时期限是默认为 30 秒。|
|/d [p\|u:]\<XX>:\<YY>|列出系统重新启动或关机的原因。 以下是参数值：</br>**p**指示计划重新启动或关机。</br>**u**指示的原因是用户定义。</br>注意：如果**p**或**u**未指定，是意外的重新启动或关机。</br>*XX*指定主要原因编号 （小于 256 的正整数）。</br>*YY*指定次要原因编号 （小于 65536 的正整数）。|
|/c"\<注释 >"|让你可以对关闭原因作详细注释。 您首先必须通过提供相应原因 **/d**选项。 注释必须用引号引起来。 最多可使用 511 个字符。|
|/?|在命令提示符下，包括在本地计算机定义的主要和次要原因的列表显示帮助。|

## <a name="remarks"></a>备注

-   必须为用户分配**关闭系统**关机本地或远程的用户权限管理使用计算机**关闭**命令。
-   用户必须是 Administrators 组的成员进行批注的本地或远程管理计算机意外的关机。 如果目标计算机加入到域中，Domain Admins 组的成员可以执行此过程。 有关详细信息，请参阅：  
    -   [默认本地组](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [默认组](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   如果你想要一次关闭多台计算机，则可以调用**关机**对于每台计算机使用的脚本，或您可以使用**关闭** **/i**以显示远程关闭对话框。
-   如果指定主要和次要原因代码，您必须首先定义你打算使用原因每台计算机上的这些原因代码。 如果目标计算机上未定义的原因代码，关闭事件跟踪程序无法记录正确的原因的文本。
-   请记住以指示，通过使用计划关闭**p:** 参数。 省略**p:** 指示计划外关闭。 如果键入**p:** 跟计划外关机的原因代码，该命令不会执行关闭。 相反，如果省略**p:** 和关闭不会执行了计划内关闭，该命令，原因代码中的类型。

## <a name="BKMK_examples"></a>示例

若要强制关闭并重新在本地计算机启动，原因是在一分钟延迟后的应用程序"应用程序：维护 （计划）"和"重新配置 myapp.exe"类型的注释：
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
若要重新启动远程计算机\\\\服务器名称使用相同的参数中，键入：
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
