---
title: AD FS 故障排除-DNS 解析
description: 本文档介绍如何排查 DNS 方面的 AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815898"
---
# <a name="ad-fs-troubleshooting---dns"></a>AD FS 故障排除-DNS 
若要检查，如果没有工作或响应，AD FS 的首要任务之一是 DNS 名称解析。  这些都是基本测试以确定是否在 AD FS 服务器或 WAP 服务器网络上找到。  对于内部用户，这些测试应解析为 AD FS 服务器 (STS)。    对于外部用户，这些测试应解析为 WAP 服务器。

此文档的其余部分将演示如何进行一些快速名称解析检查使用命令行工具。

## <a name="ping-test"></a>Ping 测试
通过发送 Internet 控制消息协议 (ICMP) 回送请求消息验证与其他 TCP/IP 计算机的 IP 级连接性。 会显示相应回送答复消息的回执，以及往返次数。  有关详细信息，请参阅[Ping](https://technet.microsoft.com/library/ff961503.aspx)。


>[!NOTE]
>请注意，某些组织中阻止其服务器上的此端口，可能会收到**请求已超时**响应。

### <a name="to-use-a-ping-test"></a>若要使用 PING 测试
1.  打开命令提示符
2. 输入 PING <name of adfs server> 。 例如：PING sts.contoso.com
3. 您应看到来自服务器的响应

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
显示可用于诊断域名系统 (DNS) 基础结构的信息。  有关详细信息，请参阅[NSLookup](https://technet.microsoft.com/library/cc725991.aspx)。

### <a name="to-use-a-nslookup"></a>若要使用 NSLookup
1.  打开命令提示符
2. 输入 PING <name of adfs server> 。 示例： nslookup sts.contoso.com
3. 你应看到服务器的 dns 信息![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
确定由发送 Internet 控制消息协议 (ICMP) 回送请求或具有以增量方式增加时间 (TTL) 字段值为目的地的 ICMPv6 消息到目标所采用的路径。   有关详细信息，请参阅[Tracert](https://technet.microsoft.com/library/ff961507.aspx)。


### <a name="to-use-tracert"></a>若要使用 Tracert
1.  打开命令提示符
2. 输入 tracert <name of adfs server> 。 示例： tracert sts.contoso.com
3. 您应看到用来连接到服务器的目标路径![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)