---
title: DNS 记录注册的 DHCP 日志记录事件
description: 本主题提供有关 DHCP 服务器的信息在 Windows Server 2016 中的日志记录事件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816898"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DNS 注册的 DHCP 日志记录事件

>适用于：Windows 服务器 （半年频道），Windows Server 2016

现在，DHCP 服务器事件日志提供有关 DNS 注册错误的详细的信息。

>[!NOTE]
>在许多情况下，DHCP 服务器的 DNS 记录注册失败的原因是 DNS 反向\-查找区域配置不正确或未完全配置。

以下新的 DHCP 事件帮助轻松识别时 DNS 注册失败由于配置不正确或缺失的 DNS 反向\-查找区域。

|ID|Event|值|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|%1 的 IPv4 地址和 FQDN %2 的正向记录注册失败，错误为 %3。 这很可能是因为 DNS 服务器上不存在此记录的正向查找区域。|
|20318|DHCPv4.ForwardRecordDNSTimeout|%1 的 IPv4 地址和 FQDN %2 的正向记录注册失败，错误为 %3。|
|20319|DHCPv4.PTRRecordDNSFailure|%1 的 IPv4 地址和 FQDN %2 的 PTR 记录注册失败，错误为 %3。 这很可能是因为 DNS 服务器上不存在此记录的反向查找区域。|
|20320|DHCPv4.PTRRecordDNSTimeout|%1 的 IPv4 地址和 FQDN %2 的 PTR 记录注册失败，错误为 %3。|
|20321|DHCPv6.ForwardRecordDNSFailure|IPv6 地址 %1 和 FQDN %2 的正向记录注册失败，错误为 %3。 这很可能是因为 DNS 服务器上不存在此记录的正向查找区域。|
|20322|DHCPv6.ForwardRecordDNSTimeout|IPv6 地址 %1 和 FQDN %2 的正向记录注册失败，错误为 %3。|
|20323|DHCPv6.PTRRecordDNSFailure|IPv6 地址 %1 和 FQDN %2 的 PTR 记录注册失败，错误为 %3。 这很可能是因为 DNS 服务器上不存在此记录的反向查找区域。|
|20324|DHCPv6.PTRRecordDNSTimeout|IPv6 地址 %1 和 FQDN %2 的 PTR 记录注册失败，错误为 %3。|
|20325|DHCPv4.ForwardRecordDNSError|%1 的 IPv4 地址和 FQDN %2 PTR 记录注册失败，出现错误 %3 \(%4\)。|
|20326|DHCPv6.ForwardRecordDNSError|有关 IPv6 地址 %1 和 FQDN %2 正向记录注册失败，出现错误 %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|有关 IPv6 地址 %1 和 FQDN %2 PTR 记录注册失败，出现错误 %3 \(%4\)。|

