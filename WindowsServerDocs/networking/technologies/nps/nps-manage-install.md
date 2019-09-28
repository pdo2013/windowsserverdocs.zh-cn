---
title: 安装网络策略服务器
description: 你可以使用本主题通过使用 windows PowerShell 或 Windows Server 2016 中的添加角色和功能向导来安装网络策略服务器（NPS）
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25b8586d370865dd3ae4393c2536c348a5d0810f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396193"
---
# <a name="install-network-policy-server"></a>安装网络策略服务器

您可以使用本主题通过使用 Windows PowerShell 或 "添加角色和功能向导" 来安装网络策略服务器（NPS）。 NPS 是网络策略和访问服务服务器角色的一个角色服务。

> [!NOTE]
> 默认情况下，NPS 在所有已安装的网络适配器上侦听端口 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果在安装 NPS 时启用了具有高级安全性的 Windows 防火墙，则在安装过程中会自动为这些端口创建防火墙例外，同时为 Internet 协议版本 6 @no__t 0IPv6 @ no__t 和 IPv4 通信。 如果网络访问服务器被配置为通过这些默认端口之外的端口发送 RADIUS 流量，请在安装 NPS 期间删除在 "高级安全 Windows 防火墙" 中创建的例外，并为用于的端口创建例外。RADIUS 流量。

**管理凭据**

若要完成此过程，你必须是**Domain Admins**组的成员。

## <a name="to-install-nps-by-using-windows-powershell"></a>使用 Windows PowerShell 安装 NPS

若要使用 Windows PowerShell 执行此过程，请以管理员身份运行 Windows PowerShell，键入以下命令，然后按 ENTER。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>使用服务器管理器安装 NPS

1.  在 NPS1 上，在 “服务器管理器”中，单击 **“管理”** ，然后单击 **“添加角色和功能”** 。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”** ，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”** ，然后单击 **“下一步”** 。

4.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”** 。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步”。

5.  在 "**选择服务器角色**" 的 "**角色**" 中，选择 "**网络策略和访问服务**"。 此时将打开一个对话框，询问是否应添加网络策略和访问服务所需的功能。 单击 "**添加功能**"，然后单击 "**下一步**"

6.  在**选择功能**中，单击 **“下一步”** ，然后在 **“网络策略和访问服务”** 中查看提供的信息，然后单击 **“下一步”** 。

7.  在**选择角色服务**中，单击 **“网络策略服务器”** 。  在**添加“网络策略服务器”需要的功能**中，单击 **“添加功能”** 。 单击“下一步”。

8.  在 **“确认安装选择”** 中，单击 **“如果需要，自动重启目标服务器”** 。 当提示你确认这一选择时，单击 **“是”** ，然后单击 **“安装”** 。 在安装进程期间，安装进度页显示状态。 此过程完成后，将显示消息 " *computername*上安装成功"，其中*ComputerName*是安装了网络策略服务器的计算机的名称。 单击 **“关闭”** 。

有关详细信息，请参阅[管理 NPSs](nps-manage-servers.md)。