---
title: 适用于 WindowsServer 2016 的升级和转换选项
description: 介绍了 WindowsServer 2016 的所有受支持的升级路径。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 796ee001e20cff5587c5409f7571d223ff7900a8
ms.sourcegitcommit: ae0a69e9eb994c0b47a842ef4ca452f8c587a0f4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5858892"
---
# 适用于 WindowsServer 2016 的升级和转换选项

>适用于： Windows Server 2019，Windows Server 2016

本主题包括有关使用多种方法从以前的操作系统升级到 WindowsServer® 2016 的信息。

根据着手的操作系统以及采用的途径的不同，过渡到 WindowsServer 2016 的过程可能会有很大的不同。 我们会使用以下术语来区分不同的操作，进行全新的 WindowsServer 2016 部署时可能会涉及到其中的任何一项操作。

- **安装** 是一个基本概念，表示将新的操作系统装入你的硬件。 具体而言， **全新安装** 需要删除以前的操作系统。 有关安装 WindowsServer 2016 的信息，请参阅 [WindowsServer 2016 的系统要求和安装信息](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements--and-installation)。 有关安装其他版本的 WindowsServer 的信息，请参阅 [WindowsServer 安装与升级](https://technet.microsoft.com//windowsserver/dn527667)。

- 
  **迁移**表示通过转移到另一组硬件或虚拟机，从现有操作系统过渡到 WindowsServer 2016。 根据所安装服务器角色的不同，迁移过程可能会有相当大的不同，有关详细信息，请参阅 [WindowsServer Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795)（WindowsServer 安装、升级和迁移）。

- 
  **群集操作系统滚动升级**是 WindowsServer 2016 中的新增功能，管理员利用此功能可以将群集节点的操作系统从 WindowsServer 2012 R2 升级到 WindowsServer 2016，而无需停止 Hyper-v 或横向扩展文件服务器工作负荷。 利用此功能可以避免出现可能影响服务级别协议的故障时间。 [群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) 中对这一新增功能进行了更详细地讨论。

- **许可证转换**在某些操作系统发行版中，可以使用简单的命令和相应的许可证密钥，通过一个步骤将发行版的特定版本转换成同一发行版的另一个版本。 我们称之为“许可证转换”。 例如，如果正在运行 WindowsServer 2016 Standard，可以将其转换为 WindowsServer 2016 Datacenter。

- **升级**表示从现有的操作系统发行版过渡到更新的发行版，同时使用相同的硬件。 （这有时称为“就地”升级。）例如，如果服务器正在运行 WindowsServer 2012 或 WindowsServer 2012 R2，可以升级到 WindowsServer 2016。 可以从操作系统评估版升级到零售版，从早期的零售版升级到较新版本，在某些情况下，还可以从操作系统批量授权版升级到普通零售版。

> [!IMPORTANT]  
> 升级最适合用于虚拟机，其中进行成功升级不需要特定 OEM 硬件驱动程序。  

> [!IMPORTANT]  
> 对于 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，只能将使用“桌面体验”选项（非“服务器核心”选项）安装的 Windows Server 2016 **从评估版转换为零售版**。 从 14393.0.161119-1705.RS1_REFRESH 版本和更高版本开始，无论使用哪个安装选项，你都可以将评估版本转换为零售版本。

> [!IMPORTANT]  
> 如果服务器使用 NIC 组合，请在升级前禁用 NIC 组合，然后在升级完成后重新启用该组合。 请参阅 [NIC 组合概述](https://technet.microsoft.com/library/hh831648(v=ws.11).aspx) 了解详细信息。

## 将以前的 WindowsServer 零售版升级为 WindowsServer 2016

下表简单总结了**已经许可的**（即非评估版）Windows 操作系统可以升级到的 WindowsServer 2016 版本。

请注意受支持路径的下列一般指南：

- 不支持从 32 位到 64 位体系结构的升级。 所有版本的 WindowsServer 2016 都仅有 64 位。
- 不支持从一种语言到另一种语言的升级。
- 如果服务器是域控制器，请参阅[将域控制器升级到 WindowsServer 2012 R2 和 WindowsServer 2012](https://technet.microsoft.com/library/hh994618.aspx) 以获取重要信息。
- 不支持从 WindowsServer 2016 的预发布版本（预览版）升级。 为 WindowsServer 2016 执行干净安装。
- 不支持从“服务器核心安装”切换到“带桌面安装的服务器”的升级（反之亦然）。
- 不支持从以前的 WindowsServer 安装到 WindowsServer 的评估副本升级。 评估版本应作为干净安装进行安装。

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


## 升级时每服务器角色的考虑事项

甚至在从以前的零售版到 WindowsServer 2016 的受支持的升级路径中，已经安装的某些服务器角色可能要求更多的准备或操作才能在升级后使角色继续发挥作用。 对于要升级的每个服务器角色，请查阅具体的 TechNet 库主题，以了解可能需要执行的附加步骤的详细信息。

## 将当前评估版转换为当前零售版

可以将 WindowsServer 2016 Standard 的评估版转换为 WindowsServer 2016 Standard（零售版）或 Datacenter（零售版）。 同样，可以将 Windows Server 2016 Datacenter 的评估版转换为零售版。

> [!IMPORTANT]  
> 对于 14393.0.161119-1705.RS1_REFRESH 之前的 Windows Server 2016 版本，只能将使用“桌面体验”选项（非“服务器核心”选项）安装的 Windows Server 2016 从评估版转换为零售版。 从 14393.0.161119-1705.RS1_REFRESH 版本和更高版本开始，无论使用哪个安装选项，你都可以将评估版本转换为零售版本。

在尝试从评估版转换为零售版之前，请验证你的服务器是否确实在运行评估版。 为此，请执行下列两项操作之一：

- 在提升的命令提示符下运行 **slmgr.vbs /dlv**；评估版将在输出中包括“EVAL”。

- 从“开始”屏幕中，打开**控制面板**。 打开**系统和安全**，然后打开**系统**。 在**系统**页的 Windows 激活区域中查看 Windows 激活状态。 单击 Windows 激活中的**查看详细信息**以了解有关 Windows 激活状态的详细信息。

如果你已经激活 Windows，则桌面会显示评估期的剩余时间。

如果服务器运行的是零售版而不是评估版，请参阅本主题的“将以前的 WindowsServer 零售版升级到 WindowsServer 2016”部分，了解升级到 WindowsServer 2016 的说明。

有关 **WindowsServer 2016 Essentials**：可以通过在命令 **slmgr.vbs** 中输入零售、批量许可证或 OEM 密钥转换为完整零售版。

如果服务器正在运行 WindowsServer 2016 Standard 或 WindowsServer 2016 Datacenter 评估版，可以将其转换为零售版，如下所示：

1.  如果服务器是**域控制器**，无法将其转换为零售版。 在这种情况下，在运行零售版的服务器上安装另一个域控制器，并从运行评估版的域控制器上删除 AD DS。 有关详细信息，请参阅[将域控制器升级到 WindowsServer 2012 R2 和 WindowsServer 2012](https://technet.microsoft.com/library/hh994618.aspx)。
2.  阅读许可条款。
3.  在提升的命令提示符下使用命令 **DISM /online /Get-CurrentEdition**确定当前的版本名称。 记录版本 ID（版本名称的缩写格式）， 然后运行 **DISM /online /Set-Edition:<edition ID> /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**，同时提供版本 ID 和零售产品密钥。 服务器将重启两次。

对于 WindowsServer 2016 Standard 的评估版，还可以使用这一相同命令和相应的产品密钥通过一个步骤转换为 WindowsServer 2016 Datacenter 零售版。

> 提示 有关 Dism.exe 的详细信息，请参阅 [DISM 命令行选项](https://go.microsoft.com/fwlink/?LinkId=192466)。

## 将当前零售版转换为其他某个当前零售版

在安装 WindowsServer 2016 后，随时可以运行“Setup”来修复安装（有时称为“就地修复”）或在某些情况下，转换为另一版本。
可以运行“Setup”以在任何版本的 WindowsServer 2016 上执行“就地修复”；结果将是开始使用的同一版本。

对于 WindowsServer 2016 Standard，可以将系统转换为 WindowsServer 2016 Datacenter，如下所示：在提升的命令提示符下，使用命令 **DISM /online /Get-CurrentEdition** 确定当前的版本名称。 对于 Windows Server 2016 Standard 其将位于`ServerStandard`。 运行命令**DISM /online /Get-TargetEditions**若要获取可以升级到版本 ID。 记下此版本 ID、 版本名称的缩写格式。 然后运行**DISM /online /Set-Edition: \ < 版 ID\ > /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula**，提供版本 ID 的目标和其零售产品密钥。 服务器将重启两次。

## 将当前零售版转换为当前批量授权版

安装 WindowsServer 2016 后，随时可以自由地将其转换为零售版、批量许可证版或 OEM 版。 在执行这种转换期间，产品主版本 (Edition) 将保持相同。 如果从评估版开始，请首先转换为零售版，然后像在此处所述这样互换。

为此，请从提升的命令提示符处运行：**slmgr /ipk <key>**

其中，<key> 是相应的批量许可证、零售或 OEM 产品密钥。


 

