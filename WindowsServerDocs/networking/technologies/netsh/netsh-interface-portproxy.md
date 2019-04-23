---
title: 用于接口端口代理的 Netsh 命令
description: 使用 netsh 接口端口代理命令为 IPv4 和 IPv6 网络和应用程序之间的代理。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 194a418fe6b33e312a3f2529e82d50d76cd15f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842478"
---
# <a name="netsh-interface-portproxy-commands"></a>Netsh 界面端口代理命令

使用**netsh 接口端口代理**命令为 IPv4 和 IPv6 网络和应用程序之间的代理。 这些命令可用于建立代理服务在以下方面：

-   配置 IPv4 的计算机和应用程序发送到其他 IPv4 配置计算机和应用程序的消息。

-   配置 IPv4 的计算机和应用程序的消息发送到 IPv6 配置计算机和应用程序。

-   配置了 IPv6 的计算机和应用程序的消息发送到 IPv4 配置计算机和应用程序。

-   配置了 IPv6 的计算机和应用程序发送到其他配置了 IPv6 的计算机和应用程序的消息。

每个命令时编写批处理文件或脚本使用以下命令，必须首先具有**netsh 接口端口代理**。 例如，在使用**删除 v4tov6**命令指定端口代理服务器从服务器在侦听 IPv4 地址的列表中删除一个 IPv4 端口和地址、 批处理文件或脚本必须使用以下语法：

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

可用 netsh 接口端口代理命令是：

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [全部显示](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>添加 v4tov4

端口代理服务器侦听的端口和 IPv4 地址发送的单独 TCP 连接建立后收到的消息发送到特定端口和 IPv4 地址和映射的消息。

### <a name="syntax"></a>语法

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters


| | |
|-----|--------|----------|
| **listenport**     | 指定的 IPv4 端口，端口号或服务名称，要侦听。                                                                                                                      | 必需 |
| **connectaddress** | 指定要连接到的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv4 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="add-v4tov6"></a>添加 v4tov6

端口代理服务器侦听的端口和 IPv6 地址发送的单独 TCP 连接建立后收到的消息发送到特定端口和 IPv4 地址和映射的消息。

### <a name="syntax"></a>语法

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-----------|-------------|----------|
| **listenport**     | 指定的 IPv4 端口，端口号或服务名称，要侦听。       | 必需 |
| **connectaddress** | 指定要连接的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv6 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="add-v6tov4"></a>添加 v6tov4

端口代理服务器侦听的消息发送到特定端口和 IPv6 地址和端口和 IPv4 地址向其发送的单独 TCP 连接建立后收到的消息的映射。

### <a name="syntax"></a>语法

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|------------|-------------|----------|
| **listenport**     | 指定的 IPv6 端口，端口号或服务名称，要侦听。              | 必需 |
| **connectaddress** | 指定要连接到的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv4 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **protocol**       | 指定要使用的协议。      |          |

## <a name="add-v6tov6"></a>添加 v6tov6

端口代理服务器侦听的消息发送到特定端口和 IPv6 地址和端口和 IPv6 地址向其发送的单独 TCP 连接建立后收到的消息的映射。

### <a name="syntax"></a>语法

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-------------|------------------|----------|
| **listenport**     | 指定的 IPv6 端口，端口号或服务名称，要侦听。       | 必需 |
| **connectaddress** | 指定要连接的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv6 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="delete-v4tov4"></a>删除 v4tov4

端口代理服务器的 IPv4 端口和服务器在侦听的地址列表从删除的 IPv4 地址。

### <a name="syntax"></a>语法

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要删除的 IPv4 端口。                                                                       | 必需 |
| **listenaddress** | 指定要删除的 IPv4 地址。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**      | 指定要使用的协议。                                                                           |          |

## <a name="delete-v4tov6"></a>delete v4tov6

端口代理服务器从服务器在侦听 IPv4 地址的列表中删除一个 IPv4 端口和地址。

### <a name="syntax"></a>语法

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要删除的 IPv4 端口。                                                                       | 必需 |
| **listenaddress** | 指定要删除的 IPv4 地址。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**      | 指定要使用的协议。                                                                           |          |

## <a name="delete-v6tov4"></a>删除 v6tov4

端口代理服务器从服务器在侦听 IPv6 地址的列表中删除一个 IPv6 端口和地址。

### <a name="syntax"></a>语法

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要删除的 IPv6 端口。                                                                       | 必需 |
| **listenaddress** | 指定要删除的 IPv6 地址。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**      | 指定要使用的协议。                                                                           |          |

## <a name="delete-v6tov6"></a>删除 v6tov6

端口代理服务器从服务器在侦听 IPv6 地址的列表中删除的 IPv6 地址。

### <a name="syntax"></a>语法

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | 指定要删除的 IPv6 端口。                                                                       | 必需 |
| **listenaddress** | 指定要删除的 IPv6 地址。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**      | 指定要使用的协议。                                                                           |          |

## <a name="reset"></a>reset

重置的 IPv6 配置状态。

### <a name="syntax"></a>语法

`reset`

## <a name="set-v4tov4"></a>集 v4tov4

修改与创建的端口代理服务器上的现有条目的参数值**添加 v4tov4**命令，或将新条目添加到列表，映射端口/地址对。

### <a name="syntax"></a>语法

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|--------------------|---------------------------|----------|
| **listenport**     | 指定的 IPv4 端口，端口号或服务名称，要侦听。     | 必需 |
| **connectaddress** | 指定要连接到的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv4 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="set-v4tov6"></a>set v4tov6

修改与创建的端口代理服务器上的现有条目的参数值**添加 v4tov6**命令，或将新条目添加到列表，映射端口/地址对。

### <a name="syntax"></a>语法

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|--------------------|---------------------|----------|
| **listenport**     | 指定的 IPv4 端口，端口号或服务名称，要侦听。     | 必需 |
| **connectaddress** | 指定要连接的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv6 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="set-v6tov4"></a>set v6tov4

修改与创建的端口代理服务器上的现有条目的参数值**添加 v6tov4**命令，或将新条目添加到列表，映射端口/地址对。

### <a name="syntax"></a>语法

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|--------------------|----------------------|----------|
| **listenport**     | 指定的 IPv6 端口，端口号或服务名称，要侦听。      | 必需 |
| **connectaddress** | 指定要连接到的 IPv4 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。 |          |
| **connectport**    | 指定的 IPv4 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。              |          |
| **listenaddress**  | 指定要侦听的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                    |          |

## <a name="set-v6tov6"></a>set v6tov6

修改与创建的端口代理服务器上的现有条目的参数值**添加 v6tov6**命令，或将新条目添加到列表，映射端口/地址对。

### <a name="syntax"></a>语法

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parameters

|   |   |
|--------------------|-------------------------|----------|
| **listenport**     | 指定的 IPv6 端口，端口号或服务名称，要侦听。   | 必需 |
| **connectaddress** | 指定要连接的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定地址，则默认为本地计算机。  |          |
| **connectport**    | 指定的 IPv6 端口，端口号或服务名称，要连接到。 如果**connectport**未指定，默认的值为**为 listenport**本地计算机上。               |          |
| **listenaddress**  | 指定要侦听的 IPv6 地址。 可接受的值是 IP 地址、 计算机 NetBIOS 名称或计算机 DNS 名称。 如果不指定一个地址，则默认为本地计算机。 |          |
| **protocol**       | 指定要使用的协议。                                                                                                                                                                     |          |

## <a name="show-all"></a>显示所有

显示所有端口代理参数，包括端口/地址对 v4tov4、 v4tov6、 v6tov4 和 v6tov6 的。

### <a name="syntax"></a>语法

`show all`


## <a name="show-v4tov4"></a>显示 v4tov4

显示 v4tov4 端口代理参数。

### <a name="syntax"></a>语法

`show v4tov4`

## <a name="show-v4tov6"></a>显示 v4tov6

显示 v4tov6 端口代理参数。

### <a name="syntax"></a>语法

`show v4tov6`

## <a name="show-v6tov4"></a>显示 v6tov4

显示 v6tov4 端口代理参数。

### <a name="syntax"></a>语法

`show v6tov4`


## <a name="show-v6tov6"></a>显示 v6tov6

显示 v6tov6 端口代理参数。

### <a name="syntax"></a>语法

`show v6tov6`

---
