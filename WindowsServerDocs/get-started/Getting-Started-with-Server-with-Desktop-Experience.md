---
title: 安装具有桌面体验的服务器
description: '说明如何获取和安装具有桌面体验安装的服务器 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: eb2e5be2ed19fe7cd64f6c6bd64ca9afafd93bff
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2018
ms.locfileid: "2711802"
---
# 安装具有桌面体验的服务器
> 适用于：WindowsServer 2016
  

使用安装向导安装 WindowsServer 2016 时，可以在 **WindowsServer 2016** 和 **WindowsServer（带有桌面体验的服务器）** 之间进行选择。 “带桌面体验的服务器”选项是 WindowsServer 2016 中的选项，相当于安装了桌面体验功能的 WindowsServer 2012 R2 中的完全安装选项。 如果没有在安装向导中进行选择，则会安装 **WindowsServer 2016**；这是**服务器核心**安装选项。

选择“带有桌面体验的服务器”选项将安装标准用户界面和所有工具，其中包括需要在 WindowsServer 2012 R2 中单独安装的客户端体验功能。 将会通过服务器管理器或其他方法安装服务器角色和功能。 与“服务器核心”选项相比，它需要更多的磁盘空间、具有更高的服务要求，因此建议选择服务器核心安装，除非你有特殊需求要用到“带有桌面体验的服务器”选项中包含的附加用户界面元素和图形管理工具。 如果感觉可以不借助其他元素进行操作，请参阅[安装服务器核心](Getting-Started-with-Server-Core.md)。 有关更轻量的选项，请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md)。

>[!NOTE]
>
>与某些之前版本的 WindowsServer 不同，安装后无法在服务器核心和具有桌面体验的服务器之间转换。 如果安装具有桌面体验的服务器，但后来决定使用服务器核心，则应重新安装。

**用户界面：** 标准图形用户界面（“服务器图形 Shell”）。 服务器图形 Shell 包括新的 Windows10 shell。 默认情况下，此选项会安装的特定 Windows 功能是 User-Interfaces-Infra、Server-GUI-Shell、Server-GUI-Mgmt-Infra、InkAndHandwritingServices、ServerMediaFoundation 和桌面体验。 此版本的服务器管理器中会显示这些功能，但不支持对其进行卸载，且这些功能不适用于将来版本。

**在本地安装、配置、卸载服务器角色：** 使用服务器管理器或 Windows PowerShell 进行

**远程安装、配置、卸载服务器角色：** 使用服务器管理器、远程服务器、RSAT 或 Windows PowerShell 进行

**Microsoft 管理控制台：已安装**

## 安装方案

### 评估
可以从 [WindowsServer 评估](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) 获取 WindowsServer 的 180 天许可证评估副本。 选择 **WindowsServer 2016 | 64 位 ISO 选项**下载，或访问 **WindowsServer 2016 | 虚拟实验室**。

> [!IMPORTANT]  
> 对于 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，只能将使用“桌面体验”选项（非“服务器核心”选项）安装的 Windows Server 2016 从评估版转换为零售版。 从 14393.0.161119-1705.RS1_REFRESH 版本和更高版本开始，无论使用哪个安装选项，你都可以将评估版本转换为零售版本。


### 全新安装

若要从媒体安装“带有桌面体验的服务器”安装选项，请将媒体插入驱动器中，重启计算机，然后运行 Setup.exe。 在打开的向导中，选择 **Windows 服务器（带有桌面体验的服务器）**（Standard 或 Datacenter），然后完成该向导。

### 升级
**升级**表示从现有的操作系统发行版过渡到更新的发行版，同时使用相同的硬件。

如果你已经有相应的 WindowsServer 产品的完全安装，则可以升级到安装有桌面体验的 WindowsServer 2016 相应版本的服务器，如下所示。

> [!IMPORTANT]  
> 在此版本中，升级最适合用于虚拟机，其中进行成功升级不需要特定 OEM 硬件驱动程序。 另外，推荐的选项为迁移。  

- 不支持从 32 位到 64 位体系结构的就地升级。 所有版本的 WindowsServer 2016 都仅有 64 位。
- 不支持从一种语言到另一种语言的就地升级。
- 如果服务器是域控制器，请参阅[将域控制器升级到 WindowsServer 2012 R2 和 WindowsServer 2012](https://technet.microsoft.com/library/hh994618.aspx) 以获取重要信息。
- 不支持从 WindowsServer 2016 的预发布版本（预览版）升级。 为 WindowsServer 2016 执行干净安装。
- 不支持从“服务器核心安装”切换到“带桌面安装的服务器”的升级（反之亦然）。

如果在左列中看不到当前版本，则不支持到此版本的 WindowsServer 2016 的升级。

如果在右列中看到一个以上版本，则支持从相同的开始版本升级到**任一**版本。

|如果运行此版本：|可以升级到这些版本：|  
|-------------------|----------|  
|WindowsServer 2012 Standard|WindowsServer 2016 Standard 或 Datacenter|
|WindowsServer 2012 Datacenter|WindowsServer 2016 Datacenter|
|WindowsServer 2012 R2 Standard|WindowsServer 2016 Standard 或 Datacenter|
|WindowsServer 2012 R2 Datacenter|WindowsServer 2016 Datacenter|
|WindowsServer 2012 R2 Essentials|WindowsServer 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

有关迁移到 WindowsServer 2016 的许多其他选项（例如在批量许可版本、评估版本和其他版本之间进行许可转换），请参阅[升级选项](Supported-Upgrade-Paths.md)以了解详细信息。

### 迁移

  **迁移**意味着通过在一组不同的硬件或虚拟机上执行全新安装，然后将旧服务器的工作负荷转移到新服务器，从而将现有的操作系统移动到 WindowsServer 2016。 根据所安装服务器角色的不同，迁移过程可能会有相当大的不同，有关详细信息，请参阅 [WindowsServer Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795)（WindowsServer 安装、升级和迁移）。

迁移能力因服务器角色不同而异。 以下网格介绍了专门针对移动到 WindowsServer 2016 的服务器角色升级和迁移选项。 有关单个角色的迁移指南，请访问[在 WindowsServer 中迁移角色和功能](https://technet.microsoft.com/windowsserver/jj554790.aspx)。 有关安装和升级的详细信息，请参阅 [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795)（Windows Server 安装、升级和迁移）。

|服务器角色|是否可从 Windows Server 2012 R2 升级？|是否可从 Windows Server 2012 升级？|是否支持迁移？|是否无需停机就可完成迁移？|  
|-------------------|----------|--------------|--------------|----------|  
|Active Directory 证书服务| 是|    是|    是|    否|
|Active Directory 域服务|  是|    是|    是|    是|
|Active Directory 联合身份验证服务|  否| 否| 是|    否（需要将新节点添加到场中）|
|Active Directory 轻型目录服务|   是|    是|    是|    是|
|Active Directory 权限管理服务|   是|    是|    是|    否|
|故障转移群集|是，[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)过程包括节点暂停排出、逐出、升级到 Windows Server 2016 以及重新加入原始群集。 是，当群集删除服务器以进行升级，并随后再将服务器添加到不同的群集时。|而非服务器是群集的一部分时。 是，当群集删除服务器以进行升级，并随后再将服务器添加到不同的群集时。  |是|否，对于 Windows Server 2012 故障转移群集而言。 是，对于具有 Hyper-V VM 的 Windows Server 2012 R2 故障转移群集或运行横向扩展文件服务器角色的 Windows Server 2012 R2 故障转移群集。 请参阅[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)。|
|文件和存储服务| 是|    是|    因子功能而不同|  否|
|打印和传真服务|    否| 否| 是 (Printbrm.exe)| 否|
|远程桌面服务|   是，对于所有子角色，但不支持混合模式场|   是，对于所有子角色，但不支持混合模式场|   是|    否|
|Web 服务器 (IIS)|  是|    是|    是|    否|
|Windows Server Essentials 体验|  是|    N/A - 新功能|  是|    否|
|Windows Server Update Services|    是|    是|    是|    否|
|工作文件夹|  是|    是|    是|    是，使用[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)从 WS 2012 R2 群集进行操作时。|

> [!IMPORTANT]  
> 完成安装后，如果已安装所需的所有服务器角色和功能，则可以使用 Windows 更新或其他更新方法立即检查并安装 WindowsServer 2016 可用的更新。

---------------------------------------
如果需要其他安装选项，或者如果已完成安装并准备好部署特定的工作负荷，则可以[返回到 WindowsServer 2016 主页](Windows-Server-2016.md)。