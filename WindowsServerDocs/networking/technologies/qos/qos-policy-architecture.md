---
title: QoS 策略建筑
description: 本主题提供质量服务 (QoS) 策略，这允许你使用组策略来优先考虑网络的特定应用程序和 Windows Server 2016 服务的交通带宽的概述。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>QoS 策略建筑

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解 QoS 策略的体系结构。

下图显示策略基于 QoS 体系的结构。

![体系结构 QoS 策略](../../media/QoS/QoS-Policy-Architecture.jpg)

基于策略的 QoS 的体系结构包含以下组件：

- **组策略客户端服务**。 管理用户和计算机配置组策略设置 Windows 服务。

- **组策略引擎**。 在启动时和定期从 Active Directory 检索用户和计算机配置组策略设置在组策略客户服务的组件检查的更改 \（默认情况下，每个 90 minutes\）。 检测到更改时，在组策略引擎检索新组策略设置。 组策略引擎处理传入 Gpo，并 QoS 策略更新时通知 QoS 客户端扩展。

- **QoS 客户端扩展**。 等待从组策略引擎已更改 QoS 策略指示并将其通知 QoS 检查模块在组策略客户服务组件。

- **TCP/IP 堆栈**。 包括对 IPv4 和 IPv6 集成的支持，并支持 Windows 筛选平台 TCP/IP 堆栈。 

- **QoS 检查**。 等待 QoS 客户端扩展，从 QoS 策略更改的迹象 TCP/IP 堆栈中的模块组件检索 QoS 策略设置，并传输层以及 Pacer.sys 内部标记匹配 QoS 策略流量与之交互。

- **NDIS 6.x**。 标准接口之间内核模式的网络驱动程序和 Windows Server 和客户端操作系统中的操作系统。 NDIS 6.x 支持简洁筛选器，它是一个简化驱动程序模型 NDIS 中间的驱动程序和微型端口驱动程序提供更好的性能。

- **QoS 网络提供商接口 \(NPI\)**。 对于内核模式驱动程序与 Pacer.sys 进行交互的界面。

- **Pacer.sys**。 NDIS 6.x 简洁筛选器驱动程序控件数据包计划基于策略的 QoS 和应用程序使用通用 QoS \(GQoS\) 和交通控制 \(TC\) Api 的通信。 Pacer.sys 替换 Psched.sys Windows Server 2003 和 Windows XP 中。 与网络连接或适配器属性从 QoS 数据包计划组件安装 Pacer.sys。

本指南中的下一步主题，请参阅[QoS 策略方案](qos-policy-scenarios.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。

