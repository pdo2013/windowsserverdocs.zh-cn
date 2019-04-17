---
title: 安装服务器核心
description: 如何获取和安装服务器核心安装在 Windows Server 2019、 Windows Server 2016 或 Windows Server （半年频道）。
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
ms.sourcegitcommit: 7fc7271745e40f110c54918b55624cadd0d7ff98
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991794"
---
# 安装服务器核心

> 适用范围： Windows Server 2019、Windows Server 2016、Windows Server（半年频道）
  
第一次安装 Windows Server 时，你可以使用以下安装选项：

>[!NOTE]
> 在以下列表中，不带“桌面体验”的版本是服务器核心安装选项

-   Windows Server Standard
-   带桌面体验的 Windows Server Standard
-   Windows Server Datacenter
-   带桌面体验的 Windows Server Datacenter

在安装 Windows Server （半年频道），包括版本 1709年、 1803、 和 1809，你有以下安装选项：

-   Windows Server Standard 
-   Windows Server Datacenter

“服务器核心”选项可减少所需的磁盘空间和潜在的攻击面，因此我们建议选择服务器核心安装，除非你有特殊需求要用到“带桌面体验的服务器”选项中包含的附加用户界面元素和图形管理工具。 如果确实感觉到需要额外的用户界面元素，请参阅[安装带桌面体验的服务器](Getting-Started-with-Server-with-Desktop-Experience.md)。 

使用“服务器核心”选项不会安装标准用户界面（桌面体验）；通过使用命令行、Windows PowerShell 或远程方法来管理服务器。

>[!NOTE]
>
>与某些之前版本的 WindowsServer 不同，安装后无法在服务器核心和具有桌面体验的服务器之间转换。 如果安装服务器核心，但后来决定使用具有桌面体验的服务器，则应重新安装。

**用户界面：** 命令提示符

**在本地安装、配置、卸载服务器角色：** 在 Windows PowerShell 的命令提示符下进行。

**安装、 配置、 卸载服务器角色远程从 Windows 客户端计算机 （或具有桌面体验的服务器安装）：** 使用服务器管理器、 远程服务器管理工具 (RSAT)、 Windows PowerShell 或 Windows Admin Center。

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
- WindowsServer 更新服务器
- Active Directory 权限管理服务器
- 路由和远程访问服务器以及下列子角色：
- 远程桌面服务连接代理
- 授权
- 虚拟化
- 批量激活服务

有关未包含在服务器核心中的角色，请参阅[角色、 角色服务和不在 Windows Server 的服务器核心的功能](../administration/server-core/server-core-removed-roles.md)。

## Windows Server 2019 或 Windows Server 2016 上安装

有关常规安装步骤和适用于 Windows Server (Long Term Servicing Channel) 的选项，请参阅[Windows Server 安装与升级](installation-and-upgrade.md)。

## 在 Windows Server （半年频道） 上安装

适用于 Windows Server （半年频道） 的安装步骤是相同安装以前版本的 Windows Server (从。ISO 映像），有以下例外：
- 不支持将以前版本的 Windows Server 升级到 Windows Server 版本 1709。 始终需要进行重新安装。
   这意味着从 Windows 计算机的桌面运行 setup.exe 时，安装体验不允许升级选项 （它灰显）。
- 适用于 Windows Server （半年频道） 没有评估版本
- 没有任何 OEM 或零售版本。 仅可以通过软件保障或会员计划获得授权 Windows Server （半年频道）。

若要获取 Windows Server 版本 1709，请参阅 [Windows Server 版本 1709 简介](get-started-with-1709.md)。

若要获取 Windows Server 版本 1803年，请参阅[Windows Server 简介，版本 1803年](get-started-with-1803.md)。

若要查看 Windows Server 中新增，版本 1809，请参阅[什么是 Windows Server 版本 1809年中的新增功能](whats-new-in-windows-server-1809.md)
