---
title: DNS 记录注册的 DHCP 日志记录事件
description: 本主题提供有关 Windows Server 2016 中的 DHCP 服务器日志记录事件的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccd8024af30f1103afa8eac52926a6b42d32940a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355424"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DNS 注册的 DHCP 日志记录事件

>适用于：Windows Server（半年频道）、Windows Server 2016

DHCP 服务器事件日志现在提供了有关 DNS 注册失败的详细信息。

>[!NOTE]
>在许多情况下，DHCP 服务器的 DNS 记录注册失败的原因是 DNS 反向 @ no__t-0Lookup 区域配置不正确或根本未进行配置。

以下新的 DHCP 事件可帮助你轻松识别 DNS 注册何时由于配置错误或缺少 DNS 反向 @ no__t-0Lookup 区域而失败。

|id|Event|ReplTest1|
|-----|--------------------|--------------------------------------------------------|
|20317|ForwardRecordDNSFailure|IPv4 地址% 1 和 FQDN% 2 的转发记录注册失败，出现错误% 3。 这很可能是因为在 DNS 服务器上不存在此记录的正向查找区域。|
|20318|ForwardRecordDNSTimeout|IPv4 地址% 1 和 FQDN% 2 的转发记录注册失败，出现错误% 3。|
|20319|PTRRecordDNSFailure|IPv4 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3。 这很可能是因为在 DNS 服务器上不存在此记录的反向查找区域。|
|20320|PTRRecordDNSTimeout|IPv4 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3。|
|20321|ForwardRecordDNSFailure|IPv6 地址% 1 和 FQDN% 2 的转发记录注册失败，出现错误% 3。 这很可能是因为在 DNS 服务器上不存在此记录的正向查找区域。|
|20322|ForwardRecordDNSTimeout|IPv6 地址% 1 和 FQDN% 2 的转发记录注册失败，出现错误% 3。|
|20323|PTRRecordDNSFailure|IPv6 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3。 这很可能是因为在 DNS 服务器上不存在此记录的反向查找区域。|
|20324|PTRRecordDNSTimeout|IPv6 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3。|
|20325|ForwardRecordDNSError|IPv4 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3 \(% 4 @ no__t。|
|20326|ForwardRecordDNSError|IPv6 地址% 1 和 FQDN% 2 的转发记录注册失败，出现错误% 3 \(% 4 @ no__t-1|
|20327|PTRRecordDNSError|IPv6 地址% 1 和 FQDN% 2 的 PTR 记录注册失败，出现错误% 3 \(% 4 @ no__t。|

