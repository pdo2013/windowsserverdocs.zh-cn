---
title: 服务质量 (QoS) 策略
description: 本主题概述了服务质量 (QoS) 策略，它允许您使用组策略为优先处理的特定应用程序和 Windows Server 2016 中的服务的网络流量带宽。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820278"
---
# <a name="quality-of-service-qos-policy"></a>服务质量\(QoS\)策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

通过创建其设置使用组策略分发的 QoS 配置文件，你可以在整个 Active Directory 基础架构中，将 QoS 策略用作网络带宽管理的中心点。

>[!NOTE]
>  除了本主题提供了以下的 QoS 策略文档。  
>   
>  - [开始使用 QoS 策略](qos-policy-get-started.md)
>  - [管理 QoS 策略](qos-policy-manage.md)
>  - [QoS 策略方面的常见问题](qos-policy-faq.md)

QoS 策略应用到用户登录会话或计算机作为组策略对象的一部分\(GPO\)的已链接到 Active Directory 容器，如域、 站点或组织单位\(OU\)。

QoS 通信管理出现以下应用程序层，这意味着您现有的应用程序不需要进行修改，以从提供的 QoS 策略的优势中受益。

## <a name="operating-systems-that-support-qos-policy"></a>支持 QoS 策略的操作系统

可以使用 QoS 策略为计算机或具有以下 Microsoft 操作系统的用户的带宽进行管理。

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

### <a name="location-of-qos-policy-in-group-policy"></a>在组策略的 QoS 策略的位置

Windows Server 2016 组策略管理编辑器，在计算机的配置 QoS 策略的路径是以下。

**默认域策略 |计算机配置 |策略 |Windows 设置 |策略\-基于 QoS**

下图说明了此路径。

![在组策略的 QoS 策略的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 组策略管理编辑器，用户配置的 QoS 策略的路径是以下。

**默认域策略 |用户配置 |策略 |Windows 设置 |策略\-基于 QoS**

默认情况下不配置任何 QoS 策略。

## <a name="why-use-qos-policy"></a>为何使用 QoS 策略？
  
随着网络上的流量增加，日益重要，以便平衡网络性能的服务的成本，但网络流量不是通常容易优先级并管理。

在网络上的任务关键型\-关键和延迟\-敏感应用程序必须争夺对较低优先级的流量的网络带宽。 同时，一些用户和计算机特定的网络性能要求可能需要使用不同服务级别。

提供经济高效、 可预测网络性能级别的挑战通常第一次出现通过广域网\(WAN\)连接或易受延迟影响的应用程序，如 IP 语音\(VoIP\)和视频流式处理。 但是，最终目标是提供可预测的网络服务级别应用于任何网络环境\(例如，企业的本地网络\)，并为多个 VoIP 的应用程序，例如你公司的自定义行\-的\-业务应用程序。
  
基于策略的 QoS 是你提供网络控制-基于应用程序、 用户和计算机的网络带宽管理工具。 

当您使用 QoS 策略时，不需要你的应用程序要为特定的应用程序编程接口写入\(Api\)。 这使您能够与现有应用程序使用 QoS。 此外，基于策略的 QoS 利用的现有管理基础结构，因为基于策略的 QoS 构建在组策略。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>定义通过差异化的服务代码点 QoS 优先级\(DSCP\)
  
可以创建 QoS 策略用于定义与差分服务代码点的网络流量优先级\(DSCP\)值分配给不同类型的网络流量。 

DSCP 允许您将值应用\(0-63\)类型的服务内\(TOS\)字段在 IPv4 数据包的标头，并在 IPv6 中流量类字段中。 

DSCP 值提供了在 Internet 协议的网络流量分类\(IP\)级别，哪些路由器使用来确定流量排队行为。 

例如，可以配置路由器以将具有特定 DSCP 值的数据包放入三个队列之一： 高优先级，最大努力，或低于最大努力。 

关键网络流量，这是高优先级队列中，具有比其他通信首选项。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>每个应用程序使用调节率的限制网络带宽使用

您还可以通过指定中止值等级 QoS 策略中限制应用程序的出站网络流量。

用于定义的限制的 QoS 策略确定出站网络流量的速率。 例如，若要管理 WAN 成本，IT 部门可以实现服务级别协议，指定的文件服务器可以永远不会提供下载超过特定的速率。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 策略来应用 DSCP 值并遏制费率

此外可以使用 QoS 策略应用的 DSCP 值并限制到以下出站网络流量的速率：

- 发送应用程序和目录路径

- 源和目标 IPv4 或 IPv6 地址或地址前缀

- 协议-传输控制协议\(TCP\)和用户数据报协议\(UDP\)

- 源和目标端口和端口范围\(TCP 或 UDP\)

- 特定的用户或通过组策略中的部署的计算机组

通过使用这些控件，您可以指定 QoS 策略，DSCP 值为 46 VoIP 应用程序，启用路由器，在低延迟队列中，将 VoIP 数据包或你可以使用 QoS 策略来限制为 512 千字节 / 秒服务器的出站流量的一组<c1/1>kBps\)时从 TCP 端口 443 发送。

也可以将 QoS 策略应用到具有特殊的带宽需求的特定应用程序。 有关详细信息，请参阅[QoS 策略方案](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 策略的优点

使用 QoS 策略，可以配置和强制执行不能在路由器和交换机配置的 QoS 策略。 QoS 策略可提供以下优势。
  
1. **级别的详细信息：** 很难在路由器或交换机上创建用户级别的 QoS 策略，尤其是当用户的计算机可以通过使用动态 IP 地址分配配置或如果计算机未连接到固定的交换机或路由器端口，因为是经常使用这种情况便携式计算机。 与此相反，QoS 策略更容易对用户进行配置\-级别的域控制器上的 QoS 策略并传播到用户的计算机的策略。
2. **灵活性**。 无论其中或如何在计算机连接到网络，应用 QoS 策略-计算机可以从任何位置使用 WiFi 或以太网连接。 为用户\-级别 QoS 策略，在用户登录的任何位置的任何兼容设备应用策略的 QoS。
3. **安全性：** 如果你的 IT 部门通过使用 Internet 协议安全加密用户的流量从一端\(IPsec\)，你不能对分类基于数据包中 IP 层上的任何信息的路由器上的流量\(等TCP 端口\)。 但是，通过使用 QoS 策略，你可以对分类在最终设备，以指示 IP 标头中的数据包的优先级，IP 有效负载进行加密，并会发送数据包之前的数据包。
4. **性能：** 它们是更接近于源时，更好地执行某些 QoS 函数，如限制、。 QoS 策略将此类源最接近的 QoS 功能。
5. **可管理性：** QoS 策略可增强网络可管理性两种方式：

    。 因为它基于组策略，可以使用 QoS 策略来配置和管理一组用户/计算机 QoS 策略，只要有必要，和一个中央的域控制器计算机上。

    **b**。 QoS 策略可简化用户/计算机配置，它提供一种机制来指定策略的统一资源定位符\(URL\)而不是指定基于每个 QoS 策略，需要的服务器的 IP 地址的策略若要应用。 例如，假设您的网络具有共享一个公共 URL 的服务器的群集。 通过使用 QoS 策略，可以创建一个策略根据公共 URL，而不是每个策略基于每个服务器的 IP 地址在群集中创建的每个服务器的一个策略。

本指南的下一个主题，请参阅[开始使用 QoS 策略](qos-policy-get-started.md)。

