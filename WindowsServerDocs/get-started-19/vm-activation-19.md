---
title: 虚拟机自动激活
TOCTitle: Automatic VM Activation
description: 如何激活在 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 R2 中的 Vm
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822088"
---
# <a name="automatic-virtual-machine-activation"></a>虚拟机自动激活

> 适用于：Windows Server 2019，Windows Server 半年频道，Windows Server 2016 中，Windows Server 2012 R2

虚拟机自动激活 (AVMA) 充当一个购买证明机制，帮助确保用户根据产品使用权和 Microsoft 软件许可条款使用 Windows 产品。

AVMA 可让你在正确激活的 Windows Server 上安装虚拟机，而无需管理每一台虚拟机的产品密钥，即使在连接断开的环境中，也是如此。 AVMA 将虚拟机激活绑定到经授权的虚拟化服务器，并在其启动时激活该虚拟机。 AVMA 还提供有关使用情况的实时报告，以及有关虚拟机许可状态的历史数据。 虚拟服务器上会提供报告和跟踪数据。

## <a name="practical-applications"></a>实际应用程序

在使用批量许可或 OEM 许可激活的虚拟化服务器上，AVMA 具有多种优势。

服务器数据中心管理员可以使用 AVMA 来执行以下操作：

  - 在远程位置激活虚拟机

  - 使用或不使用 Internet 连接激活虚拟机

  - 从虚拟化服务器跟踪虚拟机使用情况和许可证，且无需虚拟化系统的任何访问权限

无需管理任何产品密钥，并且无需在服务器上读取任何标贴。 即使将虚拟机迁移到一组虚拟化服务器，该虚拟机也能激活并持续工作。

服务提供商许可协议 (SPLA) 伙伴和其他托管提供商无需与租户共享产品密钥或访问租户的虚拟机就能激活该虚拟机。 使用 AVMA 时，虚拟机激活对于租户是透明的。 托管提供商可以使用服务器日志来验证许可证遵从性以及跟踪客户端使用历史记录。

## <a name="system-requirements"></a>系统要求

AVMA 要求运行 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter 或 Windows Server 2012 R2 的 Microsoft 虚拟化服务器。 

下面是在不同版本的主机可以激活来宾：

|服务器主机版本|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

请注意这些激活所有版本 （数据中心、 标准或 Essentials）。

此工具并不适用于其他虚拟化服务器技术。

## <a name="how-to-implement-avma"></a>如何实施 AVMA

1.  在 Windows Server Datacenter 虚拟化服务器上，安装和配置 Microsoft HYPER-V 服务器角色。 有关详细信息，请参阅[安装 HYPER-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md)。

2.  [创建虚拟机](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md)和其上安装受支持的服务器操作系统。

3.  在该虚拟机中安装 AVMA 密钥。 在提升的命令提示符下运行以下命令：
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

虚拟机将会针对虚拟化服务器自动激活许可证。


> [!TIP]
> 也可以使用任何 Unattend.exe 安装文件中的 AVMA 密钥。


## <a name="avma-keys"></a>AVMA 密钥

以下 AVMA 密钥可以用于 Windows Server 2019。

|版本|   AVMA 密钥|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|标准|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
适用于 Windows Server，版本 1809年，可以使用以下 AVMA 密钥。

|版本|   AVMA 密钥|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|标准|  TNK62-RXVTB-4P47B-2D623-4GF74|

以下 AVMA 密钥可以用于 Windows Server 版本 1803年和 1709年。

|版本|AVMA 密钥|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|标准|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Windows Server 2016，可以使用以下 AVMA 密钥。

|版本|AVMA 密钥|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|标准|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essentials|B4YNW-62DX9-W8V6M-82649-MHBKQ|


可以针对 Windows Server 2012 R2 使用以下 AVMA 密钥。

|版本|AVMA 密钥|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|标准|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essentials|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>报告和跟踪

虚拟化服务器上的注册表 (KVP) 为来宾操作系统提供实时跟踪数据。 由于注册表项会伴随虚拟机，因此你还可以获取许可证信息。 默认情况下，KVP 返回有关虚拟机的信息，其中包括：

  - 完全限定的域名

  - 安装的操作系统和 Service Pack

  - 处理器体系结构

  - IPv4 和 IPv6 网络地址

  - RDP 地址

有关如何获取此信息的详细信息，请参阅[HYPER-V 脚本：查看 KVP guestintrinsicexchangeitems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx)。


> [!NOTE]
> KVP 数据未受保护。 可以修改它，对其所做的更改不受监视。



> [!IMPORTANT]
> 如果将 AVMA 密钥更换为其他产品密钥（零售、OEM 或批量许可密钥），则应删除 KVP 数据。


虚拟化服务器上的日志文件中提供了有关 AVMA 请求的历史数据 (EventID 12310)。

由于 AVMA 激活过程是透明的，因此不显示任何错误消息。 但是，虚拟机上的日志文件中会捕获以下事件 (EventID 12309)。

|通知|描述|
|-|-|
|AVMA 成功|虚拟机已激活。|
|无效主机|虚拟化服务器无响应。 如果服务器运行了不支持的 Windows 版本，则可能会发生此情况。|
|无效数据|这通常是由于数据损坏、加密或数据不匹配，导致虚拟化服务器与虚拟机之间的通信失败。|
|拒绝激活|由于 AVMA ID 不匹配，虚拟化服务器无法激活来宾操作系统。|

