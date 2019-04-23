---
title: QoS 策略的工作原理
description: 本主题概述了服务质量 (QoS) 策略，它允许您使用组策略为优先处理的特定应用程序和 Windows Server 2016 中的服务的网络流量带宽。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864278"
---
# <a name="how-qos-policy-works"></a>QoS 策略的工作原理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

当启动或获取已更新的用户或计算机配置组策略设置 QoS 时，将发生以下过程。

1. 组策略引擎从 Active Directory 检索用户或计算机配置组策略设置。

2. 组策略引擎会 QoS 客户端扩展通知中包括了 QoS 策略的更改。

3. QoS 客户端扩展将 QoS 策略事件通知发送到 QoS 检查模块。

4. QoS 检查模块检索的用户或计算机的 QoS 策略，并将其存储。

当新的传输层终结点\(TCP 连接或 UDP 流量\)创建后，将发生以下过程。

1. TCP/IP 堆栈的传输层组件通知 QoS 检查模块。

2. QoS 检查模块进行比较的参数的存储 QoS 策略的传输层终结点。

3. 如果找到匹配项，则 QoS 检查模块联系 Pacer.sys 创建流，其中包含的 DSCP 值和限制设置匹配的 QoS 策略的流量的数据结构。 如果有多个 QoS 策略的传输层终结点的参数相匹配，则使用最具体的 QoS 策略。

4. Pacer.sys 存储流，并返回对应于 QoS 检查模块到流的流数字。

5. QoS 检查模块返回到传输层的流数。

6. 传输层存储具有传输层终结点的流数量。

对应于传输层终结点的数据包流数字开头的标记时发送，将发生以下过程。

1. 传输层在内部将标记包含流数目的数据包。

2. 网络层中查询 Pacer.sys DSCP 值对应的数据包流数。

3. Pacer.sys 返回网络层的 DSCP 值。

4. 网络层 IPv4 TOS 字段或 IPv6 通信类字段更改为指定 Pacer.sys 的 DSCP 值，并为 IPv4 数据包计算最终的 IPv4 标头校验和。

5. 网络层将数据包传递给分帧层。

6. 分帧层数据包已被标记为流数，因为将数据包传递给通过 NDIS Pacer.sys 6.x。

7. Pacer.sys 使用的数据包流数来确定数据包需要限制，以及如果是这样，计划用于发送数据包。

8. Pacer.sys 将数据包可以立即\(如果没有流量限制\)或按计划\(流量是否限制\)到 NDIS 6.x 通过相应的网络适配器的传输。

基于策略的 QoS 这些过程提供以下优点。

- 要确定是否应用了 QoS 策略的流量的检查是完成的每个传输层终结点，而不是每个数据包。

- 与 QoS 策略匹配的流量没有性能影响。

- 应用程序不需要进行修改，以充分利用基于 DSCP 的不同服务或流量限制。

- QoS 策略可以应用于受 IPsec 保护的流量。

本指南的下一个主题，请参阅[QoS 策略体系结构](qos-policy-architecture.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。
