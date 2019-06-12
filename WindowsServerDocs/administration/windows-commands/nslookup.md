---
title: nslookup
description: 'Windows 命令主题 * * *- '
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
ms.openlocfilehash: 194cb96846e42b175978a2f6fc7268a93875d315
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811093"
---
# <a name="nslookup"></a>nslookup

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示可用于诊断域名系统 (DNS) 基础结构的信息。 使用此工具之前，应熟悉 DNS 的工作原理。 Nslookup 命令行工具是已安装 TCP/IP 协议才可用。
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
| [nslookup finger 命令](nslookup-finger-command.md) |                                                                                  与当前计算机上的手指服务器建立连接。                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    显示的短摘要**nslookup**子命令。                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             列出 DNS 域的信息。                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   默认服务器更改到指定的 DNS 域。                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     更改应用到 DNS 域命名空间的根服务器的默认服务器。                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   默认服务器更改到指定的 DNS 域。                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              更改影响的配置设置如何查找函数。                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  打印的配置设置的当前值。                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     更改查询类。 类指定的协议组的信息。                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     打开或关闭详尽的调试模式。 打印的每个数据包的所有字段。                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               打开或关闭调试模式下打开。                                                                                               |
|                 nslookup /set defname                 |                                            将默认 DNS 域的名称追加到单个组件查找请求。 单个组件是一个组件，它不包含周期。                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 默认 DNS 域名称更改为指定的名称。                                                                                  |
|                 nslookup /set ignore                  |                                                                                              将忽略数据包截断错误。                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          默认 TCP/UDP DNS 名称服务器端口更改为指定的值。                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       更改查询的资源记录类型。                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    指示 DNS 名称服务器查询的其他服务器，如果它不具有信息。                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 设置重试次数。                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    更改用于查询根服务器的名称。                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | 将 DNS 域搜索列表中的 DNS 域名称追加到请求，直到得到答复。 适用于一组，并且该查找请求包含至少一个句点，但不是以尾随句点结尾。 |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    更改默认 DNS 域的名称和搜索列表。                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           更改初始要等待的时间对请求的回复的秒数。                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       更改查询的资源记录类型。                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     指定要使用或不使用虚拟线路时发送请求到的服务器。                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          对进行排序并列出的上一个输出**ls**子命令。                                                                          |

## <a name="remarks"></a>备注
- 如果*要查找的计算机*是 IP 地址和查询是 a 或返回 PTR 资源记录类型，计算机的名称。 如果*要查找的计算机*是一个名称并不具有尾随句点，DNS 域名追加到名称的默认值。 此行为取决于以下的状态**设置**子命令：**域**， **srchlist**， **defname**，和**搜索**。
- 如果您键入连字符 （-） 而不是*要查找的计算机*，命令提示符将更改为**nslookup**交互模式。
- 命令行的长度必须少于 256 个字符。
- **nslookup**有两种模式： 交互式和非交互式。
  如果需要查找只有一段数据，，使用非交互模式。 对于第一个参数，请键入名称或你想要查找的计算机的 IP 地址。 对于第二个参数，请键入名称或 IP 地址的 DNS 名称服务器。 如果省略第二个参数，则**nslookup**使用的默认 DNS 名称服务器。
  如果需要查找的数据的多个段，可以使用交互模式。 为第一个参数的名称或 IP 地址的第二个参数的 DNS 名称服务器键入连字符 （-）。 或忽略这两个参数和**nslookup**使用的默认 DNS 名称服务器。 以下是有关在交互模式下使用的一些提示：
  -   若要在任何时间中断交互式命令，按 CTRL + B。
  -   若要退出，请键入**退出**。
  -   若要将内置命令视为计算机名称，前面加上转义符 (\\)。
  -   无法识别的命令解释为计算机名称。
- 如果查找请求失败， **nslookup**显示一条错误消息。 下表列出了可能的错误消息。
  |**错误消息**|**说明**|
  |-----------|----------|
  |`timed out`|服务器未在一定的时间和重试一定次数后响应请求。 可以设置使用的超时期限**超时设置**子命令。 可以设置使用的重试次数**集重试**子命令。|
  |`No response from server`|在服务器计算机上不运行任何 DNS 名称服务器。|
  |`No records`|尽管计算机名称有效，没有计算机，当前的查询类型的资源记录的 DNS 名称服务器。 使用指定的查询类型**设置 querytype**命令。|
  |`Nonexistent domain`|计算机或 DNS 域名不存在。|
  |`Connection refused`<br /><br />-或-<br /><br />`Network is unreachable`|无法建立到 DNS 名称服务器或手指服务器的连接。 此错误通常会出现**ls**并**手指**请求。|
  |`Server failure`|DNS 名称服务器在其数据库中找到内部不一致和不能返回一个有效的答案。|
  |`Refused`|DNS 名称服务器拒绝为请求提供服务。|
  |`format error`|DNS 名称服务器发现请求数据包未采用正确格式。 它可能指示中的错误**nslookup**。|
- 有关详细信息**nslookup**命令和 DNS，请参阅以下资源：
  - Lee, T., Davies, J.2000. *Microsoft Windows 2000 TCP/IP Protocols and Services 技术参考*。 华盛顿州雷蒙德市：Microsoft Press.
  - Albitz，P.和 Loukides，M.和 C.Liu。 2001. *DNS 和 BIND、 第四版*。 加利福尼亚州 Sebastopol:O'Reilly and associates，inc.
  - Larson，M.和 C.Liu。 2001. *Windows 2000 上的 DNS*。 加利福尼亚州 Sebastopol:O'Reilly and associates，inc.
    #### <a name="examples"></a>示例
    每个命令行选项包含连字符 （-） 后紧跟的命令名称，并在某些情况下，等号 （=） 和一个值。 例如，若要默认查询类型更改为主机 （计算机） 信息，并为 10 秒的初始超时值，请键入： **nslookup-querytype = hinfo 超时 = 10**
    ## <a name="see-also"></a>请参阅
    [命令行语法项](command-line-syntax-key.md)
