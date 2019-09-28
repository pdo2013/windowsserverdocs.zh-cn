---
title: ipconfig
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4bfe3476dd90016291881ca8cee2b66283772bce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375382"
---
# <a name="ipconfig"></a>ipconfig



显示所有当前 TCP/IP 网络配置值并刷新动态主机配置协议（DHCP）和域名系统（DNS）设置。 在不使用参数的情况下， **ipconfig**显示所有适配器的 Internet 协议版本4（IPv4）和 IPv6 地址、子网掩码和默认网关。

## <a name="syntax"></a>语法

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/all|显示所有适配器的完整 TCP/IP 配置。 适配器可表示物理接口（例如已安装的网络适配器）或逻辑接口（如拨号连接）。|
|/allcompartments|显示所有隔离舱的完整 TCP/IP 配置。|
|/displaydns|显示 DNS 客户端解析程序缓存的内容，其中包括从本地主机文件预加载的条目，以及由计算机解析的名称查询的任何最近获取的资源记录。 DNS 客户端服务使用此信息在查询其配置的 DNS 服务器之前快速解决经常查询的名称。|
|/flushdns|刷新并重置 DNS 客户端解析程序缓存的内容。 在 DNS 疑难解答过程中，可以使用此过程从缓存中丢弃否定缓存条目，以及动态添加的任何其他条目。|
|/registerdns|为在计算机上配置的 DNS 名称和 IP 地址启动手动动态注册。 你可以使用此参数对失败的 DNS 名称注册进行故障排除，或解决客户端与 DNS 服务器之间的动态更新问题，而无需重新启动客户端计算机。 TCP/IP 协议的高级属性中的 DNS 设置确定哪些名称在 DNS 中注册。|
|/release [\<Adapter >]|向 DHCP 服务器发送一条 DHCPRELEASE 消息，以释放当前的 DHCP 配置，并放弃所有适配器（如果未指定适配器）的 IP 地址配置，或者如果包含*适配器*参数，则丢弃特定适配器的 IP 地址配置。 此参数为配置为自动获取 IP 地址的适配器禁用 TCP/IP。 若要指定适配器名称，请键入在不带参数的情况下使用**ipconfig**时显示的适配器名称。|
|/release6 [\<Adapter >]|向 DHCPv6 服务器发送一条 DHCPRELEASE 消息，以释放当前的 DHCP 配置，并放弃所有适配器（如果未指定适配器，则为）或特定适配器（如果包含*适配器*参数）中的 IPv6 地址配置。 此参数为配置为自动获取 IP 地址的适配器禁用 TCP/IP。 若要指定适配器名称，请键入在不带参数的情况下使用**ipconfig**时显示的适配器名称。|
|/renew [\<Adapter >]|续订所有适配器的 DHCP 配置（如果未指定适配器）; 如果包含*适配器*参数，则为特定适配器续订 DHCP 配置。 此参数仅在配置为自动获取 IP 地址的适配器的计算机上可用。 若要指定适配器名称，请键入在不带参数的情况下使用**ipconfig**时显示的适配器名称。|
|/renew6 [\<Adapter >]|续订所有适配器的 DHCPv6 配置（如果未指定适配器），或为特定适配器续订 DHCPv6 配置（如果包含*适配器*参数）。 此参数仅在配置为自动获取 IPv6 地址的适配器的计算机上可用。 若要指定适配器名称，请键入在不带参数的情况下使用**ipconfig**时显示的适配器名称。|
|/setclassid \<Adapter > [<ClassID>]|配置指定适配器的 DHCP 类 ID。 若要设置所有适配器的 DHCP 类 ID，请使用星号（ **&#42;** ）通配符代替*适配器*。 此参数仅在配置为自动获取 IP 地址的适配器的计算机上可用。 如果未指定 DHCP 类 ID，则删除当前类 ID。|
|/showclassid \<Adapter >|显示指定适配器的 DHCP 类 ID。 若要查看所有适配器的 DHCP 类 ID，请使用星号（ **&#42;** ）通配符代替*适配器*。 此参数仅在配置为自动获取 IP 地址的适配器的计算机上可用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- 在配置为自动获取 IP 地址的计算机上，此命令最有用。 这样，用户便可以确定哪些 TCP/IP 配置值已由 DHCP 配置、自动专用 IP 寻址（APIPA）或备用配置。
- 如果为*适配器*提供的名称包含空格，请使用引号将适配器名称括起来（例如： **"** <em>适配器名称</em> **"** ）。
- 对于适配器名称， **ipconfig**支持使用星号（\*）通配符来指定名称以指定字符串开头的适配器，或使用包含指定字符串的适配器的名称。 例如， **local @ no__t**与所有以字符串 local 开头的适配器和 **\*Con @ no__t**匹配包含该字符串 Con 的所有适配器。

## <a name="examples"></a>示例

若要显示所有适配器的基本 TCP/IP 配置，请键入：
```
ipconfig
```
若要显示所有适配器的完整 TCP/IP 配置，请键入：
```
ipconfig /all
```
若要仅为本地区域连接适配器续订 DHCP 分配的 IP 地址配置，请键入：
```
ipconfig /renew "Local Area Connection"
```
若要在排除 DNS 名称解析问题疑难解答时刷新 DNS 解析程序缓存，请键入：
```
ipconfig /flushdns
```
若要显示名称以 Local 开头的所有适配器的 DHCP 类 ID，请键入：
```
ipconfig /showclassid Local*
```
若要设置要测试的本地连接适配器的 DHCP 类 ID，请键入：
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
