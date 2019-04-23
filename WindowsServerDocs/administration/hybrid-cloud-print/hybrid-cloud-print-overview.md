---
title: Windows Server 混合云打印概述
description: 混合云打印允许 IT 专业人员支持 BYOD 或域打印要求已加入的设备。
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878828"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Windows Server 混合云打印概述

**适用于**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>什么是混合云打印？
**混合云打印**是 Windows Server 2016 的新功能可通过**按需功能**。 它允许 IT 专业人员的人员将他们自己的设备，或将设备加入 Azure Active Directory 支持打印的要求。 这包括 Windows phone、 便携式计算机或运行 Windows 10 或 Windows Mobile 的平板电脑等移动设备。 它提供打印支持从任何位置的人员有权访问 Internet。

对于 IT 管理员**混合云打印**通过使用 Azure 的多重身份验证来验证用户的访问提供安全的用户访问权限的本地打印机。 单一登录 (SSO) 功能可以简化用户体验。 **混合云打印**基于 Windows**打印服务器**角色，为 IT 专业人员提供类似于管理打印机和代码访问安全性的体验。

**混合云打印**允许用户在你的组织能够从他们用于完成其工作的即使它们是离开其办公桌或工作区的设备进行打印。

**混合云打印**支持在 Windows 10 创意者更新和 Windows 10 s。
 
## <a name="feature-summary"></a>功能摘要
**混合云打印**两个主要的服务器端组件组成：**发现**服务，并**Windows 打印**服务。
- **发现**支持用于在云中的打印机发现 Mopria 联盟行业标准的 IIS 服务上运行的服务终结点。
- **Windows 打印**服务终结点运行在 IIS 服务支持行业标准 Internet 打印协议 (IPP) 以确保最广泛的客户端 OS 支持。

## <a name="deployment"></a>部署
**混合云打印**支持几个不同的部署选项，具体取决于你的组织要求用户身份验证的位置。 下面是一个部署可能如下所示：

![显示混合云打印解决方案的图形表示的关系图](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*混合云打印解决方案关系图*

关系图所示：
- **混合云打印**作为用户标识提供者使用 Azure Active Directory。 
- **Windows 打印**服务并**发现**与 Azure Active Directory 以启用要检索所需的用户身份验证令牌，以用于这些服务的客户端设备注册服务终结点。 
- MDM 服务，如**Microsoft Intune**，会为客户端设备设置与连接到 Azure Active Directory 所需的策略**Windows 打印**服务和**发现**服务。

此表包含在关系图中元素的详细信息。  

| 元素 | 描述 |
| ------- | ----------- |
| Azure Active Directory  | 提供并控制用户标识和授权功能 |
| Active Directory        | 提供并控制用户标识和授权功能 |
| Azure AD Connect  | 同步 Azure AD 之间的用户凭据，并在本地 AD。 |
| MDM 服务 (Intune) | 提供了设备策略预配功能，以确保客户端设备 （BYOD 设备） 符合公司策略。 |
| Azure AD Proxy | 提供了建立到 Azure，以允许来自 Internet 的公司网络的特定配置的应用程序流量流向在防火墙后面的长期连接。 |
| Azure Web 应用 | 混合云打印解决方案的核心。 提供了必需的 web 终结点，以发现打印机并发送打印内容的非域加入的设备。 |
| BYOD 设备 / Windows 打印服务器后台处理程序 / 打印机 | 这些是作为-是。 在部署中的功能方面没有变化。 |

有两种安装方法**混合云打印**:
- **按需功能**-请参阅[配置的功能在 Windows Server 中按需](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server)若要了解有关添加和删除角色和功能文件的详细信息。 
- **Windows Server 2016 设置**-管理员可以转到**设置** -> **应用** -> **管理可选功能** -> **添加一项功能**和按需包功能的搜索 
- PowerShell 命令-在 PowerShell 管理员窗口中，运行以下命令：

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
