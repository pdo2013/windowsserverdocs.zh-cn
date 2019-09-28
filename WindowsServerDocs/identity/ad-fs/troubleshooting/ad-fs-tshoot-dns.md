---
title: AD FS 故障排除-DNS 解析
description: 本文档介绍如何排查 AD FS 的 DNS 方面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7ffda6916bd91f1195ac0c289959becafff1d2c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407201"
---
# <a name="ad-fs-troubleshooting---dns"></a>AD FS 疑难解答-DNS 
首先要检查的是，如果 AD FS 不起作用或未响应，则是 DNS 名称解析。  这些是基本测试，用于确定在网络中是否找到 AD FS 服务器或 WAP 服务器。  对于内部用户，这些测试应该解析为 AD FS 服务器（STS）。    对于外部用户，这些测试应该解析为 WAP 服务器。

本文档的其余部分将演示如何使用命令行工具执行快速名称解析检查。

## <a name="ping-test"></a>Ping 测试
通过发送 Internet 控制消息协议（ICMP）回送请求消息验证与另一台 TCP/IP 计算机的 IP 级连接。 将显示相应的回响回复消息以及往返时间。  有关详细信息，请参阅[Ping](https://technet.microsoft.com/library/ff961503.aspx)。


>[!NOTE]
>请注意，某些组织会在其服务器上阻止此端口，并且你可能会收到**请求超时**响应。

### <a name="to-use-a-ping-test"></a>使用 PING 测试
1.  打开命令提示符
2. 输入 PING <name of adfs server> a。 例如：PING sts.contoso.com
3. 你应看到来自服务器的答复

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
显示可用于诊断域名系统（DNS）基础结构的信息。  有关详细信息，请参阅[NSLookup](https://technet.microsoft.com/library/cc725991.aspx)。

### <a name="to-use-a-nslookup"></a>使用 NSLookup
1.  打开命令提示符
2. 输入 PING <name of adfs server> a。 示例： nslookup sts.contoso.com
3. 应会看到服务器 @no__t 的 dns 信息-0NSLookup @ no__t-1

## <a name="tracert"></a>Tracert
通过将 Internet 控制消息协议（ICMP）回送请求或 ICMPv6 消息发送到目标，以递增递增的生存时间（TTL）字段值，确定目标的路径。   有关详细信息，请参阅[Tracert](https://technet.microsoft.com/library/ff961507.aspx)。


### <a name="to-use-tracert"></a>使用 Tracert
1.  打开命令提示符
2. 输入 tracert <name of adfs server> a。 示例： tracert sts.contoso.com
3. 应该会看到用于访问服务器的目标路径 ![Tracert @ no__t-1

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)