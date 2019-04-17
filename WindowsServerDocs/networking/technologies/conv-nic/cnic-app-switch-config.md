---
title: 已合并好 nic 物理开关配置
description: 本主题介绍有关 Windows Server 2016 汇聚 NIC 配置指南。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>已合并好 nic 物理开关配置

>适用于：Windows Server（半年通道），Windows Server 2016

你可以来配置你的物理交换机作为指南中使用以下各部分。

以下是仅命令和他们使用;必须确定端口 Nic 连接到您的环境中。 

>[!IMPORTANT]
>确保 VLAN 和无放策略设置的优先级哪些 SMB 配置。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista 切换 \ (dcs\ 7050s\-64 EOS\-4.13.7M\)

1.  短 \ 转到管理员模式 (通常所要求的 password\）
2.  配置 \（以输入配置 mode\）
3.  显示运行 \（显示当前正在运行 configuration\）
4.  查明你 Nic 连接到切换端口。 在以下示例中，它们是 14/1,15 月 1,16/1,17 月 1。
5.  Int 以太网 14/1,15 月 1,16/1,17 月 1 \（进入这些 ports\ 配置模式）
6.  Dcbx 模式 ieee
7.  上的优先级流控制模式
8.  交换机端口主干本机 vlan 225
9.  允许 vlan 100-225 交换机端口主干
10. 交换机端口模式主干
11. 优先级流控制优先级 3 不放
12. Qos 信任 cos
13. 显示运行 \（验证该配置正确设置 ports\ 上）
14. Wr \（若要使这些设置仍然存在切换 reboot\ 跨）

### <a name="tips"></a>使用技巧：
1.  没有 #command # 否定命令
2.  如何添加新 VLAN: int vlan 100 \（如果存储网络位于 VLAN 100\）
3.  如何检查现有 Vlan：显示 vlan
4.  配置 Arista 切换的详细信息，联机搜索：Arista 电源过压手册。
5.  使用此命令以确认 PFC 设置：显示计数器优先级流量控制的详细信息

## <a name="dell-switch-s4810-ftos-99-00"></a>戴尔切换 \ (S4810，FTOS 9.9 \(0.0\)\)

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco 切换 \ (核心 3132、版本 6.0\(2\)U6\(1\)\)

### <a name="global"></a>全球
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>特定端口

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    

## <a name="all-topics-in-this-guide"></a>本指南中的所有主题

本指南中包括以下主题。

- [已合并好的 NIC 配置了一个网络适配器](cnic-single.md)
- [已合并好的 NIC 成的 NIC 配置](cnic-datacenter.md)
- [已合并好 nic 物理开关配置](cnic-app-switch-config.md)
- [故障排除汇聚 NIC 配置](cnic-app-troubleshoot.md)
