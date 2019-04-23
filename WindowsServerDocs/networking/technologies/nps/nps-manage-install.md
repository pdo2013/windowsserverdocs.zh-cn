---
title: 安装网络策略服务器
description: 可以使用本主题通过在 Windows Server 2016 中使用 Windows PowerShell 或添加角色和功能向导安装网络策略服务器 (NPS)
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831188"
---
# <a name="install-network-policy-server"></a>安装网络策略服务器

本主题可用于使用 Windows PowerShell 或添加角色和功能向导安装网络策略服务器 (NPS)。 NPS 是网络策略和访问服务服务器角色的一个角色服务。

> [!NOTE]
> 默认情况下，NPS 在所有已安装的网络适配器上侦听端口 1812、1813、1645 和 1646 上的 RADIUS 流量。 如果安装 NPS 时启用了高级安全 Windows 防火墙，这两个 Internet 协议版本 6 的安装过程中自动创建的这些端口的防火墙例外\(IPv6\)和 IPv4 流量。 如果您的网络访问服务器配置为这些默认值以外的端口上发送 RADIUS 流量，删除 NPS 安装期间在具有高级安全性的 Windows 防火墙中创建的例外和您使用的端口创建例外RADIUS 流量。

**管理凭据**

若要完成此过程，你必须是**Domain Admins**组的成员。

## <a name="to-install-nps-by-using-windows-powershell"></a>若要使用 Windows PowerShell 安装 NPS

若要使用 Windows PowerShell，以管理员身份，运行 Windows PowerShell 执行此过程键入以下命令，然后按 ENTER。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>若要使用服务器管理器安装 NPS

1.  在 NPS1 上，在 “服务器管理器”中，单击 **“管理”**，然后单击 **“添加角色和功能”**。 将打开“添加角色和功能向导”。

2.  在“开始之前”中单击“下一步”。

    > [!NOTE]
    > 如果以前运行“添加角色和功能向导”时选择了 **“默认跳过此页”**，则“添加角色和功能向导”的 **“开始之前”** 页不会显示。

3.  在 **“选择安装类型”** 中，确保选中 **“基于角色或基于功能的安装”**，然后单击 **“下一步”**。

4.  在 **“选择目标服务器”** 中，确保选中 **“从服务器池中选择一个服务器”**。 在 **“服务器池”** 中，确保选中了本地计算机。 单击“下一步” 。

5.  在中**选择服务器角色**，在**角色**，选择**网络策略和 Access Services**。 会打开一个对话框，要求将应添加所需的网络策略和访问服务的功能。 单击**添加功能**，然后单击**下一步**

6.  在**选择功能**中，单击 **“下一步”**，然后在 **“网络策略和访问服务”** 中查看提供的信息，然后单击 **“下一步”**。

7.  在**选择角色服务**中，单击 **“网络策略服务器”**。  在**添加“网络策略服务器”需要的功能**中，单击 **“添加功能”**。 单击“下一步” 。

8.  在 **“确认安装选择”** 中，单击 **“如果需要，自动重启目标服务器”**。 当提示你确认这一选择时，单击 **“是”**，然后单击 **“安装”**。 在安装进程期间，安装进度页显示状态。 当进程完毕时，消息"上的安装已成功*ComputerName*"会显示，其中*ComputerName*是在其安装网络策略服务器的计算机的名称。 单击 **“关闭”**。

有关详细信息，请参阅[管理 NPSs](nps-manage-servers.md)。