---
title: DNS 客户端疑难解答
description: 本文介绍如何从客户端对 DNS 问题进行故障排除。
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 1f18159d6232bd9e7864b13419b3648c12b9f44f
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917814"
---
# <a name="troubleshooting-dns-clients"></a>DNS 客户端疑难解答

本文介绍如何解决来自 DNS 客户端的问题。

## <a name="check-ip-configuration"></a>检查 IP 配置

1. 在客户端计算机上以管理员身份打开命令提示符窗口。

2. 运行下面的命令：

   ```cmd
   ipconfig /all
   ```

3. 验证客户端是否具有有效的 IP 地址、子网掩码以及它所附加和使用的网络的默认网关。

4. 检查输出中列出的 DNS 服务器, 并验证列出的 IP 地址是否正确。

5. 检查输出中特定于连接的 DNS 后缀并验证它是否正确。

如果客户端没有有效的 TCP/IP 配置, 请使用以下方法之一:

* 对于动态配置的客户端, `ipconfig /renew`请使用命令手动强制客户端使用 DHCP 服务器续订其 IP 地址配置。

* 对于静态配置的客户端, 修改客户端 TCP/IP 属性以使用有效的配置设置或完成网络的 DNS 配置。

## <a name="check-network-connection"></a>检查网络连接

### <a name="ping-test"></a>Ping 测试

验证客户端是否可以通过使用首选 DNS 服务器的 IP 地址对其进行 ping 操作来联系首选 (或备用) DNS 服务器。

例如, 如果客户端使用的首选 DNS 服务器为 10.0.0.1, 请在命令提示符下运行以下命令:

```cmd
ping 10.0.0.1
```

如果配置的 DNS 服务器没有响应其 IP 地址的直接 ping, 这表示问题的根源更可能是客户端与 DNS 服务器之间的网络连接。 如果是这种情况, 请遵循基本 TCP/IP 网络故障排除步骤来解决此问题。 请记住, 若要使 ping 命令正常工作, 必须通过防火墙允许 ICMP 通信。

### <a name="dns-query-tests"></a>DNS 查询测试

如果 dns 客户端可以对 dns 服务器计算机执行 ping 操作, 请尝试使用`nslookup`以下命令来测试服务器是否可响应 dns 客户端。 由于 nslookup 不使用客户端的 DNS 缓存, 因此名称解析将使用客户端的配置的 DNS 服务器。

#### <a name="test-a-client"></a>测试客户端

```cmd
nslookup <client>
```
  
例如, 如果客户端计算机名为**client1**, 请运行以下命令:
  
```cmd
nslookup client1
```
  
如果未返回成功的响应, 请尝试运行以下命令:
  
```cmd
nslookup <fqdn of client>
```
  
例如, 如果 FQDN 为**client1.corp.contoso.com**, 请运行以下命令:

```cmd
nslookup client1.corp.contoso.com.
```

> [!NOTE]
> 运行此测试时, 必须包含尾随句点。

如果 Windows 成功找到了 FQDN 但找不到短名称, 请检查 NIC 的 "高级 TCP/IP 设置" 的 "DNS" 选项卡上的 DNS 后缀配置。 有关详细信息, 请参阅[配置 DNS 解析](https://docs.microsoft.com/previous-versions/tn-archive/dd163570(v=technet.10)#configuring-dns-resolution)。

#### <a name="test-the-dns-server"></a>测试 DNS 服务器

```cmd
nslookup <DNS Server>
```

例如, 如果 DNS 服务器名为 DC1, 请运行以下命令:

```cmd
nslookup dc1
```
如果前面的测试成功, 则此测试也应该会成功。 如果此测试未成功, 请验证与 DNS 服务器的连接。

#### <a name="test-the-failing-record"></a>测试失败记录

```cmd
nslookup <failed internal record>
```

例如, 如果失败记录为**app1.corp.contoso.com**, 请运行以下命令:

```cmd
nslookup app1.corp.contoso.com
```

#### <a name="test-a-public-internet-address"></a>测试公共 Internet 地址

```cmd
nslookup <external name>
```

例如： 
```cmd
nslookup bing.com
```

如果所有这四项测试均成功, 请`ipconfig /displaydns`运行并检查输出中是否有失败的名称。 如果在失败名称下看到 "名称不存在", 则表示从 DNS 服务器返回了否定响应, 并缓存在客户端上。 

若要解决此问题, 请通过运行`ipconfig /flushdns`来清除缓存。

## <a name="next-step"></a>下一步

如果名称解析仍失败, 请参阅[DNS 服务器故障排除](troubleshoot-dns-server.md)部分。
