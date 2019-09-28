---
title: 用于超文本传输协议（HTTP）的 Netsh 命令
description: 使用 netsh http 查询和配置 HTTP.SYS 设置和参数。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401874"
---
# <a name="netsh-http-commands"></a>Netsh http 命令


使用**netsh http**查询和配置 http.sys 设置和参数。  

>[!TIP]
>如果在运行 Windows Server 2016 或 Windows 10 的计算机上使用 Windows PowerShell，请键入**netsh** ，然后按 enter。 在 netsh 提示符下，键入**http** ，然后按 enter 以获取 netsh http 提示符。
>
>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6netsh http @ no__t-7

可用的 netsh http 命令包括：

- [添加 iplisten](#add-iplisten)
- [添加 sslcert](#add-sslcert)
- [添加超时](#add-timeout)
- [添加 urlacl](#add-urlacl)
- [删除缓存](#delete-cache)
- [删除 iplisten](#delete-iplisten)
- [删除 sslcert](#delete-sslcert)
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

向 IP 侦听列表添加新的 IP 地址，不包括端口号。

**语法**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**参数**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **地址** | 要添加到 IP 侦听列表的 IPv4 或 IPv6 地址。 IP 侦听列表用于确定 HTTP 服务绑定到的地址列表的范围。 "0.0.0.0" 表示任何 IPv4 地址，"：：" 表示任何 IPv6 地址。 | 必需 |

---

**示例**

下面是 "**添加 iplisten** " 命令的四个示例。

-   add iplisten ipaddress = fe80：：1
-   add iplisten ipaddress = 1.1.1。1
-   添加 iplisten ipaddress = 0.0.0。0
-   添加 iplisten ipaddress =：

---

## <a name="add-sslcert"></a>添加 sslcert

为 IP 地址和端口添加新的 SSL 服务器证书绑定和相应的客户端证书策略。

**语法**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**参数**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       指定绑定的 IP 地址和端口。 一个冒号字符（:)用作 IP 地址和端口号之间的分隔符。                        | 必需 |
|                 **certhash**                 |                                     指定证书的 SHA 哈希。 此哈希长度为20个字节，并以十六进制字符串的形式指定。                                      | 必需 |
|                  **appid**                   |                                                                  指定用于标识所属应用程序的 GUID。                                                                  | 必需 |
|              **certstorename**               |                                  指定证书的存储名称。 默认值为 "MY"。 证书必须存储在本地计算机上下文中。                                  | 可选 |
|        **verifyclientcertrevocation**        |                                                      指定对客户端证书吊销的启用/关闭验证。                                                       | 可选 |
| **verifyrevocationwithcachedclientcertonly** |                                      指定是启用还是禁用用于吊销检查的仅缓存客户端证书的使用。                                       | 可选 |
|                **usagecheck**                |                                                      指定是否启用或禁用使用情况检查。 默认为启用。                                                       | 可选 |
|         **revocationfreshnesstime**          | 指定检查更新的证书吊销列表（CRL）的时间间隔（以秒为单位）。 如果此值为零，则仅当上一个 CRL 过期时才会更新新的 CRL。 | 可选 |
|           **urlretrievaltimeout**            |                            指定尝试检索远程 URL 的证书吊销列表之后的超时间隔（毫秒）。                            | 可选 |
|             **sslctlidentifier**             |                指定可信任的证书颁发者的列表。 此列表可以是计算机信任的证书颁发者的子集。                 | 可选 |
|             **sslctlstorename**              |                                                指定存储 SslCtlIdentifier 的 LOCAL_MACHINE 下的证书存储名称。                                                | 可选 |
|              **dsmapperusage**               |                                                        指定是启用还是禁用 DS 映射器。 默认为禁用。                                                         | 可选 |
|          **clientcertnegotiation**           |                                              指定是启用还是禁用证书协商。 默认为禁用。                                               | 可选 |

---

**示例**

下面是**add sslcert**命令的一个示例。

add sslcert ipport = 1.1.1.1： 443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899}

---

## <a name="add-timeout"></a>添加超时

将全局超时添加到服务。

**语法** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**参数**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    设置的超时类型。                                     |
|    **value**    | 超时值（以秒为单位）。 如果值采用十六进制表示法，则添加前缀0x。 |

---

**示例**

下面是 "**添加超时**" 命令的两个示例。

-   add timeout timeouttype = idleconnectiontimeout value = 120
-   add timeout timeouttype = headerwaittimeout value = 0x40

---

## <a name="add-urlacl"></a>添加 urlacl

添加统一资源定位器（URL）预订条目。 此命令保留非管理员用户和帐户的 URL。 可以通过将 NT 帐户名与侦听和委托参数一起使用或使用 SDDL 字符串来指定 DACL。

**语法**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**参数**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **链接**    |                                          指定完全限定的统一资源定位器（URL）。                                           | 必需 |
|   **user**   |                                                      指定用户或用户组名称                                                       | 必需 |
|  **侦听**  | 指定下列值之一： yes：允许用户注册 Url。 这是默认值。 不：拒绝用户注册 Url。 | 可选 |
| **委托** |  指定下列值之一： yes：允许用户委托 Url：拒绝用户委托 Url。 这是默认值。  | 可选 |
|   **sddl**   |                                                指定描述 DACL 的 SDDL 字符串。                                                 | 可选 |

---

**示例**

下面是 "**添加 urlacl** " 命令的四个示例。

- 添加 urlacl url = https://+:80/MyUri user = DOMAIN @ no__t-1user
- add urlacl url = <https://www.contoso.com:80/MyUri> user = DOMAIN @ no__t-1user 侦听 = yes
- 添加 urlacl url = <https://www.contoso.com:80/MyUri> user = DOMAIN @ no__t-1user delegate = no
- 添加 urlacl url = https://+:80/MyUri sddl = 。

---

## <a name="delete-cache"></a>删除缓存

从 HTTP 服务内核 URI 缓存中删除所有条目或指定的条目。

**语法**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**参数**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **链接**    |                    指定要删除的完全限定的统一资源定位器（URL）。                     | 可选 |
| **recursive** | 指定是否删除 url 缓存下的所有条目。 **是**：删除所有项 **，不删除所有项（&** a） | 可选 |

---

**示例**

下面是两个**删除缓存**命令示例。

- 删除缓存 url = <https://www.contoso.com:80/myresource/> recursive = 是
- 删除缓存

---

## <a name="delete-iplisten"></a>删除 iplisten

从 IP 侦听列表中删除 IP 地址。 IP 侦听列表用于确定 HTTP 服务绑定到的地址列表的范围。

**语法**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**参数**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **地址** | 要从 IP 侦听列表中删除的 IPv4 或 IPv6 地址。 IP 侦听列表用于确定 HTTP 服务绑定到的地址列表的范围。 "0.0.0.0" 表示任何 IPv4 地址，"：：" 表示任何 IPv6 地址。 这不包括端口号。 | 必需 |

---


**示例**

下面是 "**删除 iplisten** " 命令的四个示例。

-   delete iplisten ipaddress = fe80：：1
-   delete iplisten ipaddress = 1.1.1。1
-   删除 iplisten ipaddress = 0.0.0。0
-   delete iplisten ipaddress =：

---

## <a name="delete-sslcert"></a>删除 sslcert


删除 IP 地址和端口的 SSL 服务器证书绑定和相应的客户端证书策略。

**语法**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**参数**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定为其删除 SSL 证书绑定的 IPv4 或 IPv6 地址和端口。 一个冒号字符（:)用作 IP 地址和端口号之间的分隔符。 | 必需 |

---


**示例**

下面是 "**删除 sslcert** " 命令的三个示例。

- delete sslcert ipport = 1.1.1.1：443
- delete sslcert ipport = 0.0.0.0：443
- delete sslcert ipport = [：：]：443

---

## <a name="delete-timeout"></a>删除超时

删除全局超时，并使服务还原为默认值。

**语法**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**参数**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | 指定超时设置的类型。 | 必需 |

---


**示例**

下面是 "**删除超时**" 命令的两个示例。

-   delete timeout timeouttype = idleconnectiontimeout
-   delete timeout timeouttype = headerwaittimeout

---

## <a name="delete-urlacl"></a>删除 urlacl

删除 URL 保留项。

**语法**

```powershell
delete urlacl [ url= ] URL
```

**参数**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **链接** | 指定要删除的完全限定的统一资源定位器（URL）。 | 必需 |

---


**示例**

下面是 "**删除 urlacl** " 命令的两个示例。

- 删除 urlacl url = https://+:80/MyUri
- 删除 urlacl url = <https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>刷新 logbuffer

刷新日志的内部缓冲区。

**语法**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>显示 cachestate

列出缓存的 URI 资源及其相关的属性。 此命令列出缓存在 HTTP 响应缓存中的所有资源及其关联的属性，或者显示单个资源及其关联的属性。

**语法**

```powershell
show cachestate [ [url= ] URL]
```

**参数**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **链接** | 指定要显示的完全限定的 URL。 如果未指定，则显示所有 Url。 URL 还可以是注册 Url 的前缀。 | 可选 |

---


**示例**

下面是**show cachestate**命令的两个示例：

- 显示 cachestate url = <https://www.contoso.com:80/myresource>
- 显示 cachestate

---

## <a name="show-iplisten"></a>显示 iplisten

显示 IP 侦听列表中的所有 IP 地址。 IP 侦听列表用于确定 HTTP 服务绑定到的地址列表的范围。 "0.0.0.0" 表示任何 IPv4 地址，"：：" 表示任何 IPv6 地址。

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

**参数**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **视图**   | 指定是否查看基于服务器会话或请求队列的 HTTP 服务状态的快照。 | 可选 |
| **详细** |                指定是否显示还显示属性信息的详细信息。                | 可选 |

---

**示例**

下面是**show servicestate**命令的两个示例。

-   显示 servicestate view = "session"
-   显示 servicestate view = "requestq"

---

## <a name="show-sslcert"></a>显示 sslcert

显示 IP 地址和端口安全套接字层（SSL）服务器证书绑定和相应的客户端证书策略。

**语法**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**参数**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | 指定 SSL 证书绑定显示的 IPv4 或 IPv6 地址和端口。 一个冒号字符（:)用作 IP 地址和端口号之间的分隔符。 如果未指定 ipport，则会显示所有绑定。 | 必需 |

---


**示例**

下面是**show sslcert**命令的五个示例。

-   show sslcert ipport = [fe80：： 1]：443
-   show sslcert ipport = 1.1.1.1：443
-   show sslcert ipport = 0.0.0.0：443
-   show sslcert ipport = [：：]：443
-   显示 sslcert

---

## <a name="show-timeout"></a>显示超时

显示 HTTP 服务的超时值（以秒为单位）。

**语法**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>显示 urlacl

为指定的保留 URL 或所有保留的 Url 显示自由访问控制列表（Dacl）。

**语法**

```powershell
show urlacl [ [url= ] URL]
```

**参数**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **链接** | 指定要显示的完全限定的 URL。 如果不是，则显示所有 Url。 | 可选 |

---


**示例**

以下是**show urlacl**命令的三个示例。

- 显示 urlacl url = https://+:80/MyUri
- 显示 urlacl url = <https://www.contoso.com:80/MyUri>
- 显示 urlacl

---