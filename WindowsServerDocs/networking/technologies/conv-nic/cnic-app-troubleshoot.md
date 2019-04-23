---
title: 故障排除聚合 NIC 配置
description: 本主题是聚合 NIC 配置指南 Windows Server 2016 的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876478"
---
# <a name="troubleshooting-converged-nic-configurations"></a>故障排除聚合 NIC 配置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用以下脚本以验证是否正确的 HYPER-V 主机上的 RDMA 配置。

- [下载脚本测试 Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

此外可以使用以下 Windows PowerShell 命令来进行故障排除和验证将聚合的 Nic 的配置。

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

若要验证你的网络适配器 RDMA 配置，请在 HYPER-V 服务器上运行以下 Windows PowerShell 命令。

    
    Get-NetAdapterRdma | fl *
    

可以使用以下预期和意外的结果以识别并解决问题后的 HYPER-V 主机上运行此命令。

### <a name="get-netadapterrdma-expected-results"></a>Get NetAdapterRdma 预期结果

主机 vNIC 和物理 NIC 显示非零值 RDMA 功能。

![Windows PowerShell 预期结果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma unexpected results

如果在运行时收到意外的结果，请执行以下步骤**Get NetAdapterRdma**命令。

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序是最新。 对于 Mellanox，使用至少垂直 42。 
2. 验证 Mlnx 微型端口和总线驱动程序匹配通过检查驱动程序版本通过设备管理器。 在系统设备中找不到总线驱动程序。 名称应以 Mellanox Connect X 3 PRO VPI 开头，如以下屏幕截图的网络适配器属性中所示。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 请确保同时在物理 NIC 和主机 vNIC 上启用网络直接 (RDMA)。
5. 请确保通过检查其 RDMA 功能直接物理适配器上创建 vSwitch。
6. 检查事件查看器系统日志并按源"Hyper V VmSwitch"进行筛选。

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

若要验证你的 RDMA 配置执行额外步骤，在 HYPER-V 服务器上运行以下 Windows PowerShell 命令。


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Get SmbClientNetworkInterface 预期结果

从 SMB 的角度看，主机 vNIC 应显示为支持 RDMA。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get SmbClientNetworkInterface 意外的结果

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序是最新。 对于 Mellanox，使用至少垂直 42。 
2. 验证 Mlnx 微型端口和总线驱动程序匹配通过检查驱动程序版本通过设备管理器。 在系统设备中找不到总线驱动程序。 名称应以 Mellanox Connect X 3 PRO VPI 开头，如以下屏幕截图的网络适配器属性中所示。
3. 请确保同时在物理 NIC 和主机 vNIC 上启用网络直接 (RDMA)。
4. 请确保通过检查其 RDMA 功能直接物理适配器上创建的 HYPER-V 虚拟交换机。
5. 事件查看器日志中检查"SMB 客户端"**应用程序和服务 |Microsoft |Windows**。

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

您可以查看网络适配器服务质量\(QoS\)通过运行以下 Windows PowerShell 命令的配置。

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-netadapterqos 预期结果

应使用本指南执行第一个配置步骤根据显示优先级和通信类。

![服务优先级和类的质量](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-netadapterqos 意外的结果

如果您的结果意外，请执行以下步骤。

1. 确保物理网络适配器支持数据中心桥接\(DCB\)和 QoS
2. 请确保网络适配器驱动程序是最新。

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

可以使用以下 Windows PowerShell 命令来验证远程节点的 IP 地址是 RDMA\-支持。

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Get SmbMultiChannelConnection 预期结果

远程节点的 IP 地址显示为 RDMA 支持。

![RDMA 支持远程节点 IP 地址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get SmbMultiChannelConnection 意外的结果

如果您的结果意外，请执行以下步骤。

1. 请确保 ping 工作这两种方式。
2. 请确保防火墙未阻止 SMB 连接启动。 具体而言，启用防火墙规则 SMB 直通端口 5445 iwarp 和 roce 445。

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

可以使用以下命令来验证是否虚拟 NIC 启用 RDMA 报告为 RDMA\-SMB 的支持。

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Get SmbClientNetworkInterface 预期结果

已启用 RDMA 的虚拟 NIC 必须视为任务的支持 RDMA 的 SMB。

![SMB 报表 Nic 是支持 RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get SmbClientNetworkInterface 意外的结果

如果您的结果意外，请执行以下步骤。

1. 请确保 ping 工作这两种方式。
2. 请确保防火墙未阻止 SMB 连接启动。

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox 特定于\)

如果使用网络的 Mellanox 适配器，则可以使用**vstat**命令来验证 RDMA over Converged Ethernet \(RoCE\) HYPER-V 节点上的版本。

### <a name="vstat-expected-results"></a>vstat 预期结果

两个节点上的 RoCE 版本必须相同。 这也是验证两个节点上的固件版本是最新的好方法。

![RoCE 版本检查结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat 意外的结果

如果您的结果意外，请执行以下步骤。

1. 设置使用集 MlnxDriverCoreSetting 正确 RoCE 版本
2. 从 Mellanox 网站安装最新的固件。

--- 

## <a name="perfmon-counters"></a>Perfmon 计数器

你可以查看性能监视器以验证你的配置的 RDMA 活动中的计数器。

![性能监视器结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>相关主题

- [聚合的 NIC 配置具有单个网络适配器](cnic-single.md)
- [聚合的 NIC 成组的 NIC 配置](cnic-datacenter.md)
- [聚合的 NIC 的物理交换机配置](cnic-app-switch-config.md)

---
