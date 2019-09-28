---
title: 安装服务器核心
description: 如何在 Windows Server 2019、Windows Server 2016 或 Windows Server（半年频道）上获取并安装服务器核心安装。
ms.prod: windows-server
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e6264a59a837003e49e82529750cfb153cc37b92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360338"
---
# <a name="install-server-core"></a>安装服务器核心

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）
  
首次安装 Windows Server 时，你可以使用以下安装选项：

>[!NOTE]
> 在以下列表中，不带“桌面体验”的版本是服务器核心安装选项

-   Windows Server Standard
-   带桌面体验的 Windows Server Standard
-   Windows Server Datacenter
-   带桌面体验的 Windows Server Datacenter

安装 Windows Server（半年频道）时，你可以使用以下安装选项：

-   Windows Server Standard 
-   Windows Server Datacenter

“服务器核心”选项可减少所需的磁盘空间和潜在的攻击面，因此我们建议选择服务器核心安装，除非你有特殊需求要用到“带桌面体验的服务器”选项中包含的附加用户界面元素和图形管理工具。 如果确实感觉到需要额外的用户界面元素，请参阅[安装带桌面体验的服务器](Getting-Started-with-Server-with-Desktop-Experience.md)。 

使用“服务器核心”选项不会安装标准用户界面（桌面体验）；通过使用命令行、Windows PowerShell 或远程方法来管理服务器。

>[!NOTE]
>
>与某些之前版本的 Windows Server 不同，安装后无法在服务器核心和具有桌面体验的服务器之间转换。 如果安装服务器核心，但后来决定使用具有桌面体验的服务器，则应重新安装。

**用户界面：** 命令提示符

**在本地安装、配置、卸载服务器角色：** 在 Windows PowerShell 的命令提示符下进行。

从 Windows 客户端计算机（或安装了桌面体验的服务器）中远程安装、配置、卸载服务器角色：  利用服务器管理器、远程服务器管理工具 (RSAT)、Windows PowerShell 或 Windows Admin Center 来进行。

>[!NOTE]
>
>对于 RSAT，必须使用 Windows 10 版本。
>Microsoft 管理控制台在本地不可用。

可用的服务器角色示例： 

- Active Directory 证书服务
- Active Directory 域服务
- DHCP 服务器
- DNS 服务器
- 文件服务（包括文件服务器资源管理器）
- Active Directory 轻型目录服务 (AD LDS)
- Hyper-V
- 打印和文档服务
- 流式媒体服务
- Web 服务器（包括 ASP.NET 的一个子集）
- Windows Server 更新服务器
- Active Directory 权限管理服务器
- 路由和远程访问服务器以及下列子角色：
   - 远程桌面服务连接代理
   - 授权
   - 虚拟化
   - 批量激活服务

对于未包含在服务器核心中的角色，请参阅[不在 Windows Server 中的角色、角色服务和功能 - 服务器核心](../administration/server-core/server-core-removed-roles.md)。

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>在 Windows Server 2019 或 Windows Server 2016 上安装

有关适用于 Windows Server（长期服务频道）的常规安装步骤和选项，请参阅 [Windows Server 安装和升级](installation-and-upgrade.md)。

## <a name="installing-on-windows-server-semi-annual-channel"></a>在 Windows Server（半年频道）上安装

适用于 Windows Server（半年频道）的安装步骤与（从 .ISO 映像）安装以前版本的 Windows Server 相同，但也存在以下例外：

- 不支持将以前版本的 Windows Server 升级到 Windows Server 版本 1709。 始终需要进行重新安装。
   这意味着从 Windows 计算机的桌面运行 setup.exe 时，安装体验不允许使用升级选项（升级选项将显示为灰色）。
- Windows Server（半年频道）没有评估版本
- 没有任何 OEM 或零售版本。 Windows Server（半年频道）只能通过软件保障或会员计划获得授权。

有关半年频道的更多信息，请参阅[服务频道的比较](../get-started-19/servicing-channels-19.md)。

若要了解 Windows Server 半年频道中的新增功能，请参阅 [Windows Server 中的新增功能](whats-new-in-windows-server.md)
