---
title: 远程桌面服务支持的配置
description: 提供有关 Windows Server 2016 中的 RDS 的支持的配置信息。
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
ms.openlocfilehash: 894ea8b134ae5b871a2978e3f72e683c12346fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850198"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Windows Server 2016 中的远程桌面服务支持的配置

> 适用于：Windows Server 2016

谈到的远程桌面服务环境支持的配置，最大问题往往是版本互操作性。 大多数环境包含多个版本的 Windows Server-例如，你可能拥有现有的 Windows Server 2012 R2 RDS 部署，但想要升级到 Windows Server 2016 以充分利用新功能 （如对 OpenGL\OpenCL，离散的支持设备分配或存储空间直通）。 那么问题将变为，哪些 RDS 组件可以使用不同版本，这需要相同？

因此这一点后，以下是有关 Windows Server 2016 中的远程桌面服务支持的配置的基本指导原则。

> [!NOTE]
> 请确保查看[Windows Server 2016 的系统要求](../../get-started/system-requirements.md)。

## <a name="best-practices"></a>最佳做法
- 为远程桌面基础结构的 Web 访问、 网关、 连接代理和许可证服务器使用 Windows Server 2016。 Windows Server 2016 是为这些组件的向后兼容，因此 2012 R2 RD 会话主机可以连接到 2016 RD 连接代理，但反之不成立。

- 有关 RD 会话主机-集合中的所有会话主机都需要处于同一级别，但可以有多个集合。 您可以与 Windows Server 2012 R2 会话主机的集合，另一个使用 Windows Server 2016 会话主机。

- 如果升级到 Windows Server 2016 在 RD 会话主机，还升级许可证服务器。 请记住 2016年许可证服务器可以从所有以前版本的 Windows Server、 Windows Server 2003 下处理 Cal。

- 遵循中的建议的升级顺序[升级您的远程桌面服务环境](upgrade-to-rds.md#flow-for-deployment-upgrades)。 

- 如果要创建高度可用的环境，所有连接代理需要在相同的 OS 级别。

## <a name="rd-connection-brokers"></a>RD 连接代理

使用远程桌面会话主机 (RDSH) 和远程桌面虚拟化主机 (RDVH) 也运行 Windows Server 2016 时，Windows Server 2016 部署中删除的连接可以具有的代理数的限制。 下表显示 2016年和 2012 R2 版本连接代理的 RDS 组件工作的版本中使用三个或多个连接代理高可用性部署。

| 3 + 连接代理高可用性              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Windows Server 2016 连接代理    | 支持 | 支持 | 支持     | 支持     |
| Windows Server 2012 R2 Connection Broker | 不可用       | 不可用       | 支持     | 支持     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>对使用 HYPER-V 的 GPU 加速的支持
下表详细说明虚拟机上的 GPU 加速的支持。 请参阅[图形虚拟化技术最适合您？](rds-graphics-virtualization.md)有关找出所需的帮助。 有关 DDA 的特定信息，请查看[规划部署离散设备分配](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

|VM 来宾 OS  |Windows Server 2012 R2 或 Windows Server 2016<br> Hyper V RemoteFX vGPU （1 代虚拟机） |  Windows Server 2016 HYPER-V RemoteFX vGPU (Gen 2 VM) |  Windows Server 2016 的 HYPER-V 离散设备分配 （第 2 代 VM） |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | 是                                                        | 否                                                     | 否                                                                  |
| Windows 8.1                 | 是                                                        | 否                                                     | 否                                                                  |
| Windows 10 1511年更新      | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2      | 是                                                        | 否                                                     | 是 （需要 KB 3133690）                                           |
| Windows Server 2016         | 是                                                        | 是                                                    | 是                                                                 |
| Windows Server 2012 R2 RDSH | 否                                                         | 否                                                     | 是 （需要 KB 3133690）                                           |
| Windows Server 2016 RDSH    | 否                                                         | 否                                                     | 是                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>VDI 部署 – 受支持的来宾 Os 
Windows Server 2016 RD 虚拟化主机服务器支持以下来宾 Os:

- Windows 10 企业版
- Windows 8.1 企业版 
- Windows 8 企业版 
- Windows 7 SP1 Enterprise 

下表显示了支持的 RD 虚拟化主机的操作系统和来宾操作系统组合：

| RDVH OS 版本        | 来宾 OS 版本           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1，Windows 8，Windows 8.1，Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1，Windows 8，Windows 8.1，Windows 10 |
| Windows Server 2012      | Windows 7 SP1，Windows 8，Windows 8.1 |

> [!NOTE]  
> - Windows Server 2016 远程桌面服务不支持异类集合。 集合中的所有 Vm 必须都是相同的 OS 版本。 
> - 在同一主机上，可以有不同的来宾操作系统版本的单独同类集合。 
> - 必须使用作为 Windows Server 2016 HYPER-V 主机上的来宾 OS 的 Windows Server 2016 HYPER-V 主机上创建虚拟机模板。

## <a name="single-sign-on-sso"></a>单一登录 (SSO)
Windows Server 2016 RDS 支持两种主要的 SSO 体验：

 - 应用 （Windows、 iOS、 Android 和 Mac 上远程桌面应用程序）
 - Web SSO
 
使用远程桌面应用程序，您可以存储凭据的连接信息的过程 ([Mac](clients\remote-desktop-mac.md)) 或托管帐户的一部分 ([iOS](clients\remote-desktop-ios.md#manage-your-user-accounts)， [Android](clients\remote-desktop-android.md#manage-your-user-accounts)，Windows)安全地通过唯一的每个 OS 的机制。

若要通过 Windows 上的收件箱远程桌面连接客户端连接到桌面和 remoteapps 所使用 SSO，必须连接到 RD 网页通过 Internet Explorer。 在服务器端需要下列配置选项。 Web SSO 不支持任何其他配置：

 - RD Web 设置为基于表单的身份验证 （默认值）
 - RD 网关设置为密码身份验证 （默认值）
 - RDS 部署设置为"使用 RD 网关凭据对于远程计算机"（默认值） 中的 RD 网关属性

> [!NOTE]
> 由于所需的配置选项，而与智能卡不支持 Web SSO。 通过智能卡登录名可能会遇到多个提示登录的用户。

有关创建 VDI 部署的远程桌面服务的详细信息，请查看[支持 Windows 10 安全配置的远程桌面服务 VDI](rds-vdi-supported-config.md)。

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>使用应用程序代理服务使用远程桌面服务

可用于远程桌面服务，web 客户端，除了[Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop)。 远程桌面服务不支持使用[Web 应用程序代理](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server)，包含在 Windows Server 2016 及更早版本。
