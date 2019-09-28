---
title: 通过 Azure 备份从 Windows 管理中心备份 Windows Server
description: 使用 Windows 管理中心（Project Honolulu）通过 Azure 备份来备份 Windows Server
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 77171e638fa1eb8c612bdc168868d8286ed23bb6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357402"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>通过 Azure 备份从 Windows 管理中心备份 Windows Server

>适用于：Windows 管理中心预览, Windows 管理中心

[了解有关 Azure 与 Windows 管理中心的集成的详细信息。](../plan/azure-integration-options.md)

Windows 管理中心简化了将 Windows Server 备份到 Azure 并防止意外或恶意删除、损坏甚至是勒索软件的过程。 若要自动设置，可以将 Windows Admin Center 网关连接到 Azure。

使用以下信息来为你的 Windows Server 配置备份，并创建用于从 Windows 管理中心备份服务器卷和 Windows 系统状态的备份策略。

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>什么是 Azure 备份，它如何与 Windows 管理中心配合使用？ 

**Azure 备份**是基于 Azure 的服务，可用于备份（或保护）和还原 Microsoft 云中的数据。 Azure 备份将现有的本地或异地备份解决方案替换为可靠、安全且具有成本优势的基于云的解决方案。
[了解有关 Azure 备份的详细信息](https://docs.microsoft.com/azure/backup/backup-overview)。

Azure 备份提供多个组件，你可以在相应的计算机、服务器或云中下载并部署这些组件。 你部署的组件或代理取决于你想要保护的内容。 无论是保护本地数据还是 Azure 中的数据，所有 Azure 备份组件均可用于将数据备份到 Azure 中的恢复服务保管库。

Windows 管理中心中的 Azure 备份集成非常适合用于备份卷和 Windows 系统状态本地 Windows 物理或虚拟服务器。 这是一种用于备份文件服务器、域控制器和 IIS Web 服务器的综合机制。

Windows 管理中心通过本机**备份**工具公开 Azure 备份集成。 **备份**工具提供了安装、管理和监视体验，可以快速开始备份服务器、执行常见备份和还原操作以及监视 Windows 服务器的总体备份运行状况。

## <a name="prerequisites-and-planning"></a>先决条件和计划

- 至少具有一个活动订阅的 Azure 帐户
- 要备份的目标 Windows 服务器必须能够通过 Internet 访问 Azure
- [将 Windows 管理中心网关连接到 Azure](azure-integration.md)

若要启动工作流以备份 Windows Server，请打开服务器连接，单击 "**备份**" 工具，然后执行下面所述的步骤。

## <a name="setup-azure-backup"></a>设置 Azure 备份
单击尚未启用 Azure 备份的服务器连接的**备份**工具时，会显示 "**欢迎使用 azure 备份**" 屏幕。 单击 "**设置 Azure 备份**" 按钮。 这将打开 "Azure 备份安装向导"。 按照向导中列出的步骤来备份服务器。

如果 Azure 备份已配置，单击**备份**工具将打开 "**备份" 仪表板**。 请参阅（[管理和监视](#management-and-monitoring)）部分，了解可从仪表板执行的操作和任务。

### <a name="step-1-login-to-microsoft-azure"></a>第 1 步：登录到 Microsoft Azure
登录到 Azure 帐户。 

> [!NOTE]
> 如果已将 Windows 管理中心网关连接到 Azure，则应自动登录到 Azure。 你**可以单击 "注销"** 以其他用户身份登录。

### <a name="step-2-set-up-azure-backup"></a>步骤 2：设置 Azure 备份
按如下所述为 Azure 备份选择适当的设置

 - **订阅 Id：** 要使用的 Azure 订阅将 Windows Server 备份到 Azure。 所有 Azure 资产（如 Azure 资源组）都将在所选订阅中创建恢复服务保管库。
 - **保管库**恢复服务保管库，将在其中存储服务器的备份。 可以从现有保管库中选择，也可以从 Windows 管理中心创建新的保管库。  
 - **资源组：** Azure 资源组是资源集合的容器。 恢复服务保管库在指定的资源组中创建或包含。 你可以从现有的资源组中进行选择，或者 Windows 管理中心会创建一个新的资源组。
 - **位置：** 将在其中创建恢复服务保管库的 Azure 区域。 建议选择最靠近 Windows Server 的 Azure 区域。

### <a name="step-3-select-backup-items-and-schedule"></a>步骤 3:选择备份项目和计划

- 选择要从服务器备份的内容。 使用 windows 管理中心，你可以从**卷**和**Windows 系统状态**的组合中进行选择，同时为你提供要备份的数据的估计大小。

> [!NOTE]
> 第一个备份是所有选定数据的完整备份。 但是，后续备份在本质上是递增的，并且只会传输自上次备份以来对数据所做的更改。

- 对于系统状态和/或卷，从多个预设**备份计划**中进行选择。

### <a name="step-4-enter-encryption-passphrase"></a>步骤 4：输入加密密码

- 输入所选的**加密通行短语**（最少16个字符）。  **Azure 备份**使用用户配置的和用户管理的加密通行短语来保护你的备份数据。 恢复 Azure 备份中的数据需要加密密码。

> [!NOTE]
> 密码必须存储在安全的场外位置，如其他服务器或[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)。 如果丢失或忘记了通行短语，Microsoft 将不会存储该通行短语，也无法检索或重置密码。

- 查看所有设置，并单击 "**应用**"

然后，Windows 管理中心会执行以下操作

1. 如果 Azure 资源组尚不存在，则创建它
2. 创建指定的 Azure 恢复服务保管库
3. 安装 Microsoft Azure 恢复服务代理并将其注册到保管库
4. 根据选定的选项创建备份和保留计划，并将其与 Windows Server 关联。

## <a name="management-and-monitoring"></a>管理和监视

成功设置 Azure 备份后，打开现有服务器连接的备份工具时，将显示 "**备份" 仪表板**。 你可以从**备份仪表板**执行以下任务

- **访问 Azure 中的保管库：** 你可以单击 "**备份" 仪表板**的 "**概述**" 选项卡中的 "**恢复服务保管库**" 链接，以在 Azure 中执行一[组丰富的管理操作](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **执行即席备份：** 单击 "**立即备份**" 以进行即席备份。 
- **监视作业并配置警报通知：** 导航到仪表板的 "**作业**" 选项卡，监视正在进行的或过去的作业，并[配置警报通知](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts)以便接收任何失败的作业或其他与备份相关的警报的电子邮件。
- **查看恢复点和恢复数据：** 单击仪表板的 "**恢复点**" 选项卡以查看恢复点，然后单击 "**恢复数据**" 以获取从 Azure 恢复数据的步骤。