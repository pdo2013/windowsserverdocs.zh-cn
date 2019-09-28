---
title: 聚合 NIC 配置疑难解答
description: 本主题是适用于 Windows Server 2016 的汇聚 NIC 配置指南的一部分。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 297044397088bfb64b51e1553d3f69d5b933e81b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405909"
---
# <a name="troubleshooting-converged-nic-configurations"></a>聚合 NIC 配置疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用以下脚本来验证 Hyper-v 主机上的 RDMA 配置是否正确。

- [下载脚本 Test-Rdma](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

你还可以使用以下 Windows PowerShell 命令来排查和验证你的汇聚 Nic 的配置。

## <a name="get-netadapterrdma"></a>NetAdapterRdma

若要验证网络适配器 RDMA 配置，请在 Hyper-v 服务器上运行以下 Windows PowerShell 命令。

    
    Get-NetAdapterRdma | fl *
    

在 Hyper-v 主机上运行此命令后，可以使用以下预期和意外结果来识别和解决问题。

### <a name="get-netadapterrdma-expected-results"></a>NetAdapterRdma 预期结果

主机 vNIC 和物理 NIC 显示非零 RDMA 功能。

![Windows PowerShell 预期结果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>NetAdapterRdma 意外结果

如果在运行**NetAdapterRdma**命令时收到意外的结果，请执行以下步骤。

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序是最新的。 对于 Mellanox，请使用至少 drop 42。 
2. 通过检查驱动程序版本设备管理器，验证 Mlnx 微型端口和总线驱动程序是否匹配。 可以在系统设备中找到总线驱动程序。 该名称应以 Mellanox Connect-X 3 PRO VPI 开头，如以下网络适配器属性屏幕截图中所示。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 确保物理 NIC 和主机 vNIC 上均已启用 "网络直接（RDMA）"。
5. 请确保在正确的物理适配器上创建 vSwitch，方法是检查其 RDMA 功能。
6. 检查 EventViewer 系统日志，并按源 "Hyper-v-VmSwitch" 进行筛选。

--- 

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

作为验证 RDMA 配置的其他步骤，请在 Hyper-v 服务器上运行以下 Windows PowerShell 命令。


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>SmbClientNetworkInterface 预期结果

宿主 vNIC 还应显示为从 SMB 的角度来看的 RDMA。

![网络适配器属性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>SmbClientNetworkInterface 意外结果

1. 请确保 Mlnx 微型端口和 Mlnx 总线驱动程序是最新的。 对于 Mellanox，请使用至少 drop 42。 
2. 通过检查驱动程序版本设备管理器，验证 Mlnx 微型端口和总线驱动程序是否匹配。 可以在系统设备中找到总线驱动程序。 该名称应以 Mellanox Connect-X 3 PRO VPI 开头，如以下网络适配器属性屏幕截图中所示。
3. 确保物理 NIC 和主机 vNIC 上均已启用 "网络直接（RDMA）"。
4. 请确保通过检查 Hyper-v 虚拟交换机的 RDMA 功能，在正确的物理适配器上创建 Hyper-v 虚拟交换机。
5. 在应用程序和服务中检查 "SMB 客户端" 的 EventViewer 日志 **|Microsoft |Windows**。

--- 

## <a name="get-netadapterqos"></a>Get-netadapterqos

可以通过运行以下 Windows PowerShell 命令来查看\(网络\)适配器 QoS 配置的质量。

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-netadapterqos 预期结果

优先级和流量类应根据使用本指南执行的第一个配置步骤来显示。

![服务优先级和类的质量](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-netadapterqos 意外结果

如果你的结果不是预期的，请执行以下步骤。

1. 确保物理网络适配器支持数据中心桥接\(DCB\)和 QoS
2. 确保网络适配器驱动程序是最新的。

--- 

## <a name="get-smbmultichannelconnection"></a>SmbMultiChannelConnection

你可以使用以下 Windows PowerShell 命令来验证远程节点的 IP 地址是否支持 RDMA\-功能。

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>SmbMultiChannelConnection 预期结果

远程节点的 IP 地址显示为 RDMA 支持的 IP 地址。

![支持 RDMA 的远程节点 IP 地址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>SmbMultiChannelConnection 意外结果

如果你的结果不是预期的，请执行以下步骤。

1. 请确保 ping 的工作方式相同。
2. 请确保防火墙未阻止 SMB 连接启动。 具体而言，为 iWARP 使用 SMB 直通端口5445，为 ROCE 启用445规则。

--- 

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

你可以使用以下命令来验证你为 rdma 启用的虚拟 NIC 是否报告为 SMB 支持的\-rdma。

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>SmbClientNetworkInterface 预期结果

对于启用 RDMA 的虚拟 NIC，必须将其视为支持的 RDMA。

![SMB 报告 Nic 支持 RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>SmbClientNetworkInterface 意外结果

如果你的结果不是预期的，请执行以下步骤。

1. 请确保 ping 的工作方式相同。
2. 请确保防火墙未阻止 SMB 连接启动。

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox 专用\)

如果使用的是 Mellanox 网络适配器，则可以使用**vstat**命令验证 hyper-v 节点上的 RDMA over 聚合\(以太\)网 RoCE 版本。

### <a name="vstat-expected-results"></a>vstat 预期结果

两个节点上的 RoCE 版本必须相同。 这也是一种验证两个节点上的固件版本是否为最新版本的好方法。

![RoCE 版本检查结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat 意外结果

如果你的结果不是预期的，请执行以下步骤。

1. 使用 MlnxDriverCoreSetting 设置正确的 RoCE 版本
2. 从 Mellanox 网站安装最新的固件。

--- 

## <a name="perfmon-counters"></a>Perfmon 计数器

可以在性能监视器中查看计数器，以验证配置的 RDMA 活动。

![性能监视器结果示例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>相关主题

- [使用单个网络适配器汇聚 NIC 配置](cnic-single.md)
- [汇聚 NIC 分组 NIC 配置](cnic-datacenter.md)
- [汇聚 NIC 的物理交换机配置](cnic-app-switch-config.md)

---
