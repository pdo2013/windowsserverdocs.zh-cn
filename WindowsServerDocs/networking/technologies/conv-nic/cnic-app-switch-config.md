---
title: 聚合的 NIC 的物理交换机配置
description: 在本主题中，我们向你提供指导原则来配置物理交换机。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829398"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>聚合的 NIC 的物理交换机配置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们向你提供指导原则来配置物理交换机。 


这些是仅命令和它们的用途;在您的环境中，必须确定为 Nic 连接的端口。 

>[!IMPORTANT]
>确保为优先级对其配置 SMB 设置的 VLAN 和非放置策略。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista 开关\(dc\-7050s年\-64，EOS\-4.13.7M\)

1.  en\(转到管理员模式下，通常会要求输入密码\)
2.  配置\(进入配置模式\)
3.  显示运行\(显示当前正在运行的配置\)
4.  了解到将 Nic 连接到的交换机端口。 在这些示例中，它们是 14/1,15/1,16/1,17/1。
5.  int eth 14/1,15/1,16/1,17/1\(进入配置模式为这些端口\)
6.  dcbx 模式 ieee
7.  上的优先级流控制模式
8.  交换机端口 trunk 本机 vlan 225
9.  允许使用交换机端口 trunk vlan 100 225
10. 交换机端口模式 trunk
11. 优先级流控制优先级为 3 非放置
12. qos 信任 cos
13. 显示运行\(验证该配置不正确的端口上的安装程序\)
14. wr\(以使设置仍然存在跨交换机重新启动\)

### <a name="tips"></a>提示：
1.  没有 #command # 求反命令
2.  如何添加新的 VLAN: int vlan 100\(存储网络是否在 VLAN 100\)
3.  如何检查现有 Vlan： 显示 vlan
4.  有关配置 Arista 开关，联机搜索的详细信息：Arista EOS 手动
5.  使用此命令验证 PFC 设置： 显示优先级流控制计数器详细信息

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell 开关\(S4810、 FTOS 9.9 \(0.0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco 交换机\(Nexus 3132，版本 6.0\(2\)U6\(1\)\)

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
    

### <a name="port-specific"></a>特定的端口

    
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

- [聚合的 NIC 配置具有单个网络适配器](cnic-single.md)
- [聚合的 NIC 成组的 NIC 配置](cnic-datacenter.md)
- [故障排除聚合 NIC 配置](cnic-app-troubleshoot.md)

--- 