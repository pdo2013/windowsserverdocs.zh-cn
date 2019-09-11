---
title: nslookup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84e3e9ee920f458ca775dd7b76d892f10ba2f992
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878114"
---
# <a name="nslookup"></a>nslookup

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示可用于诊断域名系统（DNS）基础结构的信息。 使用此工具之前，应熟悉 DNS 的工作原理。 只有安装了 TCP/IP 协议，才能使用 nslookup 命令行工具。
## <a name="syntax"></a>语法

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

## <a name="parameters"></a>Parameters

|                       参数                       |                                                                                                         描述                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [nslookup exit 命令](nslookup-exit-command.md)   |                                                                                                     退出**nslookup**。                                                                                                     |
| [nslookup finger 命令](nslookup-finger-command.md) |                                                                                  与当前计算机上的 finger 服务器连接。                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    显示**nslookup**子命令的简短摘要。                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             列出 DNS 域的信息。                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   将默认服务器更改为指定的 DNS 域。                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     将默认服务器更改为 DNS 域名空间的根服务器的服务器。                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   将默认服务器更改为指定的 DNS 域。                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              更改影响查找功能的配置设置。                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  打印配置设置的当前值。                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     更改查询类。 类指定信息的协议组。                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     启用或禁用穷举调试模式。 打印每个数据包的所有字段。                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               启用或禁用调试模式。                                                                                               |
|                 nslookup/set bre-walkthrough-defname                 |                                            向单个组件查找请求追加默认 DNS 域名。 单个组件是不包含句点的组件。                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 将默认 DNS 域名改为指定的名称。                                                                                  |
|                 nslookup/set ignore                  |                                                                                              忽略数据包截断错误。                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          将默认的 TCP/UDP DNS 名称服务器端口更改为指定值。                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       更改查询的资源记录类型。                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    告诉 DNS 名称服务器查询其他服务器（如果没有此信息）。                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 设置重试次数。                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    更改用于查询的根服务器的名称。                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | 向请求追加 dns 域搜索列表中的 DNS 域名，直到收到答案。 这适用于以下情况：集和查找请求至少包含一个句点，但不以尾随句点结束。 |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    更改默认 DNS 域名和搜索列表。                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           更改等待答复请求的初始秒数。                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       更改查询的资源记录类型。                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     指定在向服务器发送请求时使用或不使用虚拟线路。                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          排序并列出上一**ls**子命令或命令的输出。                                                                          |

## <a name="remarks"></a>备注
- 如果*computerTofind*是 IP 地址，并且查询用于 A 或 PTR 资源记录类型，则返回该计算机的名称。 如果*computerTofind*是一个名称并且没有尾随句点，则会将默认 DNS 域名追加到该名称。 此行为取决于以下**set**子命令的状态： **domain**、 **srchlist**、 **bre-walkthrough-defname**和**search**。
- 如果键入连字符（-）而不是*computerTofind*，则命令提示符将更改为**nslookup**交互模式。
- 命令行长度必须小于256个字符。
- **nslookup**有两种模式：交互式和非交互式。
  如果只需要查找单个数据块，请使用非交互模式。 对于第一个参数，键入要查找的计算机的名称或 IP 地址。 对于第二个参数，请键入 DNS 名称服务器的名称或 IP 地址。 如果省略第二个参数，则**nslookup**将使用默认 DNS 名称服务器。
  如果需要查找多个数据片段，可以使用交互模式。 为第一个参数键入连字符（-），为第二个参数键入 DNS 名称服务器的名称或 IP 地址。 或者省略参数， **nslookup**使用默认 DNS 名称服务器。 下面是有关在交互模式下工作的一些提示：
  -   若要在任何时候中断交互式命令，请按 CTRL + B。
  -   若要退出，请键入**exit**。
  -   若要将内置命令视为计算机名称，请在该命令前面加上转义符（\\）。
  -   无法识别的命令被解释为计算机名。
- 如果查找请求失败， **nslookup**将打印一条错误消息。 下表列出了可能的错误消息。
  |**错误消息**|**说明**|
  |-----------|----------|
  |`timed out`|服务器在一段时间内未响应请求，并且有一定次数的重试。 你可以设置超时时间和**设置超时**子命令。 可以用**set retry**子命令设置重试次数。|
  |`No response from server`|服务器计算机上没有运行任何 DNS 名称服务器。|
  |`No records`|DNS 名称服务器没有计算机的当前查询类型的资源记录，但计算机名称有效。 查询类型是通过**set querytype**命令指定的。|
  |`Nonexistent domain`|计算机或 DNS 域名不存在。|
  |`Connection refused`<br /><br />-或-<br /><br />`Network is unreachable`|无法连接到 DNS 名称服务器或 finger 服务器。 此错误通常发生在**ls**和**finger**请求上。|
  |`Server failure`|DNS 名称服务器在其数据库中发现内部不一致，无法返回有效的答案。|
  |`Refused`|DNS 名称服务器拒绝为请求服务。|
  |`format error`|DNS 名称服务器发现请求数据包的格式不正确。 这可能表示**nslookup**中存在错误。|
- 有关**nslookup**命令和 DNS 的详细信息，请参阅以下资源：
  - 先生，T.，Davies，J。2000。 *Microsoft Windows 2000 Tcp/ip 协议和服务技术参考*。 华盛顿州雷蒙德：Microsoft 新闻。
  - Albitz、Loukides、Liu 和。 2001。 *DNS 和绑定，第四版*。 Sebastopol，加利福尼亚州：O'Reilly 和联合会，Inc。
  - Larson、Liu。 2001。 *Windows 2000 上的 DNS*。 Sebastopol，加利福尼亚州：O'Reilly 和联合会，Inc。
    #### <a name="examples"></a>示例
    每个命令行选项都包含一个连字符（-），后跟命令名称和（在某些情况下为等号（=），然后是一个值）。 例如，若要将默认查询类型更改为主机（计算机）信息并且初始超时值为10秒，请键入： **nslookup-querytype = hinfo-timeout = 10**
    ## <a name="see-also"></a>请参阅
    [命令行语法项](command-line-syntax-key.md)
