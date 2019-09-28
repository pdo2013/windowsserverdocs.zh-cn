---
title: 服务质量 (QoS) 策略
description: 本主题概述了服务质量（QoS）策略，该策略允许你使用组策略来确定 Windows Server 2016 中特定应用程序和服务的网络流量带宽的优先级。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 429c38d93c2c5c0053153d538304767c8261229c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395859"
---
# <a name="quality-of-service-qos-policy"></a>服务\(质量 QoS\)策略

>适用于：Windows Server（半年频道）、Windows Server 2016

通过创建其设置使用组策略分发的 QoS 配置文件，你可以在整个 Active Directory 基础架构中，将 QoS 策略用作网络带宽管理的中心点。

>[!NOTE]
>  除了本主题之外，还提供了以下 QoS 策略文档。  
>   
>  - [具有 QoS 策略的入门](qos-policy-get-started.md)
>  - [管理 QoS 策略](qos-policy-manage.md)
>  - [QoS 策略常见问题](qos-policy-faq.md)

QoS 策略将应用于用户登录会话或计算机，作为已链接到 Active Directory 容器的\(组策略\)对象 GPO 的一部分，如域、站点或组织单位\(OU\)。

QoS 流量管理发生在应用程序层下方，这意味着您的现有应用程序无需修改即可受益于 QoS 策略提供的优势。

## <a name="operating-systems-that-support-qos-policy"></a>支持 QoS 策略的操作系统

你可以使用 QoS 策略来管理具有以下 Microsoft 操作系统的计算机或用户的带宽。

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>QoS 策略在组策略的位置

在 Windows Server 2016 组策略管理编辑器中，计算机配置的 QoS 策略的路径如下。

**默认域策略 |计算机配置 |策略 |Windows 设置 |基于\-策略的 QoS**

下图演示了此路径。

![QoS 策略在组策略的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 组策略管理编辑器中，用户配置的 QoS 策略的路径如下所示。

**默认域策略 |用户配置 |策略 |Windows 设置 |基于\-策略的 QoS**

默认情况下，未配置 QoS 策略。

## <a name="why-use-qos-policy"></a>为什么使用 QoS 策略？
  
随着网络流量的增加，对网络性能的平衡与服务成本的平衡日趋重要，但网络流量通常并不易于确定优先级和管理。

在网络上，关键\-任务和延迟\-敏感的应用程序必须在网络带宽与较低优先级的流量之间争夺。 同时，某些具有特定网络性能要求的用户和计算机可能需要区分服务级别。

提供经济高效、可预测的网络性能级别的挑战通常首先出现在广域网\(WAN\)连接上，或带有延迟敏感的应用程序（例如语音 over \(IP VoIP）\)和视频流式处理。 但是，提供可预测的网络服务级别的最终目标适用于任何网络环境\(（例如，企业的局域网\)）和多个 VoIP 应用程序（例如公司的自定义线路）业务\-应用程序。\-
  
基于策略的 QoS 是网络带宽管理工具，提供基于应用程序、用户和计算机的网络控制。 

使用 QoS 策略时，应用程序不需要针对特定的应用程序编程接口\(api\)进行编写。 这使你能够将 QoS 用于现有应用程序。 此外，基于策略的 QoS 充分利用现有的管理基础结构，因为基于策略的 QoS 内置于组策略中。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>通过差分服务代码点\(DSCP 定义 QoS 优先级\)
  
你可以创建 QoS 策略，用于定义网络流量优先级，其中包含分配\(给\)不同网络流量类型的差分服务码位 DSCP 值。 

通过 DSCP，你可以在 IPv4 数据包\(标头中\)的 "Service \(TOS\) " 字段的类型内以及 IPv6 的 "流量类" 字段内应用值0–63。 

DSCP 值提供 Internet 协议\(IP\)级别的网络流量分类，路由器使用这些分类来决定流量队列的行为。 

例如，你可以将路由器配置为将具有特定 DSCP 值的数据包放入三个队列之一：高优先级、最大努力或低于最佳操作。 

高优先级队列中的任务关键网络流量优先于其他流量。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>使用限制速率限制每个应用程序的网络带宽使用

还可以通过在 QoS 策略中指定限制速率来限制应用程序的出站网络流量。

定义阻止限制的 QoS 策略决定出站网络流量的速率。 例如，要管理 WAN 成本，IT 部门可能会实施一项服务级别协议，该协议指定文件服务器绝不会提供超过特定速率的下载。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 策略应用 DSCP 值和限制率

你还可以使用 QoS 策略将出站网络流量的 DSCP 值和限制速率应用于以下内容：

- 发送应用程序和目录路径

- 源和目标 IPv4 或 IPv6 地址或地址前缀

- 协议-传输控制协议\(TCP\)和用户数据报\(协议 UDP\)

- 源和目标端口和端口范围\(TCP 或 UDP\)

- 通过组策略中的部署来指定的用户或计算机组

通过使用这些控件，可以为 VoIP 应用程序指定 DSCP 值为46的 QoS 策略，使路由器能够在低延迟队列中放置 VoIP 数据包，或者可以使用 QoS 策略将一组服务器的出站流量限制为每秒512kb/>443\) KBps。

你还可以将 QoS 策略应用于具有特殊带宽要求的特定应用程序。 有关详细信息，请参阅[QoS 策略方案](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 策略的优点

借助 QoS 策略，你可以配置和强制执行不能在路由器和交换机上配置的 QoS 策略。 QoS 策略具有以下优势。
  
1. **详细级别：** 很难在路由器或交换机上创建用户级 QoS 策略，尤其是在用户的计算机使用动态 IP 地址分配进行配置的情况下，或者如果计算机未连接到固定交换机或路由器端口，便携式计算机。 与此相反，QoS 策略使你可以更轻松地\-在域控制器上配置用户级别的 QoS 策略，并将策略传播到用户的计算机。
2. **灵活性**。 无论计算机连接到网络的位置或方式如何，应用 QoS 策略-计算机可以使用 WiFi 或以太网从任何位置连接。 对于用户\-级别的 qos 策略，会在用户登录的任何位置将 QoS 策略应用于任何兼容的设备。
3. **安全**如果 IT 部门使用 Internet 协议安全\(IPsec\)从端到端加密用户流量，则无法根据数据包\(中 IP 层以上的任何信息（例如TCP 端口\)。 但是，通过使用 QoS 策略，你可以在最终设备上对数据包进行分类，以指示 ip 标头中的数据包优先级，然后再对 IP 负载进行加密并发送数据包。
4. **性能**某些 QoS 功能（如限制）在接近源时更好地执行。 QoS 策略移动此类与源最接近的 QoS 功能。
5. **能力**QoS 策略通过两种方式增强网络可管理性：

    **一个**。 由于它基于组策略，因此，你可以根据需要使用 QoS 策略来配置和管理一组用户/计算机 QoS 策略，并在一个中央域控制器计算机上进行配置和管理。

    **b.** Qos 策略通过提供一种机制，通过统一资源定位符\(URL\)来指定策略，而不是基于需要 QoS 策略的每台服务器的 IP 地址来指定策略，从而简化了用户/计算机配置。要应用的。 例如，假设您的网络中有共享一个公共 URL 的服务器群集。 通过使用 QoS 策略，你可以基于公共 URL 创建一个策略，而不是针对群集中的每个服务器创建一个策略，每个策略基于每个服务器的 IP 地址。

有关本指南的下一个主题，请参阅[具有 QoS 策略入门](qos-policy-get-started.md)。

