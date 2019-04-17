---
title: 故障排除汇聚 NIC 配置
description: 本主题介绍有关 Windows Server 2016 汇聚 NIC 配置指南。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>故障排除汇聚 NIC 配置

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用下面的脚本验证是否 RDMA 配置无误 Hyper-V 主机上。

- [下载脚本 Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

你还可以使用下面的 Windows PowerShell 命令疑难解答，验证你的已合并好 Nic 配置。

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

验证你的网络适配器 RDMA 配置，运行以下 Windows PowerShell 命令，Hyper-V 服务器上。

    
    Get-NetAdapterRdma | fl *
    

你可以使用以下预期和意外的结果识别和解决问题之后你 Hyper-V 主机上运行此命令。

### <a name="get-netadapterrdma-expected-results"></a>Get-NetAdapterRdma 预期结果

主机 vNIC 和物理 NIC 显示非零值 RDMA 功能。

![Windows PowerShell 预期结果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma 意外的结果

如果你在运行时，你会收到意外的结果，请执行以下步骤**获取 NetAdapterRdma**命令。

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序处于最新。 对于 Mellanox，使用至少垂直 42。 
2. 验证 Mlnx 微型端口和总线驱动程序匹配通过检查驱动程序版本通过设备管理器。 在设备系统找不到总线驱动程序。 该名称应该开头 Mellanox Connect-X 3 PRO VPI，使用的网络适配器属性下面的屏幕截图中所示。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 请确保网络直接 (RDMA) 上的物理 NIC 和主机 vNIC 已启用。
5. 请确保通过检查其 RDMA 功能通过直接物理适配器创建 vSwitch。
6. 检查 EventViewer 系统日志中和按源"超 V VmSwitch"筛选器。

## <a name="get-smbclientnetworkinterface"></a>获取 SmbClientNetworkInterface

作为额外步骤验证 RDMA 配置，Hyper-V 服务器上中运行以下 Windows PowerShell 命令。


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>获取 SmbClientNetworkInterface 预期结果

主机 vNIC 应该会显示为 RDMA 支持从 SMB 的角度还。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>获取 SmbClientNetworkInterface 意外的结果

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序处于最新。 对于 Mellanox，使用至少垂直 42。 
2. 验证 Mlnx 微型端口和总线驱动程序匹配通过检查驱动程序版本通过设备管理器。 在设备系统找不到总线驱动程序。 该名称应该开头 Mellanox Connect-X 3 PRO VPI，使用的网络适配器属性下面的屏幕截图中所示。
3. 请确保网络直接 (RDMA) 上的物理 NIC 和主机 vNIC 已启用。
4. 请确保 Hyper-V 虚拟交换机用来通过直接物理适配器创建检查其 RDMA 功能。
5. "SMB 客户端"检查 EventViewer 日志**应用程序和服务 |Microsoft |Windows**。

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

你可以通过运行以下 Windows PowerShell 命令查看服务 \(QoS\) 配置的网络适配器质量。

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-NetAdapterQos 预期结果

根据你可以使用本指南执行第一个配置步骤应显示优先级并类交通。

![服务优先级并类质量](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos 意外的结果

如果你结果意外，请执行以下步骤。

1. 确保物理网络适配器支持数据中心桥 \(DCB\) 和 QoS
2. 确保网络适配器驱动程序处于最新状态。


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

可以使用下面的 Windows PowerShell 命令以验证的远程节点的 IP 地址 RDMA\ 支持。

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Get-SmbMultiChannelConnection 预期结果

远程节点 IP 地址显示为 RDMA 支持。

![RDMA 支持远程节点 IP 地址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection 意外的结果

如果你结果意外，请执行以下步骤。

1. 请确保 ping 两种方式工作。
2. 请确保防火墙不会阻止 SMB 连接启动。 特别是，能够 SMB 直接端口 iWARP 的 5445 的防火墙规则和 ROCE 的 445。

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

你可以使用以下命令来验证你启用 RDMA 虚拟 NIC 由 SMB 报告为 RDMA\ 支持。

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface 预期结果

对 RDMA 启用的虚拟 NIC 必须被 SMB 看到作为 RDMA 支持。

![SMB 报告 Nic 是 RDMA 支持](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface 意外的结果

如果你结果意外，请执行以下步骤。

1. 请确保 ping 两种方式工作。
2. 请确保防火墙不会阻止 SMB 连接启动。

## <a name="vstat-mellanox-specific"></a>Vstat \(Mellanox specific\)

如果你使用的 Mellanox 网络适配器，你可以使用**vstat**命令以确认 RDMA 通过集中以太网 \(RoCE\) Hyper-V 节点上的新版本。

### <a name="vstat-expected-results"></a>Vstat 预期结果

在这两个节点 RoCE 版本必须相同。 这是验证两个节点上的固件版本是最新版本的好方法。

![RoCE 版本检查结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>Vstat 意外的结果

如果你结果意外，请执行以下步骤。

1. 使用 Set-MlnxDriverCoreSetting 设置正确 RoCE 版本
2. 从 Mellanox 网站安装最新的固件。


## <a name="perfmon-counters"></a>Perfmon 计数器

你可以查看计数器在 Performance Monitor 验证您的配置 RDMA 活动。

![性能监视器结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>本指南中的所有主题

本指南中包括以下主题。

- [已合并好的 NIC 配置了一个网络适配器](cnic-single.md)
- [已合并好的 NIC 成的 NIC 配置](cnic-datacenter.md)
- [已合并好 nic 物理开关配置](cnic-app-switch-config.md)
- [故障排除汇聚 NIC 配置](cnic-app-troubleshoot.md)
