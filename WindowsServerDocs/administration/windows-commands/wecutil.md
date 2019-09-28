---
title: wecutil
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c82a6cb-d652-429c-9c3d-0f568c78d54b
author: coreyp-at-msft
ms.author: coreyp
manager: dansimps
ms.openlocfilehash: 78005a715a0dbd20124bfb24be27586a8e153310
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362188"
---
# <a name="wecutil"></a>wecutil



使你能够创建和管理从远程计算机转发的事件的订阅。 远程计算机必须支持 WS-MANAGEMENT 协议。 有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。


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
|{es \| 枚举-订阅}|显示存在的所有远程事件订阅的名称。|
|{gs \| 获取-订阅} \<Subid > [/f： \<Format >] [/uni： \<Unicode >]|显示远程订阅配置信息。 \<Subid > 是唯一标识订阅的字符串。 \<Subid > 与用于创建订阅的 XML 配置文件的 \<SubscriptionId > 标记中指定的字符串相同。|
|{gr \| subscriptionruntimestatus} \<Subid > [\<Eventsource > ...]|显示订阅的运行时状态。 \<Subid > 是唯一标识订阅的字符串。 \<Subid > 与用于创建订阅的 XML 配置文件的 \<SubscriptionId > 标记中指定的字符串相同。 \<Eventsource > 是一个字符串，用于标识用作事件源的计算机。 \<Eventsource > 应为完全限定的域名、NetBIOS 名称或 IP 地址。|
|{ss \| 集-订阅} \<Subid > [/e： [\<Subenabled >]] [/esa： \<Address >] [/ese] [4Srcenabled] [/aes：/res] [/un] [5Username： @no__t-/up >] [6Password： 7Desc @no__t] [/uri： >__t-8Uri >] [/cm： @no__t 9Configmode >] [/ex： 0Expires >] [/q： 1Query >] [/dia： 2Dialect >] [/tn： 3Transportname >] [14Transportport：/dm @no__t] [15Deliverymode： >16Deliverymax >] [/dmlt： 7Deliverytime >] [/hi： 8Heartbeat >] [/cf： 19Content @no__t] [/l： >-20Locale @no__t] [/ree： [>-21Readexist @no__t]] [/lf： >-22Logfile @no__t] [/essp： 4Enableport >] [/hn： 5Hostname >] [/ct： 26Type @no__t]</br>或</br>{ss \| 集-订阅/c： \<Configfile > [/cun： \<Comusername >/cup： \<Compassword >]|更改订阅配置。 您可以指定订阅 ID 以及用于更改订阅参数的适当选项，也可以指定一个 XML 配置文件来更改订阅参数。|
|{cs \| create-订阅} \<Configfile > [/cun： \<Username >/cup： \<Password >]|创建远程订阅。 \<Configfile > 指定包含订阅配置的 XML 文件的路径。 路径可以是相对于当前目录的绝对路径或相对路径。|
|{ds \| 删除-订阅} \<Subid >|删除订阅并从将事件传递给订阅的事件日志的所有事件源取消订阅。 已收到并记录的任何事件都不会被删除。 \<Subid > 是唯一标识订阅的字符串。 \<Subid > 与用于创建订阅的 XML 配置文件的 \<SubscriptionId > 标记中指定的字符串相同。|
|{rs \| retry} \<Subid > [\<Eventsource > ...]|重试建立连接并向非活动订阅发送远程订阅请求。 尝试重新激活所有事件源或指定的事件源。 不会重试禁用的源。 \<Subid > 是唯一标识订阅的字符串。 \<Subid > 与用于创建订阅的 XML 配置文件的 \<SubscriptionId > 标记中指定的字符串相同。 \<Eventsource > 是一个字符串，用于标识用作事件源的计算机。 \<Eventsource > 应为完全限定的域名、NetBIOS 名称或 IP 地址。|
|{qc \| 快速配置}[/q： [\<Quiet >]]|配置 Windows 事件收集器服务，以确保可以通过重新启动来创建和保持订阅的持续时间。 这包括以下步骤：</br>1.如果 ForwardedEvents 通道处于禁用状态，则启用它。</br>2.将 Windows 事件收集器服务设置为 "延迟启动"。</br>3.如果 Windows 事件收集器服务未运行，则启动该服务。|

## <a name="options"></a>选项

|Option|描述|
|------|-----------|
|/f： \<Format >|指定显示的信息的格式。 \<Format > 可以是 XML 或简要。 如果 @no__t 为 XML，则输出以 XML 格式显示。 如果 \<Format > 为简要，则输出将以名称/值对显示。 默认值为简要。|
|/c： \<Configfile >|指定包含订阅配置的 XML 文件的路径。 路径可以是相对于当前目录的绝对路径或相对路径。 此选项只能与 **/cun**和 **/cup**选项一起使用，并且与所有其他选项互相排斥。|
|/e： [\<Subenabled >]|启用或禁用订阅。 \<Subenabled > 可以为 true 或 false。 此选项的默认值为 true。|
|/esa： \<Address >|指定事件源的地址。 \<Address > 是包含完全限定的域名、NetBIOS 名称或 IP 地址的字符串，用于标识用作事件源的计算机。 应将此选项与 **/ese**、 **/aes**、 **/res**、 **/un**和 **/up**选项一起使用。|
|/ese： [\<Srcenabled >]|启用或禁用事件源。 \<Srcenabled > 可以为 true 或 false。 仅当指定了 **/esa**选项时，才允许使用此选项。 此选项的默认值为 true。|
|/aes|如果 **/esa**选项不是订阅的一部分，则添加由该选项指定的事件源。 如果 **/esa**选项指定的地址已是订阅的一部分，则会报告错误。 仅当指定了 **/esa**选项时，才允许使用此选项。|
|/res|如果 **/esa**选项已是订阅的一部分，则删除由该选项指定的事件源。 如果 **/esa**选项指定的地址不是订阅的一部分，则会报告错误。 仅当指定了 **/esa**选项时，才允许使用此选项。|
|/un： \<Username >|指定与 **/esa**选项指定的事件源一起使用的用户凭据。 仅当指定了 **/esa**选项时，才允许使用此选项。|
|/up： \<Password >|指定对应于用户凭据的密码。 仅当指定了 **/un**选项时，才允许使用此选项。|
|/d： \<Desc >|提供订阅的说明。|
|/uri： \<Uri >|指定订阅使用的事件类型。 \<Uri > 包含一个 URI 字符串，该字符串与事件源计算机的地址相结合，用于唯一标识事件的源。 URI 字符串用于订阅中的所有事件源地址。|
|/cm： \<Configmode >|设置配置模式。 \<Configmode > 可以为以下字符串之一：Normal、Custom、MinLatency 或 MinBandwidth。 Normal、MinLatency 和 MinBandwidth 模式设置传递模式、传递最大项、检测信号间隔和最大传输延迟时间。 仅当配置模式设置为 "自定义" 时，才能指定 **/dm**、 **/dmi**、 **/hi**或 **/dmlt**选项。|
|/ex： \<Expires >|设置订阅的过期时间。 \<Expires > 应采用标准 XML 格式或 ISO8601 日期-时间格式定义： yyyy-mm-dd： Yyyy-mm-ddthh： MM： ss [. sss] [Z]，其中 T 为时间分隔符，Z 指示 UTC 时间。|
|/q： \<Query >|指定订阅的查询字符串。 不同的 URI 值 \<Query > 的格式可能不同，并适用于订阅中的所有源。|
|/dia： \<Dialect >|定义查询字符串使用的方言。|
|/tn： \<Transportname >|指定用于连接到远程事件源的传输的名称。|
|/tp： \<Transportport >|设置连接到远程事件源时传输使用的端口号。|
|/dm： \<Deliverymode >|指定传送模式。 \<Deliverymode > 可以是请求或推送。 仅当 **/cm**选项设置为 "自定义" 时，此选项才有效。|
|/dmi： \<Deliverymax >|设置批处理传递的最大项数。 仅当 **/cm**设置为 Custom 时，此选项才有效。|
|/dmlt： \<Deliverytime >|设置提供一批事件时的最大延迟。 \<Deliverytime > 是毫秒数。 仅当 **/cm**设置为 Custom 时，此选项才有效。|
|/hi： \<Heartbeat >|定义检测信号间隔。 \<Heartbeat > 是毫秒数。 仅当 **/cm**设置为 Custom 时，此选项才有效。|
|/cf： \<Content >|指定返回的事件的格式。 \<Content > 可以是事件或 RenderedText。 如果值为 RenderedText，则会返回附加到事件的已本地化字符串（例如事件描述）的事件。 默认值为 RenderedText。|
|/l： \<Locale >|指定以 RenderedText 格式传递本地化字符串的区域设置。 \<Locale > 是语言和国家/地区标识符，例如 "EN-US"。 仅当 **/cf**选项设置为 RenderedText 时，此选项才有效。|
|/ree： [\<Readexist >]|标识为订阅传递的事件。 \<Readexist > 为 true 或 false。 如果 @no__t 为 true，则会从订阅事件源中读取所有现有事件。 如果 @no__t 为 false，则只传递未来的（到达的）事件。 对于没有值的 **/ree**选项，默认值为 true。 如果未指定 **/ree**选项，则默认值为 false。|
|/lf： \<Logfile >|指定用于存储从事件源接收的事件的本地事件日志。|
|/pn： \<Publishername >|指定发布服务器名称。 它必须是拥有或导入 **/lf**选项指定的日志的发布者。|
|/essp： \<Enableport >|指定必须将端口号附加到远程服务的服务主体名称。 \<Enableport > 可以为 true 或 false。 如果 @no__t 为 true，则会追加端口号。 如果附加了端口号，则可能需要某些配置以防止拒绝对事件源的访问。|
|/hn： \<Hostname >|指定本地计算机的 DNS 名称。 此名称由远程事件源用来推送回事件，并且必须仅用于推送订阅。|
|/ct： \<Type >|设置远程源访问的凭据类型。 \<Type > 应为以下值之一： default、negotiate、digest、basic 或 localmachine。 默认值为 "默认值"。|
|/cun： \<Comusername >|设置要用于没有自己的用户凭据的事件源的共享用户凭据。 如果此选项与 **/c**选项一起指定，则将忽略配置文件中各个事件源的用户名和 UserPassword 设置。 如果要将不同的凭据用于特定事件源，则应通过在另一个**ss**命令的命令行上指定特定事件源的 **/un**和 **/up**选项来重写此值。|
|/cup： \<Compassword >|设置共享用户凭据的用户密码。 如果 \<Compassword > 设置为 * （星号），则从控制台读取密码。 仅当指定了 **/cun**选项时，此选项才有效。|
|/q： [\<Quiet >]|指定配置过程是否提示确认。 \<Quiet > 可以为 true 或 false。 如果 @no__t 为 true，则配置过程不会提示进行确认。 此选项的默认值为 false。|

## <a name="remarks"></a>备注

> [!IMPORTANT]
> 如果收到消息 "RPC 服务器不可用？尝试运行 wecutil 时，需要启动 Windows 事件收集器服务（wecsvc）。 若要启动 wecsvc，请在提升的命令提示符下键入 net start wecsvc。

- 下面的示例显示了配置文件的内容：  
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
显示名为 sub1 的订阅的运行时状态：
```
wecutil gr sub1
```
从名为 WsSelRg2 的新 XML 文件中更新名为 sub1 的订阅配置：
```
wecutil ss sub1 /c:%Windir%\system32\WsSelRg2.xml
```
用多个参数更新名为 sub2 的订阅配置：
```
wecutil ss sub2 /esa:myComputer /ese /un:uname /up:* /cm:Normal
```
创建一个订阅，用于将事件从远程计算机的 Windows Vista 应用程序事件日志转发到 ForwardedEvents 日志（有关配置文件的示例，请参阅备注）：
```
wecutil cs subscription.xml
```
删除名为 sub1 的订阅：
```
wecutil ds sub1
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
