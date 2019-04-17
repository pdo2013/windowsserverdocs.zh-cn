---
title: 将记录事件 DHCP DNS 记录登记
description: 本主题提供 DHCP 服务器的信息可以在 Windows Server 2016 的事件日志记录。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a5099cd5e1ef1d4687baa8c20411c96ea8f519
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>将记录事件 DHCP DNS 注册

>适用于：Windows Server（半年通道），Windows Server 2016

DHCP 服务器的事件日志中现在提供有关 DNS 注册故障的详细的信息。

>[!NOTE]
>在许多情况下，通过 DHCP 服务器 DNS 记录注册失败的原因是一个 DNS Reverse\ 查找区域是配置不正确或未在所有配置。

以下新 DHCP 事件帮助你轻松地找出故障由于错误配置时缺少 DNS Reverse\ 查找区域 DNS 注册。

|ID|事件|值|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|IPv4 地址 %1 和 FQDN %2 前进记录注册将失败，错误 %3 平板电脑。 这是可能是因为该记录的前进查找区域不存在 DNS 服务器上。|
|20318|DHCPv4.ForwardRecordDNSTimeout|IPv4 地址 %1 和 FQDN %2 前进记录注册将失败，错误 %3 平板电脑。|
|20319|DHCPv4.PTRRecordDNSFailure|IPv4 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 平板电脑。 这是可能是因为该记录的反向查找区域不存在 DNS 服务器上。|
|20320|DHCPv4.PTRRecordDNSTimeout|IPv4 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 平板电脑。|
|20321|DHCPv6.ForwardRecordDNSFailure|IPv6 地址 %1 和 FQDN %2 前进记录注册将失败，错误 %3 平板电脑。 这是可能是因为该记录的前进查找区域不存在 DNS 服务器上。|
|20322|DHCPv6.ForwardRecordDNSTimeout|IPv6 地址 %1 和 FQDN %2 前进记录注册将失败，错误 %3 平板电脑。|
|20323|DHCPv6.PTRRecordDNSFailure|IPv6 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 平板电脑。 这是可能是因为该记录的反向查找区域不存在 DNS 服务器上。|
|20324|DHCPv6.PTRRecordDNSTimeout|IPv6 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 平板电脑。|
|20325|DHCPv4.ForwardRecordDNSError|IPv4 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 \(%4\)。|
|20326|DHCPv6.ForwardRecordDNSError|IPv6 地址 %1 和 FQDN %2 前进记录注册将失败，错误 %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|IPv6 地址 %1 和 FQDN %2 PTR 录制注册将失败，错误 %3 \(%4\)。|

