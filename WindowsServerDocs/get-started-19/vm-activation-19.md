---
title: 自动虚拟机激活
TOCTitle: Automatic VM Activation
description: 如何激活 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 R2 中的 Vm
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375484"
---
# 自动虚拟机激活

> 适用于： Windows Server 2019，Windows Server 半年频道、 Windows Server 2016、 Windows Server 2012 R2

自动虚拟机激活 (AVMA) 充当购买证明机制，有助于确保 Windows 产品依据的产品使用权限和 Microsoft 软件许可条款。

AVMA 允许你在正确激活 Windows server 上安装虚拟机，而无需管理每个单独的虚拟机，即使在断开连接的环境中的产品密钥。 AVMA 将虚拟机激活绑定到许可虚拟化服务器，并在启动时激活虚拟机。 AVMA 还提供了在使用情况和在虚拟机的许可证状态历史记录数据的实时报告。 报告和跟踪数据是虚拟化的服务器上可用。

## 实际应用程序

虚拟化在服务器上使用批量许可或 OEM 许可激活，AVMA 提供以下几个好处。

服务器数据中心管理人员可以使用 AVMA 执行以下操作：

  - 激活远程位置中的虚拟机

  - 激活虚拟机具有或没有 internet 连接

  - 在虚拟化服务器上，跟踪虚拟机使用情况和许可证，而无需在虚拟化的系统上的任何访问权限

有没有产品密钥来管理和在服务器上的没有贴纸来阅读。 虚拟机激活，并且会继续运行，即使它迁移跨虚拟化的服务器的数组。

服务提供商许可协议 (SPLA) 的合作伙伴和其他托管提供商无需与租户共享产品密钥或访问租户的虚拟机，以激活它。 使用 AVMA 时，虚拟机激活是透明的租户。 托管提供商可以使用服务器日志，以验证许可证合规性并跟踪客户端使用历史记录。

## 系统要求

AVMA 需要运行 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter 或 Windows Server 2012 R2 Microsoft 虚拟化的服务器。 

下面是在不同版本的主机可以激活来宾：

|服务器主机版本|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

请注意，这些激活 （Datacenter、 Standard、 或 Essentials） 的所有版本。

此工具不使用其他虚拟化 Server 技术。

## 如何实现 AVMA

1.  在 Windows Server Datacenter 虚拟化服务器上，安装和配置 Microsoft 的 HYPER-V 服务器角色。 有关详细信息，请参阅[安装 HYPER-V 服务器](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md)。

2.  [创建虚拟机](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md)和安装在其上受支持的服务器操作系统。

3.  在虚拟机中安装 AVMA 密钥。 从提升的命令提示符下，运行以下命令：
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

虚拟机将自动激活针对虚拟化服务器许可证。


> [!TIP]
> 你还可以利用任何 Unattend.exe 安装程序文件中的 AVMA 密钥。


## AVMA 键

以下 AVMA 键可以用于 Windows Server 2019。

|版本|   AVMA 密钥|
|-|-|
|数据中心版|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2 D 623 4GF74|
|基础知识|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
以下 AVMA 键可以用于 Windows Server 版本 1809年。

|版本|   AVMA 密钥|
|-|-|
|数据中心版|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2 D 623 4GF74|

以下 AVMA 键可以用于 Windows Server 版本 1803年和 1709年。

|版本|AVMA 密钥|
|-|-|
|数据中心版|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


以下 AVMA 键可以用于 Windows Server 2016。

|版本|AVMA 密钥|
|-|-|
|数据中心版|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|基础知识|B4YNW-62DX9-W8V6M-82649-MHBKQ|


以下 AVMA 键可以用于 Windows Server 2012 R2。

|版本|AVMA 密钥|
|-|-|
|数据中心版|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|基础知识|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## 报告和跟踪

虚拟化的服务器上的注册表 (KVP) 提供的来宾操作系统实时跟踪数据。 在以下注册表项移动与虚拟机，因为你可以获取许可证的信息。 默认情况下 KVP 返回有关虚拟机，包括以下信息：

  - 完全限定的域名

  - 操作系统和安装服务包

  - 处理器体系结构

  - 网络的 IPv4 和 IPv6 地址

  - RDP 地址

有关如何获取此信息的详细信息，请参阅[HYPER-V 脚本： 看着 KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx)。


> [!NOTE]
> KVP 数据不是安全的。 它可以修改的并且不监视的更改。



> [!IMPORTANT]
> 如果 AVMA 密钥将被替换为另一个产品密钥 （零售、 OEM 或批量许可密钥），应删除 KVP 数据。


虚拟化服务器 (EventID 12310) 上的日志文件中提供了有关 AVMA 请求的历史记录数据。

由于 AVMA 激活过程是透明的因此不会显示错误消息。 但是，以下事件捕获在虚拟机 (EventID 12309) 上的日志文件。

|通知|描述|
|-|-|
|AVMA 成功|虚拟机已激活。|
|无效的主机|虚拟化的服务器是无响应。 服务器未运行受支持的 Windows 版本时，会出现此错误。|
|无效的数据|这通常会从故障中的虚拟化的服务器和虚拟机，通常由损坏、 加密或数据不匹配导致之间的通信。|
|激活被拒绝|虚拟化服务器无法激活来宾操作系统，因为 AVMA ID 不匹配。|

