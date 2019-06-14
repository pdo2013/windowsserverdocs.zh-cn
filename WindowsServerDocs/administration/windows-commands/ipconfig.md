---
title: ipconfig
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fff4e5088e3e08cf2e9e742d8fb6aa2187bb9e83
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438130"
---
# <a name="ipconfig"></a>ipconfig



显示所有当前 TCP/IP 网络配置值，并刷新动态主机配置协议 (DHCP) 和域名系统 (DNS) 设置。 使用不带参数， **ipconfig**显示 Internet 协议版本 4 (IPv4) 和 IPv6 地址、 子网掩码和默认网关的所有适配器。

## <a name="syntax"></a>语法

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/all|显示所有适配器的完整 TCP/IP 配置。 适配器可表示物理接口（例如已安装的网络适配器）或逻辑接口（如拨号连接）。|
|/allcompartments|显示所有的隔离舱的完整 TCP/IP 配置。|
|/displaydns|DNS 客户端解析器缓存，其中包括这两个条目的内容从本地 Hosts 文件和任何预加载的显示最近获得的计算机解析的名称查询的资源记录。 DNS 客户端服务使用此信息来查询其配置的 DNS 服务器之前快速，解决频繁查询的名称。|
|/flushdns|刷新，然后将重置 DNS 客户端解析器缓存的内容。 期间 DNS 故障排除，可以使用此过程中丢弃从缓存中，负缓存条目和动态添加的任何其他条目。|
|/registerdns|启动手动动态注册的 DNS 名称和在计算机上配置的 IP 地址。 此参数可用于排查失败的 DNS 名称注册或解析而无需重启客户端计算机的客户端和 DNS 服务器之间的动态更新问题。 TCP/IP 协议的高级属性中的 DNS 设置将确定哪些名称在 DNS 中注册。|
|/ 发布 [\<适配器 >]|将 DHCPRELEASE 消息发送到 DHCP 服务器以释放当前的 DHCP 配置并放弃所有适配器的 IP 地址配置 （如果未指定适配器） 或为特定的适配器上，如果*适配器*包含参数。 此参数禁用 TCP/IP 的适配器配置为自动获取 IP 地址。 若要指定适配器名称，键入将显示当你使用的适配器名称**ipconfig**不带参数。|
|/release6[\<Adapter>]|将 DHCPRELEASE 消息发送到 DHCPv6 服务器以释放当前的 DHCP 配置并放弃所有适配器的 IPv6 地址配置 （如果未指定适配器） 或为特定的适配器上，如果*适配器*包含参数。 此参数禁用 TCP/IP 的适配器配置为自动获取 IP 地址。 若要指定适配器名称，键入将显示当你使用的适配器名称**ipconfig**不带参数。|
|/ 续订 [\<适配器 >]|如果将续订所有适配器 （如果未指定适配器） 或为特定的适配器的 DHCP 配置*适配器*参数会包括在内。 此参数是仅在具有配置为自动获取 IP 地址的适配器的计算机上可用。 若要指定适配器名称，键入将显示当你使用的适配器名称**ipconfig**不带参数。|
|/renew6 [\<Adapter>]|如果将续订所有适配器 （如果未指定适配器） 或为特定的适配器的 DHCPv6 配置*适配器*参数会包括在内。 此参数是仅在具有配置为自动获取 IPv6 地址的适配器的计算机上可用。 若要指定适配器名称，键入将显示当你使用的适配器名称**ipconfig**不带参数。|
|/setclassid \<Adapter>[ <ClassID>]|配置指定的适配器的 DHCP 类 ID。 若要设置所有适配器的 DHCP 类 ID，请使用星号 ( **&#42;** ) 来代替通配符*适配器*。 此参数是仅在具有配置为自动获取 IP 地址的适配器的计算机上可用。 如果未指定 DHCP 类 ID，将删除当前的类 ID。|
|/showclassid\<适配器 >|显示指定的适配器的 DHCP 类 ID。 若要查看所有适配器的 DHCP 类 ID，请使用星号 ( **&#42;** ) 来代替通配符*适配器*。 此参数是仅在具有配置为自动获取 IP 地址的适配器的计算机上可用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 此命令为配置为自动获取 IP 地址的计算机上最有用。 这使用户能够确定 DHCP、 自动专用 IP 寻址 (APIPA) 或备用的配置已配置的 TCP/IP 配置值。
- 如果您为提供的名称*适配器*包含任何空格，使用引号将适配器名称 (示例： **"** <em>适配器名称</em> **"** )。
- 对于适配器名称， **ipconfig**支持使用星号 ( *) 通配符来指定有适配器，以指定的字符串或适配器名称包含指定的字符串开头的名称。例如，**本地\\** * 匹配本地的字符串开头的所有适配器并 **\*Con\\** * 都匹配包含的所有适配器Con 的字符串。

## <a name="examples"></a>示例

若要显示的所有适配器的基本 TCP/IP 配置，请键入：
```
ipconfig
```
若要显示所有适配器的完整 TCP/IP 配置，请键入：
```
ipconfig /all
```
若要续订只有本地区域连接适配器的 DHCP 分配的 IP 地址配置，请键入：
```
ipconfig /renew "Local Area Connection"
```
若要刷新 DNS 解析器缓存 DNS 名称解析问题进行故障排除时，请键入：
```
ipconfig /flushdns
```
若要显示具有本地开头的所有适配器的 DHCP 类 ID，请键入：
```
ipconfig /showclassid Local*
```
若要为本地区域连接适配器添加到测试设置 DHCP 类 ID，请键入：
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)