---
title: 通过生成和来宾的 HYPER-V 功能兼容性
description: 列出了生成和密钥的 HYPER-V 功能与兼容的操作系统
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 5950a75da4569979794a5848bd41ab349dc34676
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812662"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>通过生成和来宾的 HYPER-V 功能兼容性

>适用于：Windows Server 2016
  
这篇文章中的表显示了代和一些按类别分组的 HYPER-V 功能与兼容的操作系统。 一般情况下，你将获得与第 2 代虚拟机运行的最新的操作系统功能的最佳可用性。  
  
请记住，某些功能依赖于硬件或其他基础结构。 有关硬件的详细信息，请参阅[System requirements for Windows Server 2016 上的 HYPER-V 要求](System-requirements-for-Hyper-V-on-Windows.md)。 在某些情况下，可以使用任何受支持的来宾操作系统的系统使用一项功能。 支持操作系统的详细信息，请参阅：  
  
* [受支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [受支持的 Windows 来宾操作系统](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>可用性和备份  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
检查点 | 1 和 2 | 任何受支持的来宾  
来宾群集 | 1 和 2 | 来宾运行群集感知应用程序和已安装的 iSCSI 目标软件  
复制 | 1 和 2 | 任何受支持的来宾  
域控制器 | 1 和 2 | 任何受支持的 Windows Server 来宾使用仅生产检查点。 请参阅[支持的 Windows Server 的来宾操作系统](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>计算  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
动态内存 | 1 和 2 | 特定版本的受支持来宾。 请参阅[HYPER-V 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)版本早于 Windows Server 2016 和 Windows 10。  
热添加/删除内存 | 1 和 2 | Windows Server 2016 中，Windows 10  
虚拟 NUMA | 1 和 2 | 任何受支持的来宾  
  
## <a name="development-and-test"></a>开发和测试  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
COM/串行端口 | 1 和 2 <br>**注意：** 对于第 2 代，使用 Windows PowerShell 配置。 有关详细信息，请参阅[添加 COM 端口进行内核调试](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging)。 | 任何受支持的来宾  
  
## <a name="mobility"></a>移动性  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
实时迁移  | 1 和 2 |  任何受支持的来宾  
导入/导出 | 1 和 2 |  任何受支持的来宾  
  
## <a name="networking"></a>网络  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
热添加/删除虚拟网络适配器 | 2 | 任何受支持的来宾  
旧的虚拟网络适配器 | 1 | 任何受支持的来宾  
单根输入/输出虚拟化 (SR-IOV) | 1 和 2 | 64 位 Windows 来宾，从 Windows Server 2012 和 Windows 8 开始。  
虚拟机多队列 (VMMQ) | 1 和 2  | 任何受支持的来宾  
  
## <a name="remote-connection-experience"></a>远程连接体验  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
离散设备分配 (DDA) | 1 和 2 | Windows Server 2016 中，仅与更新 3133690 的 Windows Server 2012 R2 安装，Windows 10 <br> **注意：** 有关更新 3133690 的详细信息，请参阅[这](https://support.microsoft.com/kb/3133690)支持文章。  
增强会话模式 | 1 和 2 | 启用 Windows Server 2016、 Windows Server 2012 R2、 Windows 10 和 Windows 8.1，与远程桌面服务 <br>**注意**：您可能需要也配置主机。 有关详细信息，请参阅[通过 VMConnect 的 HYPER-V 虚拟机上使用本地资源](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。  
RemoteFx | 1 和 2 | 第 1 代上开始的 Windows 8 的 32 位和 64 位 Windows 版本。 <br> 在 64 位 Windows 10 版本上的第 2 代  
  
## <a name="security"></a>安全性  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
安全启动 | 2 | **Linux**:Ubuntu 14.04 及更高版本、 SUSE Linux Enterprise Server 12 和更高版本、 Red Hat Enterprise Linux 7.0 及更高版本和 CentOS 7.0 及更高版本<br>**Windows**:可以在第 2 代虚拟机运行的所有受支持的版本  
受防护的虚拟机 | 2 | **Windows**:可以在第 2 代虚拟机运行的所有受支持的版本  
  
## <a name="storage"></a>存储  
  
功能  | 生成 | 来宾操作系统  
------------- | ------------- | -----------  
共享虚拟硬盘 (VHDX 仅) | 1 和 2  | Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012  
SMB3 | 1 和 2 | 支持所有的 SMB3  
存储空间直通 | 2 | Windows Server 2016  
虚拟光纤通道 | 1 和 2 | Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012  
VHDX 格式 | 1 和 2 | 任何受支持的来宾   
  
  
  
  
    


