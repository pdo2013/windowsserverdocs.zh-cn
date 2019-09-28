---
title: wevtutil
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21f3b721a2a08a7fa101ec09f1f11b5e984f0113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362175"
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
|{el \| 枚举-日志}|显示所有日志的名称。|
|{gl \| 获取日志} \<Logname > [/f： \<Format >]|显示指定日志的配置信息，其中包括日志是否已启用、日志的当前最大大小限制以及日志所存储到文件的路径。|
|{sl \| set-log} \<Logname > [/e： \<Enabled >] [/i： \<Isolation >] [4Logpath：/rt @no__t] [5Retention： >-/ab @no__t] [6Auto： >-/ms @no__t] [/l： >-7MaxSize @no__t] [： >9Keywords >] [/ca： @no__t 10Channel >] [/c： 1Config >]|修改指定日志的配置。|
|{ep \| 枚举-发布服务器}|显示本地计算机上的事件发布者。|
|{gp \| get-publisher} \<Publishername > [/ge： \<Metadata >] [/gm： \<Message >] [/f： \<Format >]]|显示指定事件发布者的配置信息。|
|{im \| 安装清单} \<Manifest >|从清单安装事件发布者和日志。 有关事件清单和使用此参数的详细信息，请参阅 Microsoft 开发人员网络（MSDN）网站上的 Windows 事件日志 SDK （[https://msdn.microsoft.com](https://msdn.microsoft.com)）。|
|{um \| 卸载清单} \<Manifest >|从清单中卸载所有发布服务器和日志。 有关事件清单和使用此参数的详细信息，请参阅 Microsoft 开发人员网络（MSDN）网站上的 Windows 事件日志 SDK （[https://msdn.microsoft.com](https://msdn.microsoft.com)）。|
|{qe \| query-events} \<Path > [/lf： \<Logfile >] [/sq： 3Structquery @no__t] [/q： >-4Query @no__t] [/bm： 5Bookmark >] [/sbm： 6Savebm @no__t] [/f： >-/Rd @no__t] [/f： >_t-9Locale >] [/c： 0Count >] [/e： 1Element >]|从事件日志、日志文件或使用结构化查询读取事件。 默认情况下，提供 \<Path > 的日志名称。 但是，如果使用 **/lf**选项，则 > \<Path 必须是日志文件的路径。 如果使用 **/sq**参数，则 > \<Path 必须是包含结构化查询的文件的路径。|
|{gli \| loginfo} \<Logname > [/lf： \<Logfile >]|显示有关事件日志或日志文件的状态信息。 如果使用了 **/lf**选项，则 > \<Logname 是日志文件的路径。 可以运行**wevtutil el**获取日志名称列表。|
|{epl \| export-log} \<Path > \<Exportfile > [/lf： \<Logfile >] [/sq： \<Structquery >] [/q： \<Query >] [/ow： 6Overwrite @no__t]|从事件日志、日志文件或使用结构化查询从事件日志中导出事件到指定的文件。 默认情况下，提供 \<Path > 的日志名称。 但是，如果使用 **/lf**选项，则 > \<Path 必须是日志文件的路径。 如果使用 **/sq**选项，> \<Path 必须是包含结构化查询的文件的路径。 \<Exportfile > 是指将在其中存储导出事件的文件的路径。|
|{al \| archive-log} \<Logpath > [/l： \<Locale >]|以自包含格式存档指定的日志文件。 将创建一个子目录，其中包含区域设置的名称，并将所有特定于区域设置的信息保存在该子目录中。 通过运行**wevtutil al**创建目录和日志文件之后，无论是否安装了发布服务器，都可以读取文件中的事件。|
|{cl \| clear-log} \<Logname > [/bu： \<Backup >]|从指定的事件日志中清除事件。 可以使用 **/bu**选项来备份已清除的事件。|

## <a name="options"></a>选项

|       Option       |                                                                                                                                                                                                                                                                 描述                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f： \<Format >    |                                                                                                                                                               指定输出应为 XML 格式或文本格式。 如果 @no__t 0Format > 为 XML，则输出以 XML 格式显示。 如果 @no__t > 是文本，则显示不带 XML 标记的输出。 默认值为 Text。                                                                                                                                                                |
|   /e： \<Enabled >    |                                                                                                                                                                                                                                         启用或禁用日志。 \<Enabled > 可以为 true 或 false。                                                                                                                                                                                                                                          |
|  /i： \<Isolation >   | 设置日志隔离模式。 \<Isolation > 可以是系统、应用程序或自定义的。 日志的隔离模式确定日志是否与同一隔离类中的其他日志共享会话。 如果指定系统隔离，则目标日志将至少与系统日志共享写入权限。 如果指定应用程序隔离，则目标日志将至少与应用程序日志共享写入权限。 如果指定自定义隔离，还必须使用 **/ca**选项提供安全描述符。 |
|  /lfn： \<Logpath >   |                                                                                                                                                                                                           定义日志文件名。 \<Logpath > 是事件日志服务用于存储此日志事件的文件的完整路径。                                                                                                                                                                                                           |
|  /rt： \<Retention >  |                                                            设置日志保持模式。 \<Retention > 可以为 true 或 false。 日志保留模式决定了日志达到其最大大小时事件日志服务的行为。 如果事件日志达到其最大大小，并且日志保留模式为 true，则会保留现有事件，并放弃传入事件。 如果日志保留模式为 false，则传入事件将覆盖日志中最旧的事件。                                                             |
|    /ab： \<Auto >     |                                                                                                                                   指定日志自动备份策略。 \<Auto > 可以为 true 或 false。 如果此值为 true，则日志将在达到最大大小时自动备份。 如果此值为 true，则保留期（用 **/rt**选项指定）还必须设置为 true。                                                                                                                                    |
|   /ms： \<MaxSize >   |                                                                                                                                                                        设置日志的最大大小（以字节为单位）。 最小日志大小为1048576字节（1024KB），日志文件始终为64KB 的倍数，因此输入的值将相应地进行舍入。                                                                                                                                                                         |
|    /l： \<Level >     |                                                                                                                                                                     定义日志的级别筛选器。 \<Level > 可以是任何有效的级别值。 此选项仅适用于具有专用会话的日志。 可以通过将 @no__t 设置为0来删除级别筛选器。                                                                                                                                                                      |
|   /k： \<Keywords >   |                                                                                                                                                                                         指定日志的关键字筛选器。 \<Keywords > 可以是任何有效的64位关键字掩码。 此选项仅适用于具有专用会话的日志。                                                                                                                                                                                         |
|   /ca： \<Channel >   |                                                                                                                   设置事件日志的访问权限。 \<Channel > 是使用安全描述符定义语言（SDDL）的安全描述符。 有关 SDDL 格式的详细信息，请参阅 Microsoft 开发人员网络（MSDN）网站（[https://msdn.microsoft.com](https://msdn.microsoft.com)）。                                                                                                                    |
|    /c： \<Config >    |                                                                                                                                  指定配置文件的路径。 此选项将导致从 \<Config > 中定义的配置文件中读取日志属性。 如果使用此选项，则不得指定 @no__t 的参数。 日志名称将从配置文件中读取。                                                                                                                                   |
|  /ge： \<Metadata >   |                                                                                                                                                                                                                 获取此发布服务器可能引发的事件的元数据信息。 \<Metadata > 可以为 true 或 false。                                                                                                                                                                                                                 |
|   /gm： \<Message >   |                                                                                                                                                                                                                       显示实际消息，而不是数字消息 ID。 \<Message > 可以为 true 或 false。                                                                                                                                                                                                                        |
|   /lf： \<Logfile >   |                                                                                                                                                                                  指定应从日志或日志文件中读取事件。 \<Logfile > 可以为 true 或 false。 如果为 true，则命令的参数是日志文件的路径。                                                                                                                                                                                   |
| /sq： \<Structquery > |                                                                                                                                                                                指定应使用结构化查询获取事件。 \<Structquery > 可以为 true 或 false。 如果为 true，则 @no__t 为包含结构化查询的文件的路径。                                                                                                                                                                                |
|    /q： \<Query >     |                                                                                                                                                                     定义用于筛选读取或导出的事件的 XPath 查询。 如果未指定此选项，则将返回或导出所有事件。 当 **/sq**为 true 时，此选项不可用。                                                                                                                                                                     |
|  /bm： \<Bookmark >   |                                                                                                                                                                                                                                 指定包含前一个查询中的书签的文件的路径。                                                                                                                                                                                                                                 |
|   /sbm： \<Savebm >   |                                                                                                                                                                                                             指定用于保存此查询书签的文件的路径。 文件扩展名应为 .xml。                                                                                                                                                                                                              |
|  /rd： \<Direction >  |                                                                                                                                                                                                   指定读取事件的方向。 \<Direction > 可以为 true 或 false。 如果为 true，则首先返回最新的事件。                                                                                                                                                                                                   |
|    /l： \<Locale >    |                                                                                                                                                                                          定义用于在特定区域设置中打印事件文本的区域设置字符串。 仅在使用 **/f**选项打印文本格式的事件时可用。                                                                                                                                                                                          |
|    /c： \<Count >     |                                                                                                                                                                                                                                                  设置要读取的最大事件数。                                                                                                                                                                                                                                                  |
|   /e： \<Element >    |                                                                                                                                                           在 XML 中显示事件时包含根元素。 \<Element > 是根元素内所需的字符串。 例如， **/e： root**会导致包含根元素对的 XML @no__t > </root>。                                                                                                                                                            |
|  /ow： \<Overwrite >  |                                                                                                                                                                 指定应覆盖导出文件。 \<Overwrite > 可以为 true 或 false。 如果为 true，并且在 <Exportfile> 中指定的导出文件已经存在，则会在不确认的情况下覆盖该文件。                                                                                                                                                                 |
|   /bu： \<Backup >    |                                                                                                                                                                                                      指定将在其中存储已清除事件的文件的路径。 在备份文件的名称中包含 .evtx 扩展名。                                                                                                                                                                                                       |
|    /r： \<Remote >    |                                                                                                                                                                                            在远程计算机上运行命令。 \<Remote > 是远程计算机的名称。 **Im**和**um**参数不支持远程操作。                                                                                                                                                                                            |
|   /u： \<Username >   |                                                                                                                                                                          指定其他用户登录到远程计算机。 \<Username > 为 domain\user 或 user 形式的用户名。 仅当指定 **/r**选项时，此选项才适用。                                                                                                                                                                          |
|   /p： \<Password >   |                                                                                                                                               指定用户的密码。 如果使用了 **/u**选项并且未指定此选项，或 \<Password > 为 " *"，则系统会提示用户输入密码。仅当指定了 \* @ no__t/u @ no__t-2 @ no__t 选项时，此选项才适用。                                                                                                                                                |
|     /a： \<Auth >     |                                                                                                                                                                                             定义用于连接到远程计算机的身份验证类型。 \<Auth > 可以是默认值、协商、Kerberos 或 NTLM。 默认值为 "协商"。                                                                                                                                                                                              |
|  /uni： \<Unicode >   |                                                                                                                                                                                                             以 Unicode 格式显示输出。 \<Unicode > 可以为 true 或 false。 如果 @no__t 为 true，则输出为 Unicode。                                                                                                                                                                                                             |

## <a name="remarks"></a>备注

-   使用带有 sl 参数的配置文件

    配置文件是一个 XML 文件，其格式与 wevtutil gl \<Logname >/f： XML 的输出相同。 以下示例显示了配置文件的格式，该配置文件启用保留、启用 autobackup 并设置应用程序日志的最大日志大小：  
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
以 XML 格式显示有关本地计算机上的系统日志的配置信息：
```
wevtutil gl System /f:xml
```
使用配置文件设置事件日志属性（有关配置文件的示例，请参阅备注）：
```
wevtutil sl /c:config.xml
```
显示有关 Microsoft Windows-Eventlog 事件发布者的信息，包括有关发布者可以引发的事件的元数据：
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
安装 myManifest 清单文件中的发布服务器和日志：
```
wevtutil im myManifest.xml
```
从 myManifest 清单文件卸载发布服务器和日志：
```
wevtutil um myManifest.xml
```
以文本格式显示应用程序日志中三个最近的事件：
```
wevtutil qe Application /c:3 /rd:true /f:text
```
显示应用程序日志的状态：
```
wevtutil gli Application 
```
将事件从系统日志导出到 C:\backup\system0506.evtx：
```
wevtutil epl System C:\backup\system0506.evtx
```
将所有事件保存到 C:\admin\backups\a10306.evtx 后，请清除应用程序日志中的所有事件：
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
