---
title: 安装服务器核心
description: 如何获取并安装服务器核心安装上 Windows Server 2019、 Windows Server 2016 或 Windows Server （半年频道）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869018"
---
# <a name="install-server-core"></a>安装服务器核心

> 适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）
  
第一次安装 Windows Server 时可以以下安装选项：

>[!NOTE]
> 在以下列表中，不带“桌面体验”的版本是服务器核心安装选项

-   Windows Server Standard
-   带桌面体验的 Windows Server Standard
-   Windows Server Datacenter
-   带桌面体验的 Windows Server Datacenter

在安装 Windows Server （半年频道），包括版本 1709年、 1803 和 1809，您具有以下安装选项：

-   Windows Server Standard 
-   Windows Server Datacenter

“服务器核心”选项可减少所需的磁盘空间和潜在的攻击面，因此我们建议选择服务器核心安装，除非你有特殊需求要用到“带桌面体验的服务器”选项中包含的附加用户界面元素和图形管理工具。 如果确实感觉到需要额外的用户界面元素，请参阅[安装带桌面体验的服务器](Getting-Started-with-Server-with-Desktop-Experience.md)。 

使用“服务器核心”选项不会安装标准用户界面（桌面体验）；通过使用命令行、Windows PowerShell 或远程方法来管理服务器。

>[!NOTE]
>
>与某些之前版本的 Windows Server 不同，安装后无法在服务器核心和具有桌面体验的服务器之间转换。 如果安装服务器核心，但后来决定使用具有桌面体验的服务器，则应重新安装。

**用户界面：** 命令提示符

**在本地安装、配置、卸载服务器角色：** 在 Windows PowerShell 的命令提示符下进行。

**安装、 配置、 从 Windows 客户端计算机 （或具有桌面体验安装的服务器） 远程卸载服务器角色：** 使用服务器管理器、 远程服务器管理工具 (RSAT)、 Windows PowerShell 或 Windows Admin Center.

>[!NOTE]
>
>对于 RSAT，必须使用 Windows 10 版本。
>Microsoft 管理控制台在本地不可用。

**可用的示例服务器角色：**

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

对于未包含在服务器核心角色，请参阅[角色、 角色服务和功能不在 Windows Server 的服务器核心](../administration/server-core/server-core-removed-roles.md)。

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>在 Windows Server 2019 或 Windows Server 2016 上安装

常规安装步骤和适用于 Windows Server （long 类型的值术语维护服务频道） 选项，请参阅[Windows Server 安装和升级](installation-and-upgrade.md)。

## <a name="installing-on-windows-server-semi-annual-channel"></a>Windows Server （半年频道） 上安装

适用于 Windows Server （半年频道） 的安装步骤将与安装的 Windows Server 的以前版本相同 (从。ISO 映像），但存在以下例外：
- 不支持将以前版本的 Windows Server 升级到 Windows Server 版本 1709。 始终需要进行重新安装。
   这意味着从 Windows 计算机的桌面上运行 setup.exe 时，安装体验不允许升级选项 （它将灰显）。
- 没有适用于 Windows Server （半年频道） 评估版本
- 没有任何 OEM 或零售版本。 仅可以通过软件保障常客或獎授予许可 Windows Server （半年频道）。

若要获取 Windows Server 版本 1709，请参阅 [Windows Server 版本 1709 简介](get-started-with-1709.md)。

若要获取 Windows Server 1803 的版本，请参阅[简介 Windows Server 版本 1803年](get-started-with-1803.md)。

若要查看新增功能在 Windows Server，版本 1809，请参阅[What's New in Windows Server 版本 1809年](whats-new-in-windows-server-1809.md)
