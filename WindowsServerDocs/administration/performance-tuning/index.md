---
title: Windows Server 2016 性能优化指南
description: Windows Server 2016 性能优化指南
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1445ae40428bc8626f8e2a12cbfc2a1e6f41592f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891958"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Windows Server 2016 性能优化指南

在你的组织中运行服务器系统时，使用默认服务器设置可能无法满足业务需求。 例如，你可能需要在服务器上实现尽可能低的能量消耗，或尽可能低的延迟，或尽可能高的吞吐量。 本指南提供了一组准则，可以使用它们来调整 Windows Server 2016 中的服务器设置和提高性能或能源效率，尤其是工作负载的性质随时间的推移变化不大时。

重要的是，优化更改应当考虑你的服务器的硬件、工作负荷、电源预算和性能目标。 本指南介绍了每个设置及其潜在影响，以帮助你就其与系统、工作负荷、性能和能源使用目标的相关性做出信息充分的决策。

> [!warning]
> 注册表设置和优化参数在不同版本的 Windows Server 之间发生了重大变化。 请确保使用最新优化准则来避免意外的结果。

## <a name="in-this-guide"></a>本指南包含的内容
本指南将 Windows Server 2016 的性能和优化指南划分为三个优化类别：

|服务器硬件 | 服务器角色 | 服务器子系统 |
|:---:|:---:|:---:|
|[硬件性能注意事项](hardware/index.md) |[Active Directory 服务器](role/active-directory-server/index.md) |[缓存和内存管理](subsystem/cache-memory-management/index.md)|
|[硬件电源注意事项](hardware/power.md)|[文件服务器](role/file-server/index.md)|[网络子系统](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Hyper-V 服务器](role/hyper-v-server/index.md)|[存储空间直通](subsystem/storage-spaces-direct/index.md)|
||[远程桌面服务](role/remote-desktop/session-hosts.md)|[软件定义的网络 (SDN)](subsystem/software-defined-networking/index.md)|
||[Web 服务器](role/web-server/index.md)||
||[Windows Server 容器](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>此版本中的更改

### <a name="sections-added"></a>添加的部分
- [Nano Server 安装类型配置注意事项](../../get-started/getting-started-with-nano-server.md)


- [软件定义的网络](subsystem/software-defined-networking/index.md)，包括 [HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) 和 [SLB 网关配置指南](subsystem/software-defined-networking/slb-gateway-performance.md)

- [存储空间直通](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 和 HTTP2](role/web-server/http-performance.md)

- [Windows Server 容器](role/windows-server-container/index.md)

### <a name="sections-changed"></a>更改的部分

- 更新了 [Active Directory 指南](role/active-directory-server/index.md)部分

- 更新了[文件服务器指南](role/file-server/index.md)部分

- 更新了 [Web 服务器指南](role/web-server/index.md)部分

- 更新了[硬件电源指南](hardware/power.md)部分

- 更新了 [PowerShell 优化指南](powershell/index.md)部分

- 对 [Hyper-V 指南](role/hyper-v-server/index.md)部分进行了重大更新

- *删除了针对工作负荷的性能优化*，在[“其他优化资源”一文](additional-resources.md)中添加了相关资源的线索

- *删除了专用存储部分*，代之以新的[存储空间直通](subsystem/storage-spaces-direct/index.md)部分和规范的 Technet 内容

- *删除了专用网络部分*，代之以规范的 Technet 内容  
