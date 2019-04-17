---
title: 服务 (QoS) 策略的质量
description: 本主题提供质量服务 (QoS) 策略，这允许你使用组策略来优先考虑网络的特定应用程序和 Windows Server 2016 服务的交通带宽的概述。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>服务 \(QoS\) 策略的质量

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用 QoS 策略用作中心点网络带宽管理你的整个 Active Directory 基础跨通过创建 QoS 配置文件，使用组策略分布其设置。

>[!NOTE]
>  本主题中，除了以下 QoS 策略文档才可用。  
>   
>  - [入门 QoS 策略](qos-policy-get-started.md)
>  - [管理 QoS 策略](qos-policy-manage.md)
>  - [QoS 策略的常见问题](qos-policy-faq.md)

组策略对象的一部分将 QoS 策略应用到用户登录会话或计算机 \(GPO\) 已链接到 Active Directory 容器，如域、网站或单位 \(OU\)。

QoS 交通管理发生下方应用程序层，这意味着你现有的应用程序不需要进行修改以受益于由 QoS 策略的优势。

## <a name="operating-systems-that-support-qos-policy"></a>支持 QoS 策略的操作系统

你可以使用 QoS 策略管理带宽计算机或使用以下 Microsoft 操作系统的用户。

- Windows Server 2016
- Windows 10
- Windows Server 2012R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>组策略中 QoS 策略的位置

在 Windows Server 2016 组策略管理编辑器，以下是 QoS 策略计算机配置为路径。

**默认域策略 |计算机配置 |策略 |Windows 设置 |按基于 QoS**

此路径下图所示。

![组策略中 QoS 策略的位置](../../media/QoS/QoS-Gp.jpg)

在 Windows Server 2016 组策略管理编辑器，以下是路径 QoS 策略用户配置。

**默认域策略 |用户配置 |策略 |Windows 设置 |按基于 QoS**

默认情况下没有 QoS 策略配置。

## <a name="why-use-qos-policy"></a>为什么使用 QoS 策略？
  
路况增加网络上时，越来越重要的是你的余额网络费用的服务的性能，但网络通信不通常简单的方法可供优化和管理。

在你的网络 mission\ 关键和 latency\ 敏感应用必须争夺网络带宽针对较低的优先级交通。 同时，一些用户和计算机的特定网络性能，可能需要要求区别服务级别。

提供成本效益且可预测的网络性能级别的难题首先通常显示通过宽的区域网络 \(WAN\) 连接或与延迟敏感应用程序，如语音 IP \(VoIP\) 和视频流式传输。 但是，将提供可预测网络服务级别最终目标适用于任何网络环境 \ (例如，企业的本地 network\)，以及比 VoIP 等应用程序，你的公司的自定义 line\ of\ 业务应用程序的详细信息。
  
基于策略的 QoS 是为你提供网络控制-基于应用、用户，以及计算机与该网络带宽管理工具。 

当你使用 QoS 策略时，你的应用程序不需要特定应用程序编程接口 \(APIs\) 对于写的。 这使你能够 QoS 使用现有的应用程序。 此外，基于策略的 QoS 充分利用你现有的管理基础结构，因为基于策略的 QoS 内置于组策略。

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>定义通过区分服务代码点 \(DSCP\) QoS 优先级
  
你可以创建 QoS 策略的定义区分服务代码点 \(DSCP\) 值分配给网络通信的不同类型的网络交通优先级。 

DSCP 允许你在应用内 IPv6 中的路况类字段以及内 IPv4 数据包标题中的服务类型 \(TOS\) 字段值 \(0–63\)。 

DSCP 值提供了在 Internet 的网络交通分类协议 \(IP\) 级别，哪些路由器使用决定交通排队行为。 

例如，将配置路由器将具有特定 DSCP 值数据包放入一个三个队列：高优先级，最大努力，或降低比最大努力。 

关键网络通信，这是高优先级队列中，已对其他通信的偏好随意选择。

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>每个应用程序限制速率与限制网络带宽使用量

你还可以通过 QoS 策略中指定调节率限制站网络通信的应用程序。

定义限制限制 QoS 策略确定站网络通信的语速。 例如，若要管理 WAN 成本，IT 部门可能实现指定文件服务器可以永远不会提供超过特定速率的下载服务级别协议。  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>使用 QoS 策略应用 DSCP 值，并限制率

你还可以使用 QoS 策略应用 DSCP 值，并限制站网络交通与以下费率：

- 发送应用程序和目录路径

- 源和目标 IPv4 或 IPv6 地址或前缀地址

- 协议-传输控件 \(TCP\) 和用户数据报协议 \(UDP\) 协议

- 源和目标端口和端口范围 \(TCP or UDP\)

- 特定组的用户或通过组策略中部署的计算机

通过使用这些控件，你可以指定 QoS 策略 DSCP 值为 46 VoIP 应用程序，启用路由器放置 VoIP 数据包在低延迟队列中，或者你可以使用 QoS 策略 TCP 端口 443 从发送时调节服务器的第二个 \(KBps\) 每 512 千字节站通信的一组。

你还可以将 QoS 策略应用到某个特定应用程序对带宽特殊的要求。 有关详细信息，请参阅[QoS 策略方案](qos-policy-scenarios.md)。
  
## <a name="advantages-of-qos-policy"></a>QoS 策略的优势

QoS 策略中，您可以配置，并执行无法路由器和开关配置的 QoS 策略。 QoS 策略提供以下的优势。
  
1. **级别的详细信息：**很困难，尤其是在用户的计算机是通过动态 IP 配置地址分配或如果计算机未连接到解决的交换机或路由器端口，因为是经常使用笔记本电脑的情况创建路由器或开关，在用户级别 QoS 策略。 相反，QoS 策略使更轻松地域控制器上配置 user\ 级别 QoS 策略和传播到用户的计算机策略。
2. **大的灵活性**。 无论为位置或方式一台计算机连接到该网络时，应用 QoS 策略，-计算机可以从任意位置使用 WiFi 或以太网进行连接。 对于 user\ 级别 QoS 策略，在任何兼容设备在用户登录的任何位置应用 QoS 策略。
3. **安全：** IT 部门用户的通信端到端使用加密 Internet 协议安全 \(IPsec\)，如果你无法进行分类路由器基于上方 IP 层数据包中的任何信息的交通 \ (例如，TCP port\)。 但是，通过使用 QoS 策略，你可以分类数据包在结束设备 IP 负载已加密并数据包会发送之前表示的数据包 IP 标题中的优先级。
4. **性能：**时更接近于源某些 QoS 功能，如调节，更好地执行。 QoS 策略移动靠近源此类 QoS 功能。
5. **可管理性：** QoS 策略增强了网络可管理性两种方法：

    **a**. 它所采用的组策略，因为你可以使用 QoS 策略配置和管理一组的用户中的计算机 QoS 策略，如有必要，并中央域控制器一台计算机上。

    **b**. QoS 策略方便用户中的计算机配置了通过提供机制指定通过统一资源定位器 \(URL\) 而不是指定的 IP 地址的每个 QoS 策略需要应用的服务器基于策略的策略。 例如，假设你的网络已的群集共享常见 URL 的服务器。 通过使用 QoS 策略，可以创建一个基于常见的 URL，而不是创建一个策略为每个服务器在群集，基于每个服务器的 IP 地址的每个策略的策略。

本指南中的下一步主题，请参阅[入门 QoS 策略](qos-policy-get-started.md)。

