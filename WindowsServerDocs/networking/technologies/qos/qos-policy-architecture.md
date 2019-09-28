---
title: QoS 策略体系结构
description: 本主题概述了服务质量（QoS）策略，该策略允许你使用组策略来确定 Windows Server 2016 中特定应用程序和服务的网络流量带宽的优先级。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 609d3f28465380b7d15648cfeb73070a39b9362f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395984"
---
# <a name="qos-policy-architecture"></a>QoS 策略体系结构

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解 QoS 策略的体系结构。

下图显示了基于策略的 QoS 的体系结构。

![QoS 策略的体系结构](../../media/QoS/QoS-Policy-Architecture.jpg)

基于策略的 QoS 的体系结构包括以下组件：

- **组策略客户端服务**。 一种 Windows 服务，用于管理用户和计算机配置组策略设置。

- **组策略引擎**。 组策略客户端服务的一个组件，用于在启动时从 Active Directory 检索用户和计算机配置组策略设置，并定期检查更改 @no__t 默认情况下，每90分钟 @ no__t。 如果检测到更改，组策略引擎将检索新的组策略设置。 当更新 QoS 策略时，组策略引擎处理传入的 Gpo 并通知 QoS 客户端扩展。

- **QoS 客户端扩展**。 组策略客户端服务的一个组件，它等待来自组策略引擎的指示 QoS 策略已更改并通知 QoS 检测模块。

- **Tcp/ip 堆栈**。 TCP/IP 堆栈，其中包括对 IPv4 和 IPv6 的集成支持，并支持 Windows 筛选平台。 

- **QoS 检查**。 模块一个 TCP/IP 堆栈内的组件，该组件等待 qos 客户端扩展的 QoS 策略更改的指示，检索 QoS 策略设置，并与传输层和 Pacer 交互，以内部标记与 QoS 匹配的流量政策.

- **NDIS**1.x。 内核模式网络驱动程序与 Windows Server 和客户端操作系统中的操作系统之间的标准接口。 NDIS 1.x 支持轻型筛选器，这是一个简化的用于 NDIS 中间驱动程序和微型端口驱动程序的驱动程序模型，可提供更好的性能。

- **QoS 网络提供程序接口 \(NPI @ no__t-2**。 用于与 Pacer 交互的内核模式驱动程序的接口。

- **Pacer. .sys**。 一种 NDIS 1.x 轻型筛选器驱动程序，用于控制基于策略的 QoS 的数据包计划，以及使用一般 QoS \(GQoS @ no__t 和流量控制的应用程序的流量，\(TC @ no__t。 Pacer. .sys 在 Windows Server 2003 和 Windows XP 中替换 Psched。 Pacer 与 QoS 数据包计划程序组件安装在网络连接或适配器的属性中。

有关本指南的下一个主题，请参阅[QoS 策略方案](qos-policy-scenarios.md)。

有关本指南的第一个主题，请参阅[服务质量（QoS）策略](qos-policy-top.md)。

