---
title: 汇聚 NIC 的物理交换机配置
description: 在本主题中，我们将向你提供配置物理交换机的准则。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: d10e8ca6e4689b89a8b9532f77613f17280282b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355482"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>汇聚 NIC 的物理交换机配置

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将向你提供配置物理交换机的准则。 


这些只是命令及其使用情况;你必须确定要在环境中将 Nic 连接到的端口。 

>[!IMPORTANT]
>确保为配置了 SMB 的优先级设置 VLAN 和非丢弃策略。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista 开关 \(dcs @ no__t-17050s @ no__t-264，EOS @ no__t-34.13.7 M @ no__t-4

1.  en \(go 到管理模式，通常会询问密码 @ no__t-1
2.  config \(to 进入配置模式 @ no__t-1
3.  显示运行 \(shows 当前正在运行的配置 @ no__t-1
4.  查找 Nic 连接到的交换机端口。 在这些示例中，它们是 14/1、15/1、16/1、17/1。
5.  int eth 14/1，15/1，16/1，17/1 @no__t 0enter 为这些端口 @ no__t-1 的配置模式
6.  dcbx 模式 ieee
7.  优先级-流动控制模式
8.  交换机间干线本机 vlan 225
9.  交换机间干线允许 vlan 100-225
10. 交换机间模式干线
11. 优先级-流控制优先级3不删除
12. qos 信任 cos
13. 显示运行 \(verify 在端口 @ no__t 上正确设置了配置
14. wr \(to 使设置在交换机重新启动时保持不变 @ no__t-1

### <a name="tips"></a>技巧
1.  No #command # 对命令求反
2.  如何添加新 VLAN： int VLAN 100 @no__t 0If 存储网络在 VLAN 100 @ no__t 上
3.  如何检查现有的 Vlan：显示 vlan
4.  有关配置 Arista 开关的详细信息，请联机搜索以下内容：Arista EOS 手动
5.  使用此命令验证 PFC 设置：显示优先级-流控制计数器详细信息

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell 交换机 \(S4810、FTOS 9.9 \(0.0 @ no__t-2 @ no__t-3

    
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
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco 交换机 \(Nexus 3132，版本 6.0 @ no__t-12 @ no__t-2U6 @ no__t-31 @ no__t-4 @ no__t-5

### <a name="global"></a>全局
    
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
    
--- 

## <a name="related-topics"></a>相关主题

- [使用单个网络适配器汇聚 NIC 配置](cnic-single.md)
- [汇聚 NIC 分组 NIC 配置](cnic-datacenter.md)
- [聚合 NIC 配置疑难解答](cnic-app-troubleshoot.md)

--- 