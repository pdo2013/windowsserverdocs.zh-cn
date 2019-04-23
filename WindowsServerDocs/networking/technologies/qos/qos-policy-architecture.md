---
title: QoS 策略体系结构
description: 本主题概述了服务质量 (QoS) 策略，它允许您使用组策略为优先处理的特定应用程序和 Windows Server 2016 中的服务的网络流量带宽。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843138"
---
# <a name="qos-policy-architecture"></a>QoS 策略体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题以了解 QoS 策略的体系结构。

下图显示了基于策略的 QoS 体系结构。

![QoS 策略的体系结构](../../media/QoS/QoS-Policy-Architecture.jpg)

基于策略的 QoS 体系结构包括以下组件：

- **组策略客户端服务**。 用于管理用户和计算机配置组策略设置的 Windows 服务。

- **组策略引擎**。 在启动时和定期从 Active Directory 检索用户和计算机配置组策略设置组策略客户端服务的一个组件会检查更改\(默认情况下，每 90 分钟\)。 如果检测到更改，组策略引擎检索新的组策略设置。 组策略引擎处理传入的 Gpo，并将 QoS 策略更新时通知 QoS 客户端扩展。

- **QoS 客户端扩展**。 等待从 QoS 策略已更改的组策略引擎的指示，并通知 QoS 检查模块的组策略客户端服务的组件。

- **TCP/IP 堆栈**。 TCP/IP 堆栈，包括对 IPv4 和 IPv6 的集成的支持，并支持 Windows 筛选平台。 

- **QoS 检查**。 等待指示从 QoS 客户端侧扩展而言，QoS 策略更改的 TCP/IP 堆栈内的模块组件检索 QoS 策略设置，并使用传输层和 Pacer.sys 在内部将标记与 QoS 相匹配的流量进行交互策略。

- **NDIS 6.x**。 内核模式网络驱动程序和 Windows Server 和客户端操作系统中的操作系统之间的标准接口。 NDIS 6.x 支持轻型筛选器，这是简化的驱动程序模型的中间的 NDIS 驱动程序和微型端口驱动程序提供更好的性能。

- **QoS 网络提供程序接口\(NPI\)**。 内核模式驱动程序与 Pacer.sys 进行交互的接口。

- **Pacer.sys**。 控制数据包调度基于策略的 QoS 和使用泛型 QoS 的应用程序通信的 NDIS 6.x 轻型筛选器驱动程序\(GQoS\)和流量控制\(TC\) Api。 Pacer.sys 取代了 Windows Server 2003 和 Windows XP 中的 Psched.sys。 Pacer.sys 随通过网络连接或适配器的属性中的 QoS 数据包计划程序组件。

本指南的下一个主题，请参阅[QoS 策略方案](qos-policy-scenarios.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。

