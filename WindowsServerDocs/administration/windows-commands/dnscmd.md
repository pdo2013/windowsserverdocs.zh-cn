---
title: Dnscmd
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815498"
---
# <a name="dnscmd"></a>Dnscmd

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

用于管理 DNS 服务器的命令行界面。 此实用程序可在脚本编写批处理文件以帮助自动执行例行 DNS 管理任务，或在网络上执行简单的无人参与的安装和配置新的 DNS 服务器中。  
## <a name="syntax"></a>语法  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|<ServerName>|远程或本地的 DNS 服务器的 IP 地址或主机名称。|  
## <a name="commands"></a>命令  
|Command|描述|  
|------|--------|  
|[dnscmd /ageallrecords](#BKMK_1)|在所有时间戳的区域或节点上设置的当前时间。|  
|[dnscmd /clearcache](#BKMK_2)|清除 DNS 服务器缓存。|  
|[dnscmd /config](#BKMK_3)|重置 DNS 服务器或区域配置。|  
|[dnscmd /createbuiltindirectorypartitions](#BKMK_4)|创建内置的 DNS 应用程序目录分区。|  
|[dnscmd /createdirectorypartition](#BKMK_5)|创建 DNS 应用程序目录分区。|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|删除 DNS 应用程序目录分区。|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|列出有关 DNS 应用程序目录分区的信息。|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|将 DNS 服务器添加到的 DNS 应用程序目录分区复制集。|  
|[dnscmd /enumdirectorypartitions](#BKMK_9)|列出服务器的 DNS 应用程序目录分区。|  
|[dnscmd /enumrecords](#BKMK_10)|列出区域中的资源记录。|  
|[dnscmd /enumzones](#BKMK_11)|列出指定的服务器托管的区域。|  
|[dnscmd /exportsettings](#BKMK_25a)|将服务器配置信息写入到文本文件中。|  
|[dnscmd /info](#BKMK_12)|获取服务器信息。|  
|[dnscmd /ipvalidate](#BKMK_29a)|验证的远程 DNS 服务器。|  
|[dnscmd /nodedelete](#BKMK_13)|删除某个区域中的节点的所有记录。|  
|[dnscmd /recordadd](#BKMK_14)|将资源记录添加到区域。|  
|[dnscmd /recorddelete](#BKMK_15)|从一个区域中删除资源记录。|  
|[dnscmd /resetforwarders](#BKMK_16)|设置 DNS 服务器，以转发递归查询。|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|设置要为 DNS 请求提供服务的服务器 IP 地址。|  
|[dnscmd /startscavenging](#BKMK_18)|启动服务器清理。|  
|[dnscmd /statistics](#BKMK_19)|查询或清除服务器统计信息数据。|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|从 DNS 应用程序目录分区复制组中删除 DNS 服务器。|  
|[dnscmd /writebackfiles](#BKMK_21)|将所有区域或根提示数据都保存到文件。|  
|[dnscmd /zoneadd](#BKMK_22)|在 DNS 服务器上创建新的区域。|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|更改区域所驻留的目录分区。|  
|[dnscmd /zonedelete](#BKMK_24)|从 DNS 服务器中删除区域。|  
|[dnscmd /zoneexport](#BKMK_25)|区域的资源记录写入文本文件。|  
|[dnscmd /zoneinfo](#BKMK_26)|显示区域的信息。|  
|[dnscmd /zonepause](#BKMK_27)|暂停区域。|  
|[dnscmd /zoneprint](#BKMK_28)|在区域中显示的所有记录。|  
|[dnscmd /zonerefresh](#BKMK_30)|强制刷新从主区域的辅助区域。|  
|[dnscmd /zonereload](#BKMK_31)|重新加载区域从其数据库。|  
|[dnscmd /zoneresetmasters](#BKMK_32)|更改区域传输信息提供给辅助区域的主服务器。|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|更改可以清理区域的服务器。|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|重置为区域的辅助信息。|  
|[dnscmd /zoneresettype](#BKMK_29)|更改区域类型。|  
|[dnscmd /zoneresume](#BKMK_35)|恢复区域。|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|使用 active directory 域服务 (AD DS) 中的数据更新 active directory 集成的区域。|  
|[dnscmd /zonewriteback](#BKMK_37)|将区域数据保存到文件。|  
### <a name="BKMK_1"></a>dnscmd /ageallrecords  
设置在指定的区域或 DNS 服务器上的节点的资源记录的时间戳的当前时间。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>Parameters  
<ServerName>  
指定的 DNS 服务器的管理员计划来管理，由表示 IP 地址、 完全限定的域名 (FQDN) 或主机名。 如果省略此参数，则使用本地服务器。  
<ZoneName>  
指定区域的 FQDN。  
<NodeName>  
在区域中指定特定节点或子树。 *NodeName*使用以下区域中指定的节点或子树：  
-   @ 根区域或 FQDN  
-   节点 （用句点 （.） 结束时的名称） 的 FQDN  
-   相对于区域根名称在一个标签  
/tree  
指定所有子节点也都接收时间戳。  
/f  
运行该命令而不要求确认。  
#### <a name="remarks"></a>备注  
-   **Ageallrecords**命令适用于 DNS 的当前版本和以前版本的 DNS 中的老化和清理不受之间向后兼容性。 它将与当前时间的时间戳添加到不具有时间戳的资源记录并在有时间戳的资源记录上设置的当前时间。  
-   除非记录时间戳，否则不会记录清理。 名称服务器 (NS) 资源记录，授权 (SOA) 资源记录的开始和在清理过程中，不包括 Windows Internet 名称服务 (WINS) 资源记录中，它们不是时间戳，即使**ageallrecords**命令将运行。  
-   此命令会失败，除非启用清理的 DNS 服务器和区域。 有关如何启用该区域的信息，请参阅**老化**参数中的区域级别语法下[config](#BKMK_3)命令。  
-   添加 DNS 资源记录的时间戳会使它们与 Windows 2000，Windows XP 或 Windows Server 2003 以外的操作系统运行的 DNS 服务器不兼容。 使用添加时间戳**ageallrecords**命令无法撤消。  
-   如果未指定任何可选参数，该命令将返回在指定节点上的所有资源记录。 如果至少一个可选参数，指定的值**dnscmd**枚举值或可选参数或参数中指定的值对应的资源记录。  
#### <a name="example"></a>示例  
请参阅[示例 1:将当前时间设置为资源记录的时间戳上](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_2"></a>dnscmd /clearcache  
清除 DNS 缓存内存的指定的 DNS 服务器上的资源记录。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>Parameters  
<ServerName>  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
#### <a name="sample-usage"></a>示例用法  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
更改 DNS 服务器和各个区域的注册表中的值。 接受服务器级别的设置和区域级别的设置。  
> [!CAUTION]  
> 不要编辑注册表直接除非别无选择。 注册表编辑器避开了标准安全措施，从而使得这些可能会降低性能、 会损坏您的系统，或甚至需要您重新安装 Windows 设置。 通过使用 Microsoft 管理控制台 (mmc) 或控制面板中的程序，可以安全地更改大多数注册表设置。 如果必须直接编辑注册表，先进行备份。 读取注册表编辑器帮助了解详细信息。  
#### <a name="server-level-syntax"></a>服务器级语法  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
修改指定的服务器的配置。  
#### <a name="parameters"></a>Parameters  
<ServerName>  
指定想要管理的 DNS 服务器由本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
<Parameter>  
指定的设置，并选择一个值。 参数值使用以下语法：*Parameter* [*Value*]  
在本部分的其余部分介绍以下参数值：  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0|5-28]**  
指定最大的 DNS 服务器可以在查询响应中发送的主机记录。 值可以为零 (0)，也可以是在 5 到 28 记录的范围内。 默认值为零 (0)。  
**/bindsecondaries[0|1]**  
更改区域传送的格式，以便它可以实现最大压缩和效率。 但是，此格式不是与早期版本的 Berkeley Internet 名称域 (BIND) 兼容的。  
**0**  
使用最大压缩。 此格式是与绑定版本 4.9.4 兼容及更高版本。  
**1**  
将只有一个资源记录每个消息发送到非 Microsoft DNS 服务器。 此格式是比 4.9.4 与之前的绑定版本兼容。 这是默认设置。  
**/bootmethod[0|1|2|3]**  
确定的源 DNS 服务器从其获取其配置信息。  
**0**  
清除配置信息的源。  
**1**  
在 DNS 目录中，这是默认情况下 %systemroot%\System32\DNS 位于该绑定文件的负载。  
**2**  
从注册表加载。  
**3**  
AD DS 和注册表的负载。 这是默认设置。  
**/defaultagingstate[0|1]**  
确定是否默认情况下，新创建的区域上启用 DNS 清理功能。  
**0**  
禁用清理。 这是默认设置。  
**1**  
启用清理。  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
设置一段时间接受未刷新的动态更新的记录。 服务器上的区域自动继承此值。 若要更改默认值，请键入一个值的范围内**0x1 0xFFFFFFFF**。 在服务器中的默认值是**0xA8**。  
**/defaultrefreshinterval [0x1-0xFFFFFFFF|0xA8]**  
设置允许的动态更新 DNS 记录的时间段。 服务器上的区域自动继承此值。 若要更改默认值，请键入一个值的范围内**0x1 0xFFFFFFFF**。 在服务器中的默认值是**0xA8**。  
**/disableautoreversezones [0|1]**  
启用或禁用自动创建反向查找区域。 反向查找区域提供 Internet 协议 (IP) 地址的 DNS 域名的解析。  
**0**  
启用自动创建的反向查找区域。 这是默认设置。  
**1**  
禁用自动创建反向查找区域。  
**/disablensrecordsautocreation {0|1}**  
指定是否在 DNS 服务器会自动创建名称服务器 (NS) 资源记录中的区域，它可以承载。  
**0**  
将自动创建名称服务器 (NS) 资源记录的区域的 DNS 服务器承载。  
**1**  
不会自动创建名称服务器 (NS) 资源记录的区域的 DNS 服务器承载。  
**/dspollinginterval 0-30**  
指定频率的 DNS 服务器轮询 AD DS 的更改在 active directory 中集成的区域。  
**/dstombstoneinterval [1-30]**  
以秒为单位保留在 AD DS 中已删除的记录的时间量。  
**/ednscachetimeout [<seconds>]**  
指定 DNS (EDNS) 的扩展的信息缓存的秒的数。 最小值为 3600，并最大值是 15,724,800。 默认值为 604,800 秒 （一周）。  
**/enableednsprobes {0|1}**  
启用或禁用要探测来确定它们是否支持 EDNS 其他服务器的服务器。  
**0**  
禁用 active 支持 EDNS 探测。  
**1**  
活动支持 EDNS 探测。  
**/enablednssec {0|1}**  
启用或禁用对 DNS 安全扩展 (DNSSEC) 支持。  
**0**  
禁用 DNSSEC。  
**1**  
启用 DNSSEC。  
**/enableglobalnamessupport {0|1}**  
启用或禁用对 GlobalNames 区域的支持。 GlobalNames 区域支持跨林的单标签 DNS 名称解析。  
**0**  
禁用对 GlobalNames 区域的支持。 如果设置为此命令的值**0**，DNS 服务器服务不会解析在 GlobalNames 区域中的单标签名称。  
**1**  
启用对 GlobalNames 区域的支持。 如果设置为此命令的值**1**，DNS 服务器服务将在 GlobalNames 区域中的单标签名称解析。  
**/enableglobalqueryblocklist {0|1}**  
启用或禁用对全局查询阻止列表阻止列表中的名称的名称解析的支持。 DNS 服务器服务创建，并在服务启动第一次时默认情况下启用全局查询阻止列表。 若要查看当前全局查询阻止列表，请使用**dnscmd /info /globalqueryblocklist**命令。  
**0**  
禁止支持人员为全局查询阻止列表。 如果设置为此命令的值**0**，DNS 服务器服务响应中的块列表的名称查询。  
**1**  
启用对全局查询阻止列表的支持。 如果设置为此命令的值**1**，DNS 服务器服务不响应中的块列表的名称查询。  
**/eventloglevel [0|1|2|4]**  
确定在事件查看器在 DNS 服务器日志中记录的事件。  
**0**  
不记录任何事件。  
**1**  
仅记录错误。  
**2**  
只记录错误和警告。  
**4**  
记录错误、 警告和信息性事件。 这是默认设置。  
**/forwarddelegations [0|1]**  
确定 DNS 服务器如何处理用于委派子区域的查询。 这些查询可以为 DNS 服务器到查询中引用子区域或名为转发器列表发送。 仅当启用转发时，会使用在设置中的项。  
**0**  
会自动发送到相应的子区域的委派子区域是指的查询。 这是默认设置。  
**1**  
将转发到现有的转发器委托子区域是指的查询。  
**/forwardingtimeout [<seconds>]**  
确定转发器尝试另一个转发器之前响应的等待的秒数 (0x1 0xFFFFFFFF) DNS 服务器。 默认值是**0x5**，即 5 秒。  
**/globalneamesqueryorder** {**0|1**}  
指定是否 DNS 服务器服务中查找第一个在 GlobalNames 区域或本地区域解析名称时。  
**0**  
DNS 服务器服务尝试通过查询它是权威的区域之前查询 GlobalNames 区域解析名称。  
**1**  
DNS 服务器服务尝试通过查询它的权威之前它将查询 GlobalNames 区域的区域解析名称。  
**/globalqueryblocklist[[<name> [<name>]...]**  
使用指定名称的列表替换当前全局查询阻止列表。 如果未指定任何名称，此命令将清除在的块列表。 默认情况下，全局查询阻止列表包含以下各项：  
-   isatap  
-   wpad  
DNS 服务器服务可以删除一个或两个这些名称在首次启动时如果在现有区域中找到这些名称。  
**/isslave [0|1]**  
确定 DNS 服务器时用于将其转发查询不收到任何响应的响应。  
**0**  
指定 DNS 服务器不是从属项 (也称为*从属*)。 如果该转发器没有响应，DNS 服务器将尝试解析查询本身。 这是默认设置。  
**1**  
指定 DNS 服务器是一个下属。 如果该转发器没有响应，DNS 服务器将终止搜索，并将失败消息发送到解析程序。  
**/localnetpriority [0|1]**  
确定 DNS 服务器具有相同名称的多个主机记录时返回主机记录的顺序。  
**0**  
在 DNS 数据库中列出的顺序返回的记录。  
**1**  
返回具有类似的 IP 的记录的第一次网络地址。 这是默认设置。  
**/logfilemaxsize [<size>]**  
以字节为单位 (0x10000 0xFFFFFFFF) 的 Dns.log 文件中指定的最大大小。 当文件达到其最大大小时，DNS 将覆盖最早的事件。 默认大小为 0x400000 处，这是 4 兆字节 (MB)。  
**/logfilepath [<path+LogFileName>]**  
指定 Dns.log 文件的路径。 默认路径为 %systemroot%\system32\dns\dns.log。 可以通过使用以下格式指定一个不同的路径*路径 + LogFileName*。  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
指定将哪些数据包记录调试日志文件中。 条目是 IP 地址的列表。 记录仅转到和从列表中的 IP 地址的数据包。  
**/loglevel [<Eventtype>]**  
确定哪些类型的事件记录在 Dns.log 文件。 一个十六进制数字，表示每个事件类型。 如果您希望多个事件日志中的，使用十六进制添加以添加相应的值，然后输入之和。  
**0x0**  
DNS 服务器不会创建一个日志。 这是默认条目。  
**0x10**  
记录查询。  
**0x10**  
记录通知。  
**0x20**  
记录更新。  
**0xFE**  
记录 nonquery 事务。  
**0x100**  
日志问题的事务。  
**0x200**  
记录答案。  
**0x1000**  
日志发送数据包。  
**0x2000**  
日志接收数据包。  
**0x4000**  
记录用户数据报协议 (UDP) 包。  
**0x8000**  
记录传输控制协议 (TCP) 数据包。  
**0xFFFF**  
记录所有数据包。  
**0x10000**  
记录 active directory 写入事务。  
**0x20000**  
记录 active directory 的更新事务。  
**0x1000000**  
日志完整数据包。  
**0x80000000**  
直写事务日志。  
**/maxcachesize**  
指定的 DNS 服务器的内存缓存的最大大小 (kb)。  
**/maxcachettl [<seconds>]**  
确定秒数 (0x0 0xFFFFFFFF) 记录保存在缓存中。 如果**0x0**设置，DNS 服务器不会缓存的记录。 默认设置是**0x15180** （86400 秒或 1 天）。  
**/maxnegativecachettl [<seconds>]**  
指定秒数 (0x1 0xFFFFFFFF) 记录的负查询答案的条目会一直存储在 DNS 缓存中。 默认设置是**0x384** （900 秒为单位）。  
**/namecheckflag [0|1|2|3]**  
指定检查 DNS 名称时使用哪些字符标准。  
**0**  
使用 ANSI 字符符合 Internet 工程任务强制 (IETF) 征求意见 (文档 Rfc)。  
**1**  
不一定符合 IETF Rfc 使用 ANSI 字符。  
**2**  
使用多字节 UCS 转换格式 8 (utf-8) 个字符。 这是默认设置。  
**3**  
使用的所有字符。  
**/norecursion [0|1]**  
确定 DNS 服务器是否执行递归名称解析。  
**0**  
如果请求在查询中，DNS 服务器执行递归名称解析。 这是默认设置。  
**1**  
DNS 服务器不执行递归名称解析。  
**/notcp**  
此参数是已过时，并且它具有在当前版本的 Windows Server 中不起作用。  
**/recursionretry [<seconds>]**  
确定 DNS 服务器再次尝试联系远程服务器之前等待的秒 (0x1 0xFFFFFFFF) 数。 默认设置为 0x3 （3 秒）。 递归发生通过慢速广域网 (wan) 链接时，应增加此值。  
**/recursiontimeout [<seconds>]**  
确定 DNS 服务器不再使用的尝试联系远程服务器之前等待的秒 (0x1 0xFFFFFFFF) 数。 设置介于**0x1**通过**0xFFFFFFFF**。 默认设置是**0xF** （15 秒为单位）。 递归发生通过慢速 WAN 链接时，应增加此值。  
**/roundrobin [0|1]**  
确定在服务器具有相同名称的多个主机记录时返回主机记录的顺序。  
0  
DNS 服务器不使用轮循机制。 相反，它返回第一条记录到每个查询。  
**1**  
DNS 服务器将从顶部返回到匹配的记录的列表底部的记录之间旋转。 这是默认设置。  
**/rpcprotocol [0x0|0x1|0x2|0x4|0xFFFFFFFF]**  
指定建立从 DNS 服务器的连接时，远程过程调用 (RPC) 使用的协议。  
**0x0**  
禁用 DNS RPC。  
**0x1**  
使用 TCP/IP。  
**0x2**  
使用命名管道。  
**0x4**  
使用本地过程调用 (LPC)。  
**0xFFFFFFFF**  
所有协议。 这是默认设置。  
**/scavenginginterval [<hours>]**  
确定是否为 DNS 服务器清理功能启用，并设置之间的小时数 (0x0 0xFFFFFFFF) 清理周期。 默认设置是**0x0**，这将禁用对 DNS 服务器进行清除。 设置大于**0x0**启用清理服务器，并设置清理周期之间的小时数。  
**/secureresponses [0|1]**  
确定 DNS 是否筛选保存在缓存的记录。  
**0**  
将所有名称查询响应都保存到缓存。 这是默认设置。  
**1**  
将保存仅属于同一个 DNS 子树的缓存的记录。  
**/sendport [<port>]**  
指定 DNS 使用递归查询发送到其他 DNS 服务器的端口号 (0x0 0xFFFFFFFF)。 默认设置是**0x0**，这意味着将会随机选择的端口号。  
**/serverlevelplugindll[<Dllpath>]**  
指定自定义插件的路径。 当*Dllpath*指定有效的 DNS 服务器插件，DNS 服务器在插件来解析名称查询的作用域外所有本地托管的区域中调用函数的完全限定的路径名称。 如果超出该插件的作用域查询的名称，DNS 服务器执行名称解析使用转发或递归中，配置。 如果*Dllpath*未指定，则 DNS 服务器停止时要使用自定义插件之前配置自定义插件。  
**/strictfileparsing [0|1]**  
加载某个区域时遇到的错误记录时，请确定 DNS 服务器的行为。  
**0**  
DNS 服务器将继续加载该区域，即使服务器遇到的错误记录。 DNS 日志中记录错误。 这是默认设置。  
**1**  
DNS 服务器停止加载该区域，并 DNS 日志中记录错误。  
**/updateoptions <RecordValue>**  
禁止动态更新的指定类型的记录。 如果你想要禁止在日志中的多个记录类型，使用十六进制添加以添加相应的值，然后输入之和。  
**0x0**  
不限制任何记录的类型。  
**0x1**  
排除的授权 (SOA) 资源记录的开始。  
**0x2**  
排除名称服务器 (NS) 资源记录。  
**0x4**  
不包括委派的名称服务器 (NS) 资源记录。  
**0x8**  
排除服务器主机记录。  
**0x100**  
安全动态更新过程中排除的授权 (SOA) 资源记录的开始。  
**0x200**  
安全动态更新的过程中不包括根名称服务器 (NS) 资源记录。  
**0x30F**  
在标准动态更新期间排除名称服务器 (NS) 资源记录，起始授权 (SOA) 资源记录和服务器主机记录。 安全动态更新的过程中不包括根名称服务器 (NS) 资源记录和授权 (SOA) 资源记录的开始。 允许委派和服务器宿主更新。  
**0x400**  
安全动态更新的过程中不包括委派名称服务器 (NS) 资源记录。  
**0x800**  
安全动态更新的过程中不包括服务器主机记录。  
**0x1000000**  
不包括委派签名者 (DS) 记录。  
**0x80000000**  
禁用 DNS 动态更新。  
**/writeauthorityns [0|1]**  
确定 DNS 服务器将名称服务器 (NS) 资源记录中写入响应的颁发机构部分中。  
**0**  
仅引用的颁发机构部分中写入名称服务器 (NS) 资源记录。 此设置将遵循与 Rfc 1034，领域名称概念和设施，和 Rfc 2181，DNS 规范的说明。 这是默认设置。  
**1**  
将名称服务器 (NS) 资源记录写入的颁发机构部分中的所有成功的权威响应。  
**/xfrconnecttimeout [<seconds>]**  
确定等待秒的数 (0x0 0xFFFFFFFF) 的主 DNS 服务器从其辅助服务器传输响应。 默认值是**0x1E** （30 秒）。 超时值过期后，连接将终止。  
#### <a name="zone-level-syntax"></a>区域级别语法  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
修改指定的区域的配置。  
#### <a name="parameters"></a>Parameters  
**<Parameters>**  
指定设置，区域名称的并选择一个值。 参数值使用以下语法：*ZoneName Parameter* [*Value*]  
在本部分的其余部分介绍了以下参数值：  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/ 老化 <ZoneName>**  
启用或禁用清理在特定区域中。  
**/allownsrecordsautocreation  <ZoneName> [<Value>]**  
重写 DNS 服务器的名称服务器 (NS) 资源记录自动创建设置。 不会影响之前已注册为此区域的名称服务器 (NS) 资源记录。 因此，您必须手动将其删除如果不希望它们。  
**/allowupdate <ZoneName>**  
确定指定的区域是否接受动态更新。  
**/forwarderslave <ZoneName>**  
重写 DNS 服务器 **/isslave**设置。  
**/forwardertimeout <ZoneName>**  
确定多少秒等待转发器响应后再尝试另一个转发器的 DNS 区域。 此值将覆盖服务器级别设置的值。  
**/norefreshinterval <ZoneName>**  
设置在此期间没有刷新可以动态更新中指定的区域的 DNS 记录的区域的时间间隔。  
**/refreshinterval <ZoneName>**  
设置在此期间刷新可以动态更新中指定的区域的 DNS 记录的区域的时间间隔。  
**/securesecondaries <ZoneName>**  
确定哪些辅助服务器可以接收区域更新此区域的主服务器中。  
#### <a name="remarks"></a>备注  
-   必须仅为区域级别参数指定的区域名称。  
### <a name="BKMK_4"></a>dnscmd /createbuiltindirectorypartitions  
创建 DNS 应用程序目录分区。 安装 DNS 时，在林和域级别创建服务应用程序目录分区。 使用此命令创建已删除或永远不会创建的 DNS 应用程序目录分区。 不使用任何参数，此命令创建内置的 DNS 目录分区的域。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**/forest**  
创建林 DNS 目录分区。  
**/alldomains**  
在林中创建所有域的 DNS 的分区。  
### <a name="BKMK_5"></a>dnscmd /createdirectorypartition  
创建 DNS 应用程序目录分区。 安装 DNS 时，在林和域级别创建服务应用程序目录分区。 此操作将创建其他的 DNS 应用程序目录分区。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<PartitionFQDN>**  
将创建的 DNS 应用程序目录分区的 FQDN。  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
删除现有的 DNS 应用程序目录分区。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<PartitionFQDN>**  
将删除的 DNS 应用程序目录分区的 FQDN。  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
列出有关指定的 DNS 应用程序目录分区的信息。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<PartitionFQDN>**  
DNS 应用程序目录分区的 FQDN。  
**/detail**  
列出有关应用程序目录分区的所有信息。  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
将 DNS 服务器添加到指定的目录分区的副本集。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<PartitionFQDN>**  
DNS 应用程序目录分区的 FQDN。  
### <a name="BKMK_9"></a>dnscmd /enumdirectorypartitions  
列出指定的服务器的 DNS 应用程序目录分区。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**/custom**  
列出了仅用户创建的目录分区。  
### <a name="BKMK_10"></a>dnscmd /enumrecords  
列出 DNS 区域中的指定节点的资源记录。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定你打算管理的 DNS 服务器由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**/enumrecords**  
列出指定区域中的资源记录。  
**<ZoneName>**  
指定的资源记录所属的区域的名称。  
**<NodeName>**  
指定资源记录的节点的名称。  
**/type <RRtype> <Rrdata>**  
指定要列出的资源记录的类型和预期数据类型：  
**<RRtype>**  
指定要列出的资源记录的类型。  
**<Rrdata>**  
指定是预期的记录的数据类型。  
**/authority**  
包括权威数据。  
**/glue**  
包括粘附数据。  
**/additional**  
包括有关列出的资源记录的所有其他信息。  
**{/node | /child | /startchild <ChildName>}**  
筛选器，或将信息添加到资源记录显示：  
**/node**  
列出了仅在指定节点的资源记录。  
**/child**  
列出了仅指定的子域的资源记录。  
**/startchild <ChildName>**  
开始在指定的子域的列表。  
**/continue | /detail**  
指定如何显示返回的数据。  
**/continue**  
列出了它们的类型和数据仅资源记录。  
**/detail**  
列出有关的资源记录的所有信息。  
#### <a name="sample-usage"></a>示例用法  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>dnscmd /enumzones  
列出指定的 DNS 服务器存在的区域。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
若要显示的区域的类型的筛选器：  
**/primary**  
列出了所有标准主要区域或 active directory 集成区域的区域。  
**/secondary**  
列出所有的标准辅助区域。  
**/forwarder**  
列出未解析的查询转发到另一台 DNS 服务器的区域。  
**/stub**  
列出所有的存根区域。  
**/cache**  
列出了仅加载到缓存的区域。  
**/auto-created**  
列出了在 DNS 服务器安装期间自动创建的区域。  
**/ 正 |/ 反向 |/ds |/file**  
指定类型的区域以显示其他筛选的器：  
**/forward**  
列表正向查找区域。  
**/reverse**  
列表反向查找区域。  
**/ds**  
列出 active directory 集成区域。  
**/file**  
列出由文件支持的区域。  
**/domaindirectorypartition**  
列出存储在域目录分区中的区域。  
**/forestdirectorypartition**  
列出存储在林 DNS 应用程序目录分区中的区域。  
**/customdirectorypartition**  
列出存储在用户定义的应用程序目录分区中的所有区域。  
**/legacydirectorypartition**  
列出存储在域目录分区中的所有区域。  
**/directorypartition <PartitionFQDN>**  
列出存储在指定的目录分区中的所有区域。  
#### <a name="remarks"></a>备注  
-   **Enumzones**参数作为筛选器区域的列表。 如果未不指定任何筛选器，则返回的区域的完整列表。 指定筛选器时，返回的区域列表中包含满足该筛选器条件的区域。  
#### <a name="example"></a>示例  
请参阅[示例 2:DNS 服务器上显示区域的完整列表](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[示例 3:显示 DNS 服务器上的自动创建区域的列表](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_25a"></a>dnscmd /exportsettings  
创建一个文本文件，其中列出了 DNS 服务器的配置详细信息。 文本文件命名为 DnsSettings.txt。 它位于服务器的 %systemroot%\system32\dns 目录中。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
#### <a name="remarks"></a>备注  
-   可以在文件中使用的信息的**dnscmd /exportsettings**创建来解决配置问题，或以确保配置多个服务器都相同。  
### <a name="BKMK_12"></a>dnscmd /info  
显示指定服务器的注册表的 DNS 部分中的设置：**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<Setting>**  
任何设置**信息**命令返回可以单独指定。 如果未指定设置，则返回的常见设置报告。  
#### <a name="remarks"></a>备注  
-   此命令显示在 DNS 服务器级别的注册表设置。 若要显示的区域级别注册表设置，请使用[zoneinfo](#BKMK_26)命令。 若要查看可以使用此命令显示的设置的列表，请参阅[config](#BKMK_3)说明。  
#### <a name="example"></a>示例  
请参阅[示例 4:显示在 DNS 服务器中设置的 IsSlave](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[示例 5:显示来自 DNS 服务器的 Recursiontimeout 设置](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_29a"></a>dnscmd /ipvalidate  
测试是否正常运行的 DNS 服务器或是否在 DNS 服务器可以充当转发器、 根提示服务器或特定区域的主服务器，IP 地址标识。  
#### <a name="syntax"></a>语法  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<Context>**  
指定要执行测试的类型。 您可以指定任何以下测试：  
-   **/dnsservers**测试具有您指定的地址的计算机都运行正常的 DNS 服务器。  
-   **/forwarders**测试指定的地址标识可以充当转发器的 DNS 服务器。  
-   **/roothints**测试指定的地址标识可以充当根提示名称服务器的 DNS 服务器。  
-   **/zonemasters**测试指定的地址标识是主服务器的 DNS 服务器*ZoneName*。  
**<ZoneName>**  
标识该区域。 使用此参数与 **/zonemasters**参数。  
**<IPaddress>**  
指定命令测试的 IP 地址。  
#### <a name="sample-usage"></a>示例用法  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>dnscmd /nodedelete  
为指定的主机中删除所有记录。 # # # 语法```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/f] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定的区域的名称。  
**<NodeName>**  
指定要删除的节点的主机名。  
**/tree**  
删除所有子记录。  
**/f**  
执行命令而不要求确认。 # # # 示例，请参见[示例 6： 从节点删除的记录](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_14"></a>dnscmd /recordadd  
将一条记录添加到 DNS 服务器中指定的区域。 # # # 语法```  
dnscmd [<ServerName>] /recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定记录所在的区域。  
**<NodeName>**  
在区域中指定一个特定节点。  
**<RRtype>**  
指定要添加的记录类型。  
**<Rrdata>**  
指定预期数据的类型。  
> [!NOTE]  
当您添加一条记录时，请确保使用正确的数据类型和数据格式。 资源记录类型和相应的数据类型的列表，请参阅[资源记录参考](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx)。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>dnscmd /recorddelete  
从指定的区域中删除资源记录。 # # # 语法```  
dnscmd <ServerName>/recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/f] ```  
#### Parameters  
**<ServerName>**  
> 指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定资源记录所在的区域。  
**<NodeName>**  
指定的主机的名称。  
**<RRtype>**  
指定要删除的资源记录的类型。  
**<Rrdata>**  
指定预期数据的类型。  
**/f**  
执行命令而不要求确认:-由于节点可以具有多个资源记录，此命令要求你非常具体的你想要删除的资源记录的类型。 -如果指定的数据类型，并且未指定资源记录数据的类型，将删除与该特定的数据类型为指定的节点的所有记录。 # # # 示例使用情况 `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>dnscmd /resetforwarders  
选择或重置 DNS 服务器将转发到 DNS 查询时无法本地解析的 IP 地址。 # # # 语法```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [，<IPaddress>]...][/ 超时<timeOut>] [/ 从 | / noslave] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<IPaddress>**  
列出 DNS 服务器将未解析的查询转发到的 IP 地址。  
**/timeout <timeOut>**  
设置 DNS 服务器等待来自该转发器的响应的秒数。 默认情况下，此值为 5 秒。  
**/slave|/noslave**  
确定是否无法解析的查询转发器 DNS 服务器是否执行其自己的重复查询：  
**/slave**  
防止 DNS 服务器执行其自身迭代的查询，如果该转发器无法解析查询。  
**/noslave**  
允许其自己的递归查询失败时要执行该转发器解析的查询的 DNS 服务器。 这是默认设置。 # # # 注释-默认情况下，DNS 服务器执行迭代的查询时它不能解析查询。 -通过使用设置的 IP 地址**resetforwarders**命令会导致 DNS 服务器执行递归查询在指定的 IP 地址的 DNS 服务器。 如果转发器不能解决查询，DNS 服务器然后可以执行其自身迭代的查询。 -如果 **/从**使用参数时，DNS 服务器不执行其自己的重复查询。 这意味着 DNS 服务器将转发仅对在列表中，DNS 服务器未解析的查询，并且它不会尝试重复查询，如果转发器不能解决它们。 它会更有效的 DNS 服务器作为转发器设置一个 IP 地址。 可以使用**resetforwarders**命令获取其无法解析将查询转发到一个 DNS 服务器具有外部连接的网络中的内部服务器。 -两次列出转发器的 IP 地址会导致 DNS 服务器尝试两次转发到该服务器。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
指定的 IP 地址上侦听 DNS 客户端请求的服务器。 # # # 语法```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<listenaddress>**  
指定的 IP 地址上侦听 DNS 客户端请求的 DNS 服务器。 如果未不指定任何侦听地址，在服务器上的所有 IP 地址都侦听的客户端请求。 # # # 注释-默认情况下，DNS 服务器上的所有 IP 地址都侦听的客户端的 DNS 请求。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>dnscmd /startscavenging  
指示要尝试的即时搜索指定的 DNS 服务器中的过时资源记录的 DNS 服务器。 # # # 语法```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。 # # # 备注的此命令成功完成后立即启动清理。 -除非满足以下前提条件尽管命令以启动清理似乎已成功完成，但未启动清理:-为服务器和区域启用了清理。 的启动区域。 -资源记录都有一个时间戳。 -若要了解如何允许对服务器进行清除，请参阅**scavenginginterval**参数中的服务器级别语法下[config](#BKMK_3)部分。 -有关如何启用该区域的信息，请参阅**老化**参数中的区域级别语法下[config](#BKMK_3)部分。 -有关如何启动已暂停的区域的信息，请参阅[zoneresume](#BKMK_35)部分。 -有关如何检查时间戳的资源记录的信息，请参阅[ageallrecords](#BKMK_1)部分。 -如果清理失败，不会出现警告消息。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>dnscmd /statistics  
显示或清除指定的 DNS 服务器的数据。 # # # 语法```  
dnscmd [<ServerName>] /statistics [<StatID>] [/clear] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<StatID>**  
指定哪些统计信息或要显示统计信息的组合。 一个标识号用于标识统计信息。 如果指定没有统计信息 ID 号，则将显示所有统计信息。  
下面是可以指定的编号和显示的相应统计信息的列表：  
**00000001**  
时间  
**00000002**  
查询  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
主机  
**00000020**  
辅助  
**00000040**  
WINS  
**00000100**  
Update  
**00000200**  
SkwanSec  
**00000400**  
ds  
**00010000**  
内存  
**00100000**  
PacketMem  
**00040000**  
Dbase  
**00080000**  
记录  
**00200000**  
NbstatMem  
**/clear**  
将重置指定的统计信息计数器为零。 # # # 注释-**统计信息**命令在 DNS 服务器上启动或恢复时将显示开始的计数器。 # # # 示例，请参阅[示例 7:显示 DNS 服务器的时间统计信息](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[示例 8:显示 DNS 服务器的 NbstatMem 统计信息](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
从指定的目录分区的副本集中删除 DNS 服务器。 # # # 语法```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<PartitionFQDN>**  
将删除的 DNS 应用程序目录分区的 FQDN。  
### <a name="BKMK_21"></a>dnscmd /writebackfiles  
检查 DNS 服务器内存的更改，并将其写入到持久存储。 # # # 语法```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要更新的区域的名称。 # # # 注释- **writebackfiles**命令将更新所有脏区域或指定的区域。 有尚未写入到持久存储在内存中的更改时，区域是已更新。 这是检查所有区域的服务器级操作。 可以在此操作中指定一个区域，也可以使用[zonewriteback](#BKMK_37)操作。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>dnscmd /zoneadd  
将区域添加到 DNS 服务器。 # # # 语法```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>|{/ 域 | / 企业版 | / 旧}] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定的区域的名称。  
**<Zonetype>**  
指定要创建区域的类型。 每个区域类型具有不同的必需的参数：  
**/dsprimary**  
创建 active directory 集成的区域。  
**/primary /file <FileName>**  
创建标准主要区域，并指定将区域信息存储文件的名称。  
**/secondary <MasterIPaddress> [<MasterIPaddress>...]**  
创建一个标准的辅助区域。  
**/stub <MasterIPaddress> [<MasterIPaddress>...] /file <FileName>**  
创建文件备份的存根区域。  
**/dsstub <MasterIPaddress> [<MasterIPaddress>...]**  
创建 active directory 集成的存根区域。  
**/ 转发器<MasterIPaddress>[<MasterIPaddress>].../ 文件 <FileName>**  
指定创建的区域将转发到另一台 DNS 服务器未解析的查询。  
**/dsforwarder**  
指定创建的 active directory 集成的区域将转发到另一台 DNS 服务器未解析的查询。  
**/dp <FQDN> {/ 域 | /enterprise | / 旧}**  
指定存储区域所在的目录分区。  
**<FQDN>**  
指定的目录分区的 FQDN。  
**/domain**  
域目录分区上存储的区域。  
**/enterprise**  
企业目录分区上存储的区域。  
**/legacy**  
旧目录分区上存储的区域。 # # # 注释-指定的区域类型 **/forwarder**或 **/dsforwarder**创建区域执行条件性转发。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>dnscmd /zonechangedirectorypartition  
更改指定的区域所驻留的目录分区。 # # # 语法```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] |[<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
该区域所在的当前目录分区的 FQDN。  
**<NewPartitionName>**  
该区域将移到的目录分区的 FQDN。  
**<Zonetype>**  
指定的区域将移到的目录分区的类型。  
**/domain**  
将区域移到内置域目录分区。  
**/forest**  
将区域移到内置的林目录分区。  
**/legacy**  
将区域移到为预 active directory 域控制器创建的目录分区。 这些目录分区则不需要为纯模式。  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
删除指定的区域。 # # # 语法```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/f] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要删除该区域的名称。  
**/dsdel**  
从 AD DS 中删除该区域。  
**/f**  
运行该命令而不要求确认。 # # # 示例，请参见[示例 9： 删除区域的 DNS 服务器从](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
创建一个文本文件，其中列出了指定的区域的资源记录。 # # # 语法 dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile># # # 参数 **<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定的区域的名称。  
**<ZoneExportFile>**  
指定要创建的文件的名称。 # # # 注释- **zoneexport**操作可创建以进行故障排除 active directory 集成区域的资源记录的文件。 默认情况下，此命令将创建该文件位于 DNS 目录，这是默认情况下 %systemroot%/System32/Dns 目录中。 # # # 示例，请参见[示例 10:区域资源记录列表导出到文件](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
显示所述的指定区域的注册表设置: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** # # # 语法```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定的区域的名称。  
**<Setting>**  
可以单独指定任何设置**zoneinfo**命令返回。 如果不指定设置，则返回所有设置。 # # # 注释- **zoneinfo**命令将显示在 DNS 区域级别的注册表设置 **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**。 -若要显示服务器级别的注册表设置，请使用[信息](#BKMK_12)命令。 -若要查看的设置，可以使用此命令显示的列表，请参阅[config](#BKMK_3)命令。 # # # 示例，请参见[示例 11:显示在注册表中设置的刷新间隔设置](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)或[示例 12:显示老化注册表中设置的设置](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx)。  
### <a name="BKMK_27"></a>dnscmd /zonepause  
暂停指定的区域，然后将忽略查询请求。 # # # 语法```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要暂停的区域的名称。 # # # 注释-若要恢复区域并使其可用后已暂停，请使用[zoneresume](#BKMK_35)命令。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
列出区域中的记录。 # # # 语法```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
标识要为列出的区域。  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
强制更新从主区域的辅助 DNS 区域。 # # # 语法```  
dnscmd <ServerName>/zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要刷新该区域的名称。 # # # 注释- **zonerefresh**命令强制执行检查的主服务器的起始授权 (SOA) 资源记录中的版本号。 如果在主服务器上的版本号高于辅助服务器的版本号，启动更新辅助服务器的区域传送。 如果版本编号是相同的不发生区域传送。 -强制的检查默认情况下每隔 15 分钟。 若要更改默认值，请使用**dnscmd 配置 refreshinterval**命令。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
将区域从其源的信息。 # # # 语法```  
dnscmd <ServerName>/zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要重新加载区域的名称。 # # # 注释-如果区域是 active directory 集成，它将重新加载 AD DS 中。 -如果区域是标准文件备份区域，它将重新加载文件。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
重置提供了到辅助区域的区域传输信息的主服务器的 IP 地址。 # # # 语法```  
dnscmd <ServerName>/zoneresetmasters <ZoneName> [/ 本地] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要重新加载区域的名称。  
**/local**  
设置本地主列表。 此参数用于 active directory 集成区域。  
**<IPaddress>**  
辅助区域的主服务器的 IP 地址。 # # # 注释-此值最初设置创建辅助区域。 使用**zoneresetmasters**辅助服务器上的命令。 如果主 DNS 服务器上设置，则此值无效。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
更改可以清理指定的区域的服务器的 IP 地址。 #### Syntax ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
标识要清理的区域。  
**<IPaddress>**  
列出可以执行清理的服务器的 IP 地址。 如果省略此参数，则承载该区域的所有服务器可以都清理它。 # # # M a k-默认情况下，所有托管区域的服务器可以清理该区域。 -如果多个 DNS 服务器上承载一个区域，可以使用此命令来减少区域被清理的次数。 的必须在 DNS 服务器和此命令所影响的区域启用清理。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
指定主服务器响应时要求的区域复制的辅助服务器的 IP 地址的列表。 # # # 语法```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/ noxfr | / 非安全 | /securens | /securelist <SecurityIPaddresses>} {/ nonotify | / 通知 | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略参数，则使用本地服务器。  
**<ZoneName>**  
指定将具有其辅助服务器的区域的名称重置。  
**/noxfr | /nonsecure | /securens | /securelist <SecurityIPaddresses>**  
指定的所有或部分请求更新的辅助服务器获取更新。  
**/noxfr**  
指定不允许任何区域传送。  
**/nonsecure**  
指定授予所有区域传送请求。  
**/securens**  
指定该区域的名称服务器 (NS) 资源记录中列出的服务器授予传输。  
**/securelist**  
指定区域传送仅授予服务器的列表。 此参数必须跟一个 IP 地址或主服务器使用的地址。  
**<SecurityIPaddresses>**  
列出从主服务器接收区域传送的 IP 地址。 此参数仅用于 **/securelist**参数。  
**/nonotify |通知 / |/notifylist <NotifyIPaddresses>**  
指定仅对某些辅助服务器发送更改通知：  
**/nonotify**  
指定没有更改通知将发送到辅助服务器。  
**/notify**  
指定更改通知将发送到所有辅助服务器。  
**/notifylist**  
指定更改通知将发送到服务器的列表。 此命令必须跟一个 IP 地址或主服务器使用的地址。  
**<NotifyIPaddresses>**  
指定的 IP 地址或地址的辅助服务器或服务器的通知发送到的更改。 此列表仅用于 **/notifylist**参数。 # # # 注释-使用**zoneresetsecondaries**命令在主服务器，以指定如何对区域传送请求响应来自辅助服务器上。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
更改区域类型。 # # # 语法```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/ overwrite_mem | /overwrite_ds] ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示本地计算机语法、 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
标识在其更改的类型的区域。  
**<Zonetype>**  
指定要创建区域的类型。 每个类型具有不同的必需的参数：  
**/dsprimary**  
创建 active directory 集成的区域。  
**/primary /file <FileName>**  
创建标准主要区域。  
**/secondary <MasterIPaddress> [,<MasterIPaddress>...]**  
创建一个标准的辅助区域。  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] /file <FileName>**  
创建文件备份的存根区域。  
**/dsstub <MasterIPaddress>[,<MasterIPaddress>...]**  
创建 active directory 集成的存根区域。  
**/转发器<MasterIPaddress[,<MasterIPaddress>].../ 文件<FileName>**  
指定创建的区域将转发到另一台 DNS 服务器未解析的查询。  
**/dsforwarder**  
指定创建的 active directory 集成的区域将转发到另一台 DNS 服务器未解析的查询。  
**/overwrite_mem | /overwrite_ds**  
指定覆盖现有数据的方式：  
**/overwrite_mem**  
将覆盖 DNS 数据从 AD DS 中的数据。  
**/overwrite_ds**  
覆盖 AD DS 中的现有数据。 # # # 注释-设置区域键入作为 **/dsforwarder**创建区域执行条件性转发。 # # # 示例使用情况
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
开始之前已暂停的指定的区域。 # # # 语法```  
dnscmd <ServerName>/zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要恢复的区域的名称。 # # # M a k-可以使用此操作以反转[zonepause](#BKMK_27)操作。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
更新 AD DS 中指定的 active directory 集成的区域。 # # # 语法```  
dnscmd <ServerName>/zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要更新的区域的名称。 # # # 注释-active directory 集成的区域执行此更新默认情况下每隔五分钟。 若要更改此参数，请使用**dnscmd 配置 dspollinginterval**命令。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
检查 DNS 服务器内存中以与指定的区域相关的更改并将其写入到持久存储。 # # # 语法```  
dnscmd <ServerName>/zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
指定 DNS 服务器来管理，由表示 IP 地址、 FQDN 或主机名。 如果省略此参数，则使用本地服务器。  
**<ZoneName>**  
指定要更新的区域的名称。 # # # M a k-这是一个区域级操作。 您可以更新与在 DNS 服务器上的所有区域[writebackfiles](#BKMK_21)操作。 # # # 示例使用情况 `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
