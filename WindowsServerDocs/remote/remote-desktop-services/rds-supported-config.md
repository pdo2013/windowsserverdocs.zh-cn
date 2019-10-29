---
title: 远程桌面服务支持的配置
description: 介绍 Windows Server 2016 和 Windows Server 2019 中 RDS 支持的配置。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 7d4641e2bb40a9a70264c68d0268208a30f36a69
ms.sourcegitcommit: 3262c5c7cece9f2adf2b56f06b7ead38754a451c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812298"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>远程桌面服务支持的配置

> 适用于：Windows Server 2016、Windows Server 2019

提到远程桌面服务环境支持的配置，最关注的往往是版本互操作性。 大多数环境包含 Windows Server 的多个版本 - 例如，你可能已有一个现有的 Windows Server 2012 R2 RDS 部署，但希望升级到 Windows Server 2016，以利用新的功能（例如，支持 OpenGL\OpenCL、离散设备分配或存储空间直通）。 那么问题是，哪些 RDS 组件可以使用不同的版本，而哪些 RDS 组件需要使用相同的版本？

记住这一点，下面就是 Windows Server 中远程桌面服务支持的配置的基本准则。

> [!NOTE]
> 请务必查看 [Windows Server 2016 的系统要求](../../get-started/system-requirements.md)和 [Windows Server 2019 的系统要求](../../get-started-19/sys-reqs-19.md)。

## <a name="best-practices"></a>最佳做法

- 将 Windows Server 2019 用于远程桌面基础结构（Web 访问、网关、连接代理和许可证服务器）。 Windows Server 2019 与这些组件向后兼容，这意味着 Windows Server 2016 或 Windows Server 2012 R2 RD 会话主机可连接到 2019 RD 连接代理，但反之则不可。

- 对于 RD 会话主机 - 集合中的所有会话主机需要处于同一级别，但你可以创建多个集合。 你可创建一个包含 Windows Server 2016 会话主机的集合，并创建一个包含 Windows Server 2019 会话主机的集合。

- 如果将 RD 会话主机升级到 Windows Server 2019，则还要升级许可证服务器。 请记住，2019 版许可证服务器可处理来自所有旧版 Windows Server（最低为 Windows Server 2003）的 CAL。

- 请遵循[升级远程桌面服务环境](upgrade-to-rds.md#flow-for-deployment-upgrades)中建议的升级顺序。

- 如果创建高可用性环境，所有连接代理都需要处于相同的 OS 级别。

## <a name="rd-connection-brokers"></a>RD 连接代理

Windows Server 2016 消除了在使用同样运行 Windows Server 2016 的远程桌面会话主机 (RDSH) 和远程桌面虚拟化主机 (RDVH) 时，可在部署中使用的连接代理数限制。 下表显示了在包含三个或更多个连接代理的高可用性部署中，哪些 RDS 组件版本可以使用 2016 和 2012 R2 版本的连接代理。

|高可用性部署中包含 3 个以上的连接代理|RDSH 或 RDVH 2019|RDSH 或 RDVH 2016|RDSH 或 RDVH 2012 R2|
|---|---|---|---|
 |Windows Server 2019 连接代理|支持|支持|支持|
 |Windows Server 2016 连接代理|N/A|支持|支持|
 |Windows Server 2012 R2 连接代理|N/A|N/A|不支持|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>支持图形处理单元 (GPU) 加速

远程桌面服务支持配有 GPU 的系统。 可通过远程连接使用需要 GPU 的应用程序。 此外，还可启用 GPU 加速的呈现和编程，以提升性能和可伸缩性。

远程桌面服务会话主机是单会话客户端操作系统，它们可通过多种方式使用提供给操作系统的物理或虚拟 GPU，例如 [Azure GPU 优化虚拟机大小](/en-us/azure/virtual-machines/windows/sizes-gpu)、可用于物理 RDSH 服务器的 GPU、RemoteFX vGPU（仅限 Windows Server 2016），以及由受支持的虚拟机监控程序提供给 VM 的 GPU。

在确定需求时如需帮助，请参阅[哪种图形虚拟化技术适合你？](rds-graphics-virtualization.md) 有关 DDA 的具体信息，请查看[规划离散设备分配的部署](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

GPU 供应商可对 RDSH 场景使用单独的许可方案，也可限制 GPU 在服务器操作系统上的使用。请咨询你喜欢的供应商，了解相关要求。

非 Microsoft 虚拟机监控程序或 Cloud Platform 提供的 GPU 必须具有由 WHQL 进行数字签名的驱动程序并由 GPU 供应商提供。

### <a name="remote-desktop-session-host-support-for-gpus"></a>远程桌面会话主机对 GPU 的支持

下表显示了不同版本的 RDSH 主机支持的方案。

|功能|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|对所有 RDP 会话使用硬件 GPU|否|是|是|是|
|H.264/AVC 硬件编码（若 GPU 支持）|否|否|是|是|
|提供给操作系统的多个 GPU 之间的负载均衡|否|否|否|是|
|力图最大程度减少带宽使用的 H.264/AVC 编码优化|否|否|否|是|
|针对 4K 分辨率的 H.264/AVC 支持|否|否|否|是|

### <a name="vdi-support-for-gpus"></a>针对 GPU 的 VDI 支持

下表显示了客户端操作系统中对 GPU 方案的支持。

|功能|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|对所有 RDP 会话使用硬件 GPU|否|是|是|
|H.264/AVC 硬件编码（若 GPU 支持）|否|否|Windows 10 1703 及更高版本|
|提供给操作系统的多个 GPU 之间的负载均衡|否|否|Windows 10 1803 及更高版本|
|力图最大程度减少带宽使用的 H.264/AVC 编码优化|否|否|Windows 10 1803 及更高版本|
|针对 4K 分辨率的 H.264/AVC 支持|否|否|Windows 10 1803 及更高版本|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>RemoteFX 3D 显示适配器 (vGPU) 支持

当 VM 以 Hyper-V 来宾的身份在 Windows Server 2012 R2 或 Windows Server 2016 上运行时，远程桌面服务支持 RemoteFX vGPU。 下列来宾操作系统具有 RemoteFX vGPU 支持：

- Windows 7 SP1
- Windows 8.1
- Windows 10 1703 或更高版本
- 仅限单会话部署中的 Windows Server 2016
- 仅限单会话部署中的 Windows Server 2019

### <a name="discrete-device-assignment-support"></a>离散设备分配支持

远程桌面服务支持从 Windows Server 2016 或 Windows Server 2019 Hyper-V 主机中通过离散设备分配提供的物理 GPU。 有关更多详细信息，请参阅[离散设备分配部署规划](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。


## <a name="vdi-deployment--supported-guest-oses"></a>VDI 部署 - 支持的来宾操作系统

Windows Server 2016 和 Windows Server 2019 RD 虚拟化主机服务器支持以下来宾操作系统：

- Windows 10 企业版
- Windows 8.1 企业版
- Windows 7 SP1 Enterprise

> [!NOTE]
> - 远程桌面服务不支持异构会话集合。 集合中所有 VM 的操作系统版本必须相同。
> - 在同一台主机上可以使用来宾 OS 版本不同的独立同类集合。
> - 用于运行 VM 的 Hyper-V 主机必须与用于创建原始 VM 模板的 Hyper-V 主机版本相同。

## <a name="single-sign-on"></a>单一登录

Windows Server 2016 和 Windows Server 2019 RDS 支持两种主要的 SSO 体验：

- 应用内（Windows、iOS、Android 和 Mac 上的远程桌面应用程序）
- Web SSO

使用远程桌面应用程序，可以安全地通过每个 OS 特有的机制，将凭据存储为连接信息的一部分 ([Mac](clients/remote-desktop-mac.md))，或存储为托管帐户的一部分（[iOS](clients/remote-desktop-ios.md#manage-your-user-accounts)、[Android](clients/remote-desktop-android.md#manage-your-user-accounts)、Windows）。

若要通过 Windows 上的内置远程桌面连接客户端使用 SSO 连接到桌面和 RemoteApp，必须通过 Internet Explorer 连接到 RD 网页。 需要在服务器端使用以下配置选项。 Web SSO 不支持任何其他配置：

- “RD Web”设置为“基于表单的身份验证”（默认设置）
- “RD 网关”设置为“密码身份验证”（默认设置）
- “RD 网关”属性中的“RDS 部署”设置为“使用远程计算机的 RD 网关凭据”（默认设置）

> [!NOTE]
> 由于使用了所需的配置选项，Web SSO 不支持智能卡。 通过智能卡登录的用户可能会看到多个登录提示。

有关为远程桌面服务创建 VDI 部署的详细信息，请查看[远程桌面服务 VDI 支持的 Windows 10 安全配置](rds-vdi-supported-config.md)。

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>将远程桌面服务与应用程序代理服务配合使用

可将远程桌面服务（Web 客户端除外）与 [Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop)配合使用。 远程桌面服务不支持使用 Windows Server 2016 及更低版本中包含的 [Web 应用程序代理](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server)。
