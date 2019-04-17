---
title: 使用 Azure 备份备份你从 Windows Admin Center 的 Windows 服务器
description: 备份 Windows 服务器与 Azure 备份到的使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296875"
---
# 使用 Azure 备份备份你从 Windows Admin Center 的 Windows 服务器

>适用于： Windows Admin Center 预览版，Windows Admin Center

[了解有关 Azure 与 Windows Admin Center 集成的详细信息。](../plan/azure-integration-options.md)

Windows Admin Center 简化了 Windows Server 备份到 Azure 和保护你免受意外或恶意删除、 损坏和甚至勒索软件的过程。 若要自动设置，可以将 Windows Admin Center 网关连接到 Azure。

使用以下信息来配置为你的 Windows Server 备份和创建备份策略来备份你的服务器的卷和 Windows Admin Center 中的 Windows 系统状态。

## 什么是 Azure 备份和如何运作使用 Windows Admin Center？ 

**Azure 备份**是可用于备份 （或保护） 的基于 Azure 的服务和还原你在 Microsoft 云中的数据。 Azure 备份是可靠、 安全且成本竞争的基于云的解决方案中替换现有的本地或非现场备份解决方案。
[了解有关 Azure 备份详细信息](https://docs.microsoft.com/azure/backup/backup-overview)。

Azure 备份提供多个组件的下载和相应的计算机上部署服务器，或在云中。 组件或代理，部署取决于你想要保护。 所有 Azure 备份组件 （无论是否要保护数据的本地或在 Azure 中） 可以都用于备份到 Azure 中恢复服务保管库的数据。

Windows Admin Center 中的 Azure 备份的集成非常适合于备份卷和 Windows 系统状态的本地 Windows 物理或虚拟服务器。 这使得备份文件服务器、 域控制器和 IIS Web 服务器的全面机制。

Windows Admin Center 公开通过本机**备份**工具的 Azure 备份集成。 **备份**工具提供了设置，管理和监视体验，以快速开始备份你的服务器，执行常见的备份和还原操作并监视 Windows Server 的整体备份运行状况。

## 先决条件和计划

- 具有至少一个有效的订阅的 Azure 帐户
- 目标到所需的 Windows Server 备份必须具有 Internet 访问 Azure
- [在 Windows Admin Center 网关连接到 Azure](azure-integration.md)

若要开始备份你的 Windows Server 工作流，打开服务器连接，单击**备份**工具并按照如下所述的步骤。

## 设置 Azure 备份
当你单击 Azure 备份不尚未启用的服务器连接的**备份**工具时，你会看到**Azure 备份到欢迎**屏幕。 单击**设置 Azure 备份**按钮。 这将打开 Azure 备份安装向导。 下面列出向导备份你的服务器中，请按照步骤操作。

如果已配置 Azure 备份，单击**备份**工具将打开**备份仪表板**。 请参阅 （[管理和监视](#management-and-monitoring)） 部分，以发现操作和可以在仪表板中执行的任务。

### 步骤 1： 登录到 Microsoft Azure
登录到你的 Azure 帐户。 

> [!NOTE]
> 如果你已连接到 Azure 在 Windows Admin Center 网关，你应该会自动登录到 Azure。 你可以单击**注销**进一步登录以其他用户身份。

### 步骤 2： 设置 Azure 备份
选择适当的设置 Azure 备份，如下所述

 - **订阅 Id:** 想要使用备份到 Azure 的 Windows Server 的 Azure 订阅。 Azure 资源组相同的所有 Azure 资源，将选择的订阅中创建恢复服务保管库。
 - **保管库：** 将你的服务器的备份存储恢复服务保管库。 你可以选择从现有的存储库或 Windows Admin Center 将创建新的存储库。  
 - **资源组：** Azure 资源组是一个资源集合的容器。 创建或指定的资源组中包含的恢复服务保管库。 你可以选择从现有资源组或 Windows Admin Center 将创建一个新。
 - **位置：** 将在其中创建恢复服务保管库 Azure 区域。 建议选择最接近到 Windows Server 的 Azure 区域。

### 步骤 3： 选择备份项目和计划

- 选择你想要从你的服务器备份。 Windows Admin Center 让你选择的同时使您处于选中状态的数据的估计的大小为备份**卷**和**Windows 系统状态**的组合。

> [!NOTE]
> 第一次备份是所选的所有数据完整备份。 但是，后续备份的增量本质上，并且上次备份后将仅更改传输到数据。

- 从多个预设**备份计划**中选择为你系统状态和/或卷。

### 步骤 4： 输入加密密码

- 输入你选择 （最低 16 个字符） 的**加密密码**。  **Azure 备份**保护与用户配置和管理用户的加密密码备份数据。 所需的加密密码从 Azure 备份恢复数据。

> [!NOTE]
> 密码必须存储在安全的异地位置，如另一台服务器或[Azure 密钥保管库](https://docs.microsoft.com/azure/key-vault/quick-create-portal)。 Microsoft 不会存储密码和无法检索或重置密码，如果它是丢失或遗忘。

- 查看所有设置，然后单击**应用**

Windows Admin Center 然后执行以下操作

1. 如果它尚不存在，创建 Azure 资源组
2. 创建指定 Azure 恢复服务保管库
3. 安装并注册到保管库 Microsoft Azure 恢复服务代理
4. 创建依据所选选项的备份和保留计划，并将它们与 Windows Server 相关联。

## 管理和监视

一旦你已成功地设置 Azure 备份，你会看到**备份仪表板**，打开现有的服务器连接的备份工具时。 你可以从**备份仪表板**执行下列任务

- **访问 Azure 中的存储库：** 你可以单击**备份仪表板**以转到 Azure 中的保管库来执行一[组丰富的管理操作](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)的**概述**选项卡中的**恢复服务保管库**链接
- **执行临时备份：** 单击**现在备份**需要临时备份。 
- **监视器作业和配置警报通知：** 导航到仪表板监视持续的**作业**选项卡或超过作业和[配置警报通知](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts)的任何接收电子邮件失败作业或其他备份的相关的警报。
- **视图恢复点和恢复数据：** 单击仪表板查看恢复点并单击**恢复数据**的步骤从 Azure 恢复你的数据的**恢复点**选项卡上。