---
title: QoS 策略的工作原理
description: 本主题提供质量服务 (QoS) 策略，这允许你使用组策略来优先考虑网络的特定应用程序和 Windows Server 2016 服务的交通带宽的概述。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>QoS 策略的工作原理

>适用于：Windows Server（半年通道），Windows Server 2016

当启动或获得更新后的用户或计算机配置 QoS 组策略设置时，将发生以下过程。

1. 组策略引擎检索从 Active Directory 的用户或计算机配置组策略设置。

2. 组策略引擎通知 QoS 客户端扩展过更改 QoS 策略中。

3. QoS 客户端扩展发送到 QoS 检查模块 QoS 策略事件通知。

4. QoS 检查模块检索的用户或计算机 QoS 策略，并将它们存储。

当新的传输层端点 \（TCP 连接 UDP traffic\）创建或时，会发生以下过程。

1. TCP/IP 堆栈传输层组件通知 QoS 检查模块。

2. QoS 检查模块比较存储 QoS 策略传输层端点参数。

3. 如果找到匹配项，QoS 检查模块联系人 Pacer.sys 创建流程，其中包含 DSCP 值和匹配 QoS 策略设置的限制的流量数据结构。 如果有多个与的参数传输层端点匹配的 QoS 策略，请使用最特定 QoS 策略。

4. Pacer.sys 存储流，并返回到 QoS 检查模块流对应的流量数字。

5. QoS 检查模块返回到传输层流数。

6. 传输层存储流传输层端点数。

当使用流量号码标记为数据包对应于传输层端点已发送，执行以下过程。

1. 传输层内部标记流量号码数据包。

2. 网络层查询 Pacer.sys 对应的数据包流量数 DSCP 值。

3. Pacer.sys 返回到网络层 DSCP 值。

4. 网络层变为 DSCP 值 Pacer.sys 由指定 IPv4 TOS 字段或 IPv6 交通类字段，并为 IPv4 数据包计算最终 IPv4 标题校验。

5. 网络层交给帧的一层的数据包。

6. 已使用流量号码标记包，因为帧的一层就数据包转发到 Pacer.sys 通过 NDIS 6.x。

7. Pacer.sys 流数数据包用于确定如果数据包需要被阻止，然后如果是这样，安排数据包以发送。

8. Pacer.sys 手数据包任一立即 \（如果没有交通 throttling\）或按计划 \（如果交通 throttling\）到 NDIS 6.x 传输的相应的网络适配器。

这些进程的策略基于 QoS 提供以下优势。

- 每个传输层端点，而不是每数据包完成交通以确定是否 QoS 策略应用检查。

- 目前不会影响性能不匹配 QoS 策略的交通。

- 应用程序不需要进行修改以充分利用 DSCP 基于与众不同服务或交通的方法，限制。

- QoS 策略可以将应用到 IPsec 受保护的交通。

本指南中的下一步主题，请参阅[QoS 策略体系结构](qos-policy-architecture.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。
