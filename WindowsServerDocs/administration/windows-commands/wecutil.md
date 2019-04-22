---
title: wecutil
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 50151a322f1ccde2be927de31b0e5c30732b278e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813948"
---
# <a name="wecutil"></a>wecutil



可以创建和管理来自远程计算机的转发的事件的订阅。 在远程计算机必须支持 WS 管理协议。 有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。


## <a name="syntax"></a>语法

```
wecutil  [{es | enum-subscription}] 
[{gs | get-subscription} <Subid> [/f:<Format>] [/uni:<Unicode>]] 
[{gr | get-subscriptionruntimestatus} <Subid> [<Eventsource> …]] 
[{ss | set-subscription} [<Subid> [/e:[<Subenabled>]] [/esa:<Address>] [/ese:[<Srcenabled>]] [/aes] [/res] [/un:<Username>] [/up:<Password>] [/d:<Desc>] [/uri:<Uri>] [/cm:<Configmode>] [/ex:<Expires>] [/q:<Query>] [/dia:<Dialect>] [/tn:<Transportname>] [/tp:<Transportport>] [/dm:<Deliverymode>] [/dmi:<Deliverymax>] [/dmlt:<Deliverytime>] [/hi:<Heartbeat>] [/cf:<Content>] [/l:<Locale>] [/ree:[<Readexist>]] [/lf:<Logfile>] [/pn:<Publishername>] [/essp:<Enableport>] [/hn:<Hostname>] [/ct:<Type>]] [/c:<Configfile> [/cun:<Username> /cup:<Password>]]] 
[{cs | create-subscription} <Configfile> [/cun:<Username> /cup:<Password>]] 
[{ds | delete-subscription} <Subid>] 
[{rs | retry-subscription} <Subid> [<Eventsource>…]] 
[{qc | quick-config} [/q:[<Quiet>]]].
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|{es\|枚举订阅}|显示所有存在的远程事件订阅的名称。|
|{gs\|获取订阅} \<Subid > [/ f:\<格式 >] [/ 单元：\<Unicode >]|显示远程订阅配置信息。 \<Subid > 是一个字符串，唯一标识订阅。 \<Subid > 中指定的字符串相同\<SubscriptionId > 标记的 XML 配置文件用于创建订阅。|
|{gr \| get subscriptionruntimestatus} \<Subid > [\<Eventsource >...]|显示订阅的运行时状态。 \<Subid > 是一个字符串，唯一标识订阅。 \<Subid > 中指定的字符串相同\<SubscriptionId > 标记的 XML 配置文件用于创建订阅。 \<Eventsource > 是一个字符串，标识用作事件的源的计算机。 \<Eventsource > 应为完全限定的域名、 NetBIOS 名称或 IP 地址。|
|{ss\|设置订阅} \<Subid > [/ e: [\<Subenabled >]] [/ esa:\<地址 >] [/ ese: [\<Srcenabled >]] [/aes] [/res] [/ 运行：\<用户名 >] [/ 最多：\<密码 >] [/ d:\<Desc >] [/ uri:\<Uri >] [/ cm:\<Configmode >] [/ ex:\<Expires >] [/ 问：\<查询 >] [/ dia:\<方言 >] [/ tn:\<Transportname >] [/ tp:\<Transportport >] [/ 数据挖掘：\<Deliverymode >] [/ dmi:\<Deliverymax >] [/ dmlt:\<Deliverytime >] [/ 大家好：\<检测信号 >] [/ cf:\<内容 >] [/ l:\<区域设置 >] [/ ree: [\<Readexist >]] [/ lf:\<日志文件 >] [/ pn:\<Publishername >] [/ essp:\<Enableport >] [/ hn:\<主机名 >] [/ ct:\<类型 >]</br>或</br>{ss\|设置订阅无\<Configfile > [/ cun:\<Comusername >/茶杯：\<Compassword >]|订阅配置更改。 你可以指定订阅 ID 和相应的选项来更改订阅的参数，也可以指定要更改订阅参数的 XML 配置文件。|
|{cs\|创建订阅} \<Configfile > [/ cun:\<用户名 >/茶杯：\<密码 >]|创建远程订阅。 \<Configfile > 指定包含订阅配置的 XML 文件的路径。 路径可以是绝对或相对于当前目录。|
|{ds\|删除订阅} \<Subid >|删除的订阅并从所有事件源将事件传递到事件日志中的订阅取消订阅。 不会删除已收到并记录任何事件。 \<Subid > 是一个字符串，唯一标识订阅。 \<Subid > 中指定的字符串相同\<SubscriptionId > 标记的 XML 配置文件用于创建订阅。|
|{rs\|重试订阅} \<Subid > [\<Eventsource >...]|重新尝试建立连接，并将远程订阅请求发送到非活动订阅。 指定事件源或尝试重新激活的所有事件源。 已禁用的源不会重试。 \<Subid > 是一个字符串，唯一标识订阅。 \<Subid > 中指定的字符串相同\<SubscriptionId > 标记的 XML 配置文件用于创建订阅。 \<Eventsource > 是一个字符串，标识用作事件的源的计算机。 \<Eventsource > 应为完全限定的域名、 NetBIOS 名称或 IP 地址。|
|{qc \| quick-config} [/q:[\<Quiet>]]|配置 Windows 事件收集器服务，以确保可以创建订阅，并将其重新启动后可持续。 这包括以下步骤：</br>1.如果它处于禁用状态，请启用 ForwardedEvents 通道。</br>2.设置要推迟启动的 Windows 事件收集器服务。</br>3.如果未运行，请启动 Windows 事件收集器服务。|

## <a name="options"></a>选项

|Option|描述|
|------|-----------|
|/f:\<格式 >|指定的格式显示的信息。 \<格式 > 可以是 XML 或 Terse。 如果<Format>为 XML，以 XML 格式显示输出。 如果\<格式 > 是 Terse，输出会显示在名称 / 值对。 默认值为 Terse。|
|无\<Configfile >|指定包含订阅配置的 XML 文件的路径。 路径可以是绝对或相对于当前目录。 此选项仅可用于 **/cun**并 **/茶杯**选项并与所有其他选项互斥。|
|/e:[\<Subenabled>]|启用或禁用订阅。 \<Subenabled > 可以是 true 或 false。 此选项的默认值为 true。|
|/esa:\<地址 >|指定事件源的地址。 \<地址 > 是一个字符串，包含完全限定的域名、 NetBIOS 名称或 IP 地址，它标识用作事件的源的计算机。 应使用此选项与 **/ese**， **/aes**， **/res**，或者 **/un**并 **/up**选项。|
|/ese:[\<Srcenabled>]|启用或禁用事件源。 \<Srcenabled > 可以是 true 或 false。 允许使用此选项仅当 **/esa**指定选项。 此选项的默认值为 true。|
|/aes|添加指定的事件源 **/esa**选项如果它已不是订阅的一部分。 如果指定的地址 **/esa**选项已订阅的一部分，会报告错误。 如果此选项仅允许 **/esa**指定选项。|
|/res|删除由指定的事件源 **/esa**选项如果它已是订阅的一部分。 如果指定的地址 **/esa**选项不订阅的一部分，会报告错误。 如果此选项仅允许 **/esa**指定选项。|
|/un:\<用户名 >|指定要使用指定的事件源的用户凭据 **/esa**选项。 如果此选项仅允许 **/esa**指定选项。|
|/ 向上：\<密码 >|指定对应于用户凭据的密码。 如果此选项仅允许 **/un**指定选项。|
|/d:\<Desc >|提供的订阅的说明。|
|/uri:\<Uri >|指定供订阅的事件的类型。 \<Uri > 包含结合要唯一标识事件源的事件源计算机的地址的 URI 字符串。 URI 字符串用于在订阅中的所有事件源地址。|
|/cm:\<Configmode>|将配置模式设置。 \<Configmode > 可以是下列字符串之一：Normal、 自定义、 MinLatency 或 MinBandwidth。 Normal、 MinLatency 和 MinBandwidth 模式设置传递模式、 交付最大项、 检测信号间隔和传递最大延迟时间。 **/Dm**， **/dmi**， **/hi**或者 **/dmlt**选项只能指定是否配置模式设置为自定义。|
|/ ex:\<过期 >|设置订阅的过期的时间。 \<过期 > 应定义采用标准的 XML 或 ISO8601 日期时间格式： yyyy-MM-ddThh:mm:ss [.sss] [Z]，其中 T 是时间分隔符，Z 表示 UTC 时间。|
|/q:\<查询 >|指定订阅的查询字符串。 格式\<查询 > 可能会因不同的 URI 值并将应用于订阅中的所有源。|
|/dia:\<方言 >|定义查询字符串使用的方言。|
|/tn:\<Transportname >|指定用于连接到远程事件源的传输的名称。|
|/tp:\<Transportport >|设置传输到远程事件源连接时使用的端口号。|
|/dm:\<Deliverymode >|指定传递模式。 \<Deliverymode > 可以是请求或推送。 此选项才有效如果 **/cm**选项设置为自定义。|
|/dmi:\<Deliverymax >|设置为批处理传递的项的最大数目。 此选项才有效如果 **/cm**设置为自定义。|
|/dmlt:\<Deliverytime >|在提供一批的事件设置的最大延迟。 \<Deliverytime > 是的毫秒数。 此选项才有效如果 **/cm**设置为自定义。|
|/ 大家好：\<检测信号 >|定义的检测信号间隔。 \<检测信号 > 是的毫秒数。 此选项才有效如果 **/cm**设置为自定义。|
|/cf:\<内容 >|指定返回的事件的格式。 \<内容 > 可以是事件或 RenderedText。 RenderedText 的值时，事件将返回具有附加到事件 （如事件描述中） 的本地化字符串。 默认值为 RenderedText。|
|/l:\<区域设置 >|RenderedText 格式指定的已本地化的字符串传递的区域设置。 \<区域设置 > 是一个语言和国家/地区的标识符，例如，"EN-我们"。 此选项才有效如果 **/cf**选项设置为 RenderedText。|
|/ree:[\<Readexist>]|标识的订阅传递的事件。 \<Readexist > 可以，则返回 true 或 false。 当<Readexist>为 true，从订阅事件源读取所有现有的事件。 当<Readexist>是仅将来 （到达） 的事件传递为 false。 默认值为 true 的 **/ree**没有值的选项。 如果没有 **/ree**指定选项，默认值为 false。|
|/lf:\<日志文件 >|指定用于存储从事件源接收的事件在本地事件日志。|
|/pn:\<Publishername>|指定发布者名称。 它必须是发布者拥有或导入指定的日志 **/lf**选项。|
|/essp:\<Enableport >|指定的端口号必须附有远程服务的服务主体名称。 \<Enableport > 可以是 true 或 false。 追加端口号时<Enableport>为 true。 追加端口号，可能需要一些配置以防止从要拒绝对事件源的访问。|
|/hn:\<主机名 >|指定本地计算机的 DNS 名称。 此名称将用作远程事件源要推送回事件，且必须仅用于推送订阅。|
|/ct:\<类型 >|设置远程源访问权限的凭据类型。 \<类型 > 应为以下值之一： 默认、 协商、 摘要式、 basic 或 localmachine。 默认值是默认值。|
|/cun:\<Comusername >|设置要用于不具有其自己的用户凭据的事件源的共享的用户凭据。 如果此选项指定与 **/c**选项用户名和用户密码设置的配置文件中的单个事件源将被忽略。 如果你想要为特定事件源使用其他凭据，则应通过指定覆盖此值 **/un**并 **/up**特定事件源选项在命令行上的另一个**ss**命令。|
|/ 茶杯：\<Compassword >|设置共享的用户凭据的用户密码。 当\<Compassword > 设置为 * （星号），从控制台读取密码。 此选项才有效 **/cun**指定选项。|
|/q:[\<Quiet>]|指定是否配置过程会提示进行确认。 \<静默 > 可以是 true 或 false。 如果<Quiet>为 true，配置过程不会提示进行确认。 此选项的默认值为 false。|

## <a name="remarks"></a>备注

> [!IMPORTANT]
> 如果你收到的消息，"RPC 服务器不可用？当你尝试运行 wecutil，您需要启动 Windows 事件收集器服务 (wecsvc)。 若要启动 wecsvc，请在提升的命令提示符，键入 net 启动 wecsvc。

-   下面的示例演示配置文件的内容：  
    ```
    <Subscription xmlns="https://schemas.microsoft.com/2006/03/windows/events/subscription">
    <Uri>https://schemas.microsoft.com/wbem/wsman/1/windows/EventLog</Uri>
    <!-- Use Normal (default), Custom, MinLatency, MinBandwidth -->
    <ConfigurationMode>Normal</ConfigurationMode>
    <Description>Forward Sample Subscription</Description>
    <SubscriptionId>SampleSubscription</SubscriptionId>
    <Query><![CDATA[
    <QueryList>
    <Query Path="Application">
    <Select>*</Select>
    </Query>
    </QueryList>
    ]]></Query>
    <EventSources>
    <EventSource Enabled="true">
    <Address>mySource.myDomain.com</Address>
    <UserName>myUserName</UserName>
    <Password>*</Password>
    </EventSource>
    </EventSources>
    <CredentialsType>Default</CredentialsType>
    <Locale Language="EN-US"></Locale>
    </Subscription>
    ```

## <a name="BKMK_examples"></a>示例

输出名为 sub1 的订阅的配置信息：
```
wecutil gs sub1
```
输出示例：
```
EventSource[0]:
Address: localhost
Enabled: true
Description: Subscription 1
Uri: wsman:microsoft/logrecord/sel
DeliveryMode: pull
DeliveryMaxSize: 16000
DeliveryMaxItems: 15
DeliveryMaxLatencyTime: 1000
HeartbeatInterval: 10000
Locale:
ContentFormat: renderedtext
LogFile: HardwareEvents
```
显示一个名为 sub1 订阅的运行时状态：
```
wecutil gr sub1
```
更新名为从新的 XML 文件调用 WsSelRg2.xml sub1 订阅配置：
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
更新订阅配置名为 sub2 报表具有多个参数：
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
创建订阅以将事件从 mySource.myDomain.com 在远程计算机的 Windows Vista 应用程序事件日志转发到 ForwardedEvents 日志 （请参阅备注有关的配置文件示例）：
```
wecutil cs subscription.xml
```
删除 sub1 名为的订阅：
```
wecutil ds sub1
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
