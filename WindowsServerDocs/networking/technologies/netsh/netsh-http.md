---
title: Netsh 命令的超文本传输协议 (HTTP)
description: 使用 netsh http 查询和配置的 HTTP.sys 设置和参数。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daecc6a385d7aa2c02d1bea02eb0a585a9b33185
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881788"
---
# <a name="netsh-http-commands"></a>Netsh http 命令


使用**netsh http**查询和配置的 HTTP.sys 设置和参数。  

>[!TIP]
>如果在运行 Windows Server 2016 或 Windows 10 的计算机上使用 Windows PowerShell，键入**netsh**然后按 Enter。 在 netsh 提示符下键入**http**然后按 Enter 以启动 netsh http 提示符。
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

可用 netsh http 命令是：

- [添加 iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [添加超时](#add-timeout)
- [添加 urlacl](#add-urlacl)
- [删除缓存](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [删除超时](#delete-timeout)
- [删除 urlacl](#delete-urlacl)
- [刷新 logbuffer](#flush-logbuffer)
- [显示 cachestate](#show-cachestate)
- [显示 iplisten](#show-iplisten)
- [显示 servicestate](#show-servicestate)
- [显示 sslcert](#show-sslcert)
- [显示超时](#show-timeout)
- [显示 urlacl](#show-urlacl)

## <a name="add-iplisten"></a>添加 iplisten

将新的 IP 地址添加到 IP 侦听列表中，不包括端口号。

**语法**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parameters**
| | | |
|---|---|---|
| **ipaddress** | IPv4 或 IPv6 地址，要添加到的 IP 侦听列表。 IP 侦听列表用于作用域的地址的 HTTP 服务绑定的列表。 "0.0.0.0"意味着任何 IPv4 地址和"::"意味着任何 IPv6 地址。 | 必需 |
---

**示例**

以下是四个示例的**添加 iplisten**命令。

-   添加 iplisten ipaddress = fe80:: 1
-   添加 iplisten ipaddress = 1.1.1.1
-   添加 iplisten ipaddress = 0.0.0.0
-   添加 iplisten ipaddress =::

---

## <a name="add-sslcert"></a>添加 sslcert

添加新的 SSL 服务器证书绑定和对应的 IP 地址和端口的客户端证书策略。

**语法**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parameters**

| | | |
|---|---|---|
| **ipport**                                   | 指定的 IP 地址和绑定的端口。 冒号字符 （:）用作分隔符之间的 IP 地址和端口号。                                              | 必需 |
| **certhash**                                 | 指定证书的 SHA 哈希。 此哈希值是 20 个字节长，并指定为十六进制字符串。                                                                          | 必需 |
| **appid**                                    | 指定 GUID 来标识所属应用程序。                                                                                                                                   | 必需 |
| **certstorename**                            | 指定证书存储名称。 默认值为 MY。 证书必须存储在本地计算机上下文中。                                                                   | 可选 |
| **verifyclientcertrevocation**               | 指定打开/关闭客户端证书吊销验证启用。                                                                                                            | 可选 |
| **verifyrevocationwithcachedclientcertonly** | 指定是启用还是禁用仅缓存客户端证书的吊销检查的使用情况。                                                                            | 可选 |
| **usagecheck**                               | 指定是启用还是禁用使用情况检查。 默认已启用。                                                                                                            | 可选 |
| **revocationfreshnesstime**                  | 指定的时间间隔，以秒为单位，以检查更新的证书吊销列表 (CRL)。 如果此值为零，则仅当上一个过期更新新的 CRL。 | 可选 |
| **urlretrievaltimeout**                      | 尝试检索证书吊销列表的远程 URL 后指定的超时间隔 （以毫秒为单位）。                                                       | 可选 |
| **sslctlidentifier**                         | 指定可以是受信任的证书颁发者的列表。 此列表可以是计算机信任的证书颁发者的子集。                                | 可选 |
| **sslctlstorename**                          | 指定本地计算机存储 SslCtlIdentifier 下的证书存储区名称。                                                                                               | 可选 |
| **dsmapperusage**                            | 指定是启用还是禁用 DS 映射器。 默认为禁用。                                                                                                                | 可选 |
| **clientcertnegotiation**                    | 指定是启用还是禁用证书的协商。 默认为禁用。                                                                                            | 可选 |
---

**示例**

下面是举例**添加 sslcert**命令。

添加 sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>添加超时

将全局超时添加到该服务。

**语法** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parameters**
| | |
|---|---|
| **timeouttype** | 设置超时值的类型。                                                                        |
| **value**       | 超时 （以秒为单位） 的值。 如果值为十六进制表示法中，然后添加前缀 0 x。 |
---

**示例**

以下是两个示例**添加超时**命令。

-   add timeout timeouttype=idleconnectiontimeout value=120
-   添加超时 timeouttype = headerwaittimeout 值 = 0x40

---

## <a name="add-urlacl"></a>添加 urlacl

添加统一资源定位器 (URL) 保留项。 此命令保留非管理员用户和帐户 URL。 使用侦听和委托参数，将 NT 帐户名称或使用的 SDDL 字符串，可以指定 DACL。

**语法**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parameters**
| | | |
|---|---|---|
| **url**       | 指定完全限定的统一资源定位器 (URL)。                                                                                    | 必需 |
| **user**      | 指定用户组名称                                                                                                            | 必需 |
| **listen**    | 指定以下值之一: 是:允许用户注册 Url。 这是默认值。 不：拒绝该用户注册 Url。 | 可选 |
| **delegate**  | 指定以下值之一: 是:允许用户不委派 Url:拒绝从委派 Url 的用户。 这是默认值。   | 可选 |
| **sddl**      | 指定描述 DACL 的 SDDL 字符串。                                                                                                | 可选 |
---

**示例**

以下是四个示例的**添加 urlacl**命令。

-   add urlacl =https://+:80/MyUri用户 = DOMAIN\\用户
-   add urlacl =https://www.contoso.com:80/MyUri用户 = DOMAIN\\用户侦听 = yes
-   add urlacl =https://www.contoso.com:80/MyUri用户 = DOMAIN\\用户委托 = 否
-   add urlacl =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>删除缓存

从 HTTP 服务内核 URI 缓存中删除所有项或指定的项。

**语法**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parameters**
| | | |
|---|---|---|
| **url**       | 指定完全限定统一资源定位器 (URL)，你想要删除。                                        | 可选 |
| **recursive** | 指定是否删除 url 缓存下的所有项。 **是**： 删除所有条目**没有**： 不都删除所有条目 | 可选 |
---

**示例**

以下是两个示例**删除缓存**命令。

-   删除缓存 url =https://www.contoso.com:80/myresource/递归 = yes
-   删除缓存

---

## <a name="delete-iplisten"></a>删除 iplisten

从 IP 侦听列表中删除 IP 地址。 IP 侦听列表用于作用域的地址的 HTTP 服务绑定的列表。

**语法**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parameters**
| | | |
|---|---|---|
| **ipaddress** | IPv4 或 IPv6 地址，以删除来自 IP 侦听列表。 IP 侦听列表用于作用域的地址的 HTTP 服务绑定的列表。 "0.0.0.0"意味着任何 IPv4 地址和"::"意味着任何 IPv6 地址。 这不包括端口号。 | 必需 |
---


**示例**

以下是四个示例的**删除 iplisten**命令。

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   删除 iplisten ipaddress =::

---

## <a name="delete-sslcert"></a>删除 sslcert


删除 SSL 服务器证书绑定和相应的客户端证书策略的 IP 地址和端口。

**语法**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parameters**
| | | |
|---|---|---|
| **ipport** | 指定的 IPv4 或 IPv6 地址和端口的 SSL 证书绑定会为其删除。 冒号字符 （:）用作分隔符之间的 IP 地址和端口号。 | 必需 |
---


**示例**

以下是三个示例**删除 sslcert**命令。

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>删除超时

删除全局超时，并使服务恢复为默认值。

**语法**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parameters**
| | | |
|---|---|---|
| **timeouttype** | 指定的超时设置的类型。 | 必需 |
---


**示例**

以下是两个示例**删除超时**命令。

-   删除超时 timeouttype = idleconnectiontimeout
-   删除超时 timeouttype = headerwaittimeout

---

## <a name="delete-urlacl"></a>删除 urlacl

删除 URL 保留项。

**语法**

```powershell
delete urlacl [ url= ] URL
```

**Parameters**
| | | |
|---|---|---|
| **url** | 指定完全限定统一资源定位器 (URL)，你想要删除。 | 必需 |
---


**示例**

以下是两个示例**删除 urlacl**命令。

-   delete urlacl =https://+:80/MyUri
-   delete urlacl =https://www.contoso.com:80/MyUri

---

## <a name="flush-logbuffer"></a>刷新 logbuffer

刷新日志文件的内部缓冲区。

**语法**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>显示 cachestate

列出缓存的 URI 的资源和其关联的属性。 此命令将列出所有资源及其关联的属性在 HTTP 响应缓存中缓存或显示单个资源及其关联的属性。

**语法**

```powershell
show cachestate [ [url= ] URL]
```

**Parameters**
| | | |
|---|---|---|
| **url** | 指定你想要显示的完全限定的 URL。 如果未指定，显示的所有 Url。 URL 也可能是为已注册 Url 前缀。 | 可选 |
---


**示例**

以下是两个示例**显示 cachestate**命令：

-   显示 cachestate url =https://www.contoso.com:80/myresource
-   显示 cachestate

---

## <a name="show-iplisten"></a>显示 iplisten

IP 侦听列表中显示的所有 IP 地址。 IP 侦听列表用于作用域的地址的 HTTP 服务绑定的列表。 "0.0.0.0"意味着任何 IPv4 地址和"::"意味着任何 IPv6 地址。

**语法**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>显示 servicestate

显示 HTTP 服务的快照。

**语法**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parameters**
| | | |
|---|---|---|
| **视图**    | 指定是否要查看基于服务器会话或请求队列的 HTTP 服务状态的快照。 | 可选 |
| **Verbose** | 指定是否显示详细信息，还显示了属性信息。                               | 可选 |
---

**示例**

以下是两个示例**显示 servicestate**命令。

-   显示 servicestate 视图 ="会话"
-   显示 servicestate 视图 ="requestq"

---

## <a name="show-sslcert"></a>显示 sslcert

显示安全套接字层 (SSL) 服务器证书绑定和相应的客户端证书策略的 IP 地址和端口。

**语法**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parameters**
| | | |
|---|---|---|
| **ipport** | 指定的 IPv4 或 IPv6 地址和端口的 SSL 证书绑定显示。 冒号字符 （:）用作分隔符之间的 IP 地址和端口号。 如果不指定 ipport，将显示所有绑定。 | 必需 |
---


**示例**

以下是的五个示例**显示 sslcert**命令。

-   show sslcert ipport=[fe80::1]:443
-   显示 sslcert ipport = 1.1.1.1:443
-   显示 sslcert ipport = 0.0.0.0: 443
-   show sslcert ipport=[::]:443
-   显示 sslcert

---

## <a name="show-timeout"></a>显示超时

显示，以秒为单位，HTTP 服务的超时值。

**语法**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>显示 urlacl

显示自定义访问控制列表 (Dacl) 指定保留的 URL 或所有保留的 Url。

**语法**

```powershell
show urlacl [ [url= ] URL]
```

**Parameters**
| | | |
|---|---|---|
| **url** | 指定你想要显示的完全限定的 URL。 如果未指定，显示的所有 Url。 | 可选 |
---


**示例**

以下是三个示例**显示 urlacl**命令。

-   显示 urlacl =https://+:80/MyUri
-   显示 urlacl =https://www.contoso.com:80/MyUri
-   显示 urlacl

---