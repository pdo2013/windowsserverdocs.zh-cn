---
title: 远程桌面服务支持的配置
description: 提供有关 Windows Server 2016 中 RDS 支持的配置的信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 8571c2220f804a27e4e1a6b744e8e15e38bd53a3
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66453084"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Windows Server 2016 中的远程桌面服务支持的配置

> 适用于：Windows Server 2016

提到远程桌面服务环境支持的配置，最关注的往往是版本互操作性。 大多数环境包含 Windows Server 的多个版本 - 例如，你可能已有一个现有的 Windows Server 2012 R2 RDS 部署，但希望升级到 Windows Server 2016，以利用新的功能（例如，支持 OpenGL\OpenCL、离散设备分配或存储空间直通）。 那么问题是，哪些 RDS 组件可以使用不同的版本，而哪些 RDS 组件需要使用相同的版本？

考虑到这一点，我们提供了以下有关 Windows Server 2016 中的远程桌面服务支持的配置的基本指导原则。

> [!NOTE]
> 请务必查看 [Windows Server 2016 的系统要求](../../get-started/system-requirements.md)。

## <a name="best-practices"></a>最佳做法
- 对远程桌面基础结构 - Web 访问、网关、连接代理和许可证服务器 - 使用 Windows Server 2016。 Windows Server 2016 可与这些组件向后兼容 - 因此，2012 R2 RD 会话主机可以连接到 2016 RD 连接代理，但反之则不行。

- 对于 RD 会话主机 - 集合中的所有会话主机需要处于同一级别，但你可以创建多个集合。 可以创建一个包含 Windows Server 2012 R2 会话主机的集合，并创建一个包含 Windows Server 2016 会话主机的集合。

- 如果将 RD 会话主机升级到 Windows Server 2016，则还可以升级许可证服务器。 请记住，2016 版许可证服务器可以处理来自所有旧版 Windows Server（最低为 Windows Server 2003）的 CAL。

- 请遵循[升级远程桌面服务环境](upgrade-to-rds.md#flow-for-deployment-upgrades)中建议的升级顺序。 

- 如果创建高可用性环境，所有连接代理都需要处于相同的 OS 级别。

## <a name="rd-connection-brokers"></a>RD 连接代理

Windows Server 2016 消除了在使用同样运行 Windows Server 2016 的远程桌面会话主机 (RDSH) 和远程桌面虚拟化主机 (RDVH) 时，可在部署中使用的连接代理数限制。 下表显示了在包含三个或更多个连接代理的高可用性部署中，哪些 RDS 组件版本可以使用 2016 和 2012 R2 版本的连接代理。

| 高可用性部署中包含 3 个以上的连接代理              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Windows Server 2016 连接代理    | 支持 | 支持 | 支持     | 支持     |
| Windows Server 2012 R2 连接代理 | N/A       | N/A       | 支持     | 支持     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Hyper-V 中的 GPU 加速支持
下表详细说明了虚拟机上的 GPU 加速支持。 在确定需求时如需帮助，请参阅[哪种图形虚拟化技术适合你？](rds-graphics-virtualization.md) 有关 DDA 的具体信息，请查看[规划离散设备分配的部署](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

|VM 来宾 OS  |Windows Server 2012 R2 或 Windows Server 2016<br> Hyper-V RemoteFX vGPU（第 1 代 VM） |  Windows Server 2016 Hyper-V RemoteFX vGPU（第 2 代 VM） |  Windows Server 2016 Hyper-V 离散设备分配（第 2 代 VM） |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | 是                                                        | 否                                                     | 否                                                                  |
| Windows 8.1                 | 是                                                        | 否                                                     | 否                                                                  |
| Windows 10 1511 Update      | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2      | 是                                                        | 否                                                     | 是（需要 KB 3133690）                                           |
| Windows Server 2016         | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2 RDSH | 否                                                         | 否                                                     | 是（需要 KB 3133690）                                           |
| Windows Server 2016 RDSH    | 否                                                         | 否                                                     | 是                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>VDI 部署 – 支持的来宾 OS 
Windows Server 2016 RD 虚拟化主机服务器支持以下来宾 OS：

- Windows 10 企业版
- Windows 8.1 企业版 
- Windows 8 企业版 
- Windows 7 SP1 Enterprise 

下表显示了支持的 RD 虚拟化主机的操作系统和来宾操作系统组合：

| RDVH OS 版本        | 来宾 OS 版本           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1、Windows 8、Windows 8.1、Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1、Windows 8、Windows 8.1、Windows 10 |
| Windows Server 2012      | Windows 7 SP1、Windows 8、Windows 8.1 |

> [!NOTE]  
> - Windows Server 2016 远程桌面服务不支持异构集合。 集合中的所有 VM 必须使用相同的 OS 版本。 
> - 在同一台主机上可以使用来宾 OS 版本不同的独立同类集合。 
> - 必须在 Windows Server 2016 Hyper-V 主机上创建 VM 模板，才能将其用作 Windows Server 2016 Hyper-V 主机上的来宾 OS。

## <a name="single-sign-on-sso"></a>单一登录 (SSO)
Windows Server 2016 RDS 支持两种主要的 SSO 体验：

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
