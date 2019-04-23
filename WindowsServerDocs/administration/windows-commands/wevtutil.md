---
title: wevtutil
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6d57f95379fce80bec9cb5e8445b28f887123c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826728"
---
# <a name="wevtutil"></a>wevtutil



可让你检索有关事件日志和发布服务器的信息。 此外，还可以使用此命令来安装和卸载事件清单，运行查询，以及导出、存档和清除日志。 有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]] 
[{ep | enum-publishers}] 
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>] 
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]] 
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]] 
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]] 
[{al | archive-log} <Logpath> [/l:<Locale>]] 
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|{el\|枚举日志}|显示所有日志的名称。|
|{gl \| get 日志} \<Logname > [/ f:\<格式 >]|显示指定的日志，其中包括是否已启用在日志、 日志和路径的当前最大大小限制的配置信息存储在日志文件。|
|{sl\|集日志} \<Logname > [/ e:\<已启用 >] [/ 实现：\<隔离 >] [/ lfn:\<Logpath >] [/ rt:\<保留 >] [/ ab:\<自动 >] [/ ms:\<最大大小 >] [/ l:\<级别 >] [/ k:\<关键字 >] [/ ca:\<通道 >] [/ c:\<配置 >]|修改指定的日志配置。|
|{ep\|枚举发布者}|显示本地计算机上的事件发布者。|
|{gp\|获取发布服务器} \<Publishername > [/ ge:\<元数据 >] [/ gm:\<消息 >] [/ f:\<格式 >]]|显示指定的事件发布服务器的配置信息。|
|{im\|安装清单}\<清单 >|通过清单安装事件发布者和日志。 有关事件清单和使用此参数的详细信息，请参阅在 Microsoft 开发人员网络 (MSDN) 网站上的 Windows 事件日志 SDK (https://msdn.microsoft.com)。|
|{um\|卸载清单}\<清单 >|从清单中卸载所有发布服务器和日志。 有关事件清单和使用此参数的详细信息，请参阅在 Microsoft 开发人员网络 (MSDN) 网站上的 Windows 事件日志 SDK (https://msdn.microsoft.com)。|
|{qe\|查询事件}\<路径 > [/ lf:\<日志文件 >] [/ sq:\<Structquery >] [/ 问：\<查询 >] [/ bm:\<书签 >] [/ sbm:\<Savebm >] [/ rd:\<方向 >] [/ f:\<格式 >] [/ l:\<区域设置 >] [/ c:\<计数 >] [/ e:\<元素 >]|从事件日志，从日志文件，或使用结构化的查询读取事件。 默认情况下提供的日志名称\<路径 >。 但是，如果您使用 **/lf**选项，然后\<路径 > 必须为日志文件的路径。 如果您使用 **/sq**参数，\<路径 > 必须为包含结构化的查询的文件路径。|
|{gli \| get loginfo} \<Logname > [/ lf:\<日志文件 >]|显示有关事件日志或日志文件的状态信息。 如果 **/lf**使用选项，则\<Logname > 是日志文件的路径。 你可以运行**wevtutil el**获取日志名称的列表。|
|{epl\|导出日志}\<路径 > \<Exportfile > [/ lf:\<日志文件 >] [/ sq:\<Structquery >] [/ 问：\<查询 >] [/ ow:\<覆盖 >]|将事件导出从事件日志，从日志文件，或使用指定的文件的结构化的查询。 默认情况下提供的日志名称\<路径 >。 但是，如果您使用 **/lf**选项，然后\<路径 > 必须为日志文件的路径。 如果您使用 **/sq**选项，\<路径 > 必须为包含结构化的查询的文件路径。 \<Exportfile > 是指向将存储导出的事件的文件的路径。|
|{al\|存档日志} \<Logpath > [/ l:\<区域设置 >]|将指定的日志文件以自包含格式进行存档。 创建的区域设置名称的子目录和所有特定于区域设置信息保存在该子目录中。 目录和日志文件创建的运行后**wevtutil al**，可以读取文件中的事件，是否安装了发布服务器。|
|{cl\|清除日志} \<Logname > [/ bu:\<备份 >]|清除指定的事件日志中的事件。 **/Bu**选项可用于备份清除的事件。|

## <a name="options"></a>选项

|Option|描述|
|------|-----------|
|/f:\<格式 >|指定输出应为 XML 或文本格式。 如果\<格式 > 为 XML，以 XML 格式显示输出。 如果\<格式 > 是文本，而无需 XML 标记显示的输出。 默认值为文本。|
|/e:\<已启用 >|启用或禁用日志。 \<启用 > 可以是 true 或 false。|
|/i:\<隔离 >|设置日志隔离模式。 \<隔离 > 可以是系统中，应用程序或自定义。 日志的隔离模式确定日志是否与同一隔离类中的其他日志共享会话。 如果指定系统隔离，将共享目标日志至少写入系统日志的权限。 如果指定应用程序隔离，将共享目标日志至少编写的应用程序日志的权限。 如果指定自定义的隔离，还必须通过使用提供的安全描述符 **/ca**选项。|
|/lfn:\<Logpath >|定义日志文件名称。 \<Logpath > 是事件日志服务存储此日志的事件的位置的文件的完整路径。|
|/rt:\<保留 >|设置日志保留模式。 \<保留期 > 可以是 true 或 false。 日志保留模式确定事件日志服务的行为，当日志达到其最大大小。 如果事件日志达到其最大大小时，且日志保留模式为 true，保留现有的事件，并传入事件将被丢弃。 如果日志保留模式为 false，则传入事件覆盖最早的事件日志中。|
|/ab:\<自动 >|指定日志自动备份策略。 \<自动 > 可以是 true 或 false。 如果此值为 true，日志将进行自动备份时达到最大大小。 如果此值为 true，将保留期 (与指定 **/rt**选项)，还必须设置为 true。|
|/ms:\<MaxSize>|以字节为单位设置日志的最大大小。 最小日志大小为 1048576 个字节 (1024 KB) 和日志文件始终是 64 KB 的倍数，因此输入的值将被舍入相应地。|
|/l:\<级别 >|定义日志级别筛选的器。 \<级别 > 可以是任何有效的级别值。 此选项才适用于具有专用会话日志。 可以通过设置删除级别筛选器<Level>为 0。|
|/k:\<关键字 >|指定的日志的关键字筛选器。 \<关键字 > 可以是任何有效的 64 位的关键字掩码。 此选项才适用于具有专用会话日志。|
|/ca:\<通道 >|设置事件日志的访问权限。 \<通道 > 是使用安全描述符定义语言 (SDDL) 的安全描述符。 有关 SDDL 格式的详细信息，请参阅 Microsoft 开发人员网络 (MSDN) 网站 (https://msdn.microsoft.com)。|
|无\<配置 >|指定配置文件的路径。 此选项将导致日志属性从配置文件中定义要读取\<配置 >。 如果您使用此选项，您必须指定<Logname>参数。 将从配置文件读取的日志名称。|
|/ge:\<元数据 >|获取此发布服务器可以引发的事件的元数据信息。 \<元数据 > 可以是 true 或 false。|
|/gm:\<消息 >|显示实际的消息，而不是数字的消息 id。 \<消息 > 可以是 true 或 false。|
|/lf:\<日志文件 >|指定应从日志中或从日志文件时读取的事件。 \<日志文件 > 可以是 true 或 false。 如果为 true，该命令的参数是日志文件的路径。|
|/ sq:\<Structquery >|指定应使用结构化查询获得的事件。 \<Structquery > 可以是 true 或 false。 如果为 true，<Path>是包含结构化的查询的文件的路径。|
|/q:\<查询 >|定义 XPath 查询以筛选读取或导出的事件。 如果未指定此选项，则将返回所有事件，或将其导出。 此选项不可用 **/sq**为 true。|
|/bm:\<书签 >|指定包含书签从上一个查询的文件的路径。|
|/sbm:\<Savebm >|指定用于保存此查询的书签的文件的路径。 文件扩展名应为.xml。|
|/rd:\<方向 >|指定读取事件的方向。 \<方向 > 可以是 true 或 false。 如果为 true，首先返回最新的事件。|
|/l:\<区域设置 >|定义用于打印事件文本中的特定区域设置的区域设置字符串。 仅当打印文本格式使用中的事件时可用 **/f**选项。|
|无\<计数 >|设置要读取的事件的最大数目。|
|/e:\<元素 >|在 XML 中显示的事件时，请包括根元素。 \<元素 > 是所需的根元素中的字符串。 例如， **/e:root**将导致 XML 包含根元素对\<根 ></root>。|
|/ ow:\<覆盖 >|指定应覆盖该导出文件。 \<覆盖 > 可以是 true 或 false。 如果 true，并导出文件中指定<Exportfile>已存在，它将被覆盖而无需确认。|
|/bu:\<备份 >|指定将在其中存储已清除的事件的文件的路径。 包含扩展名为.evtx 扩展名的备份文件的名称。|
|/r:\<远程 >|在远程计算机上运行该命令。 \<远程 > 是的远程计算机的名称。 **Im**并**um**参数不支持远程操作。|
|带 /u:\<用户名 >|指定不同的用户，登录到远程计算机上。 \<用户名 > 是在窗体域 \ 用户或用户的用户名。 此选项才适用 **/r**指定选项。|
|/p:\<密码 >|指定用户的密码。 如果 **/u**选项，并且未指定此选项或\<密码 > 是"*"，将提示用户输入密码。 此选项才适用 **/u**指定选项。|
|/a:\<身份验证 >|定义用于连接到远程计算机的身份验证类型。 \<身份验证 > 可以是默认、 协商、 Kerberos 或 NTLM。 默认值为协商。|
|/uni:\<Unicode >|以 unicode 格式显示输出。 \<Unicode > 可以是 true 或 false。 如果<Unicode>为 true，则输出采用 Unicode。|

## <a name="remarks"></a>备注

-   Sl 参数中使用配置文件

    配置文件是 XML 文件作为 wevtutil gl 的输出相同的格式和\<Logname > /f:xml。 下面的示例演示配置文件，使保持期、 启用自动备份，并设置上的应用程序日志的最大日志大小的格式：  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>示例

列出所有日志的名称：
```
wevtutil el
```
以 XML 格式在本地计算机上显示有关系统日志的配置信息：
```
wevtutil gl System /f:xml
```
使用配置文件将设置事件日志属性 （请参阅备注有关的配置文件示例）：
```
wevtutil sl /c:config.xml
```
显示有关 Microsoft Windows Eventlog 事件发布者，包括发布服务器可以引发的事件有关的元数据的信息：
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
MyManifest.xml 清单文件从安装发布服务器和日志：
```
wevtutil im myManifest.xml
```
卸载发布服务器和日志从 myManifest.xml 清单文件：
```
wevtutil um myManifest.xml
```
文本格式显示应用程序日志中的三个最新事件：
```
wevtutil qe Application /c:3 /rd:true /f:text
```
显示应用程序日志的状态：
```
wevtutil gli Application 
```
将事件从系统日志导出到 C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
清除所有应用程序日志中的事件保存到 C:\admin\backups\a10306.evtx 后：
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
