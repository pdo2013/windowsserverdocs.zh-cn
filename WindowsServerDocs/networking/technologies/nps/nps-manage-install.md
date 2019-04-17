---
title: 安装网络策略服务器
description: 你可以使用本主题安装在 Windows Server 2016 中使用 Windows PowerShell 或添加角色和功能向导网络策略 Server (NPS)
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>安装网络策略服务器

你可以使用本主题安装网络策略 Server (NPS)，使用 Windows PowerShell 或添加角色和功能向导。 NPS 是网络策略和服务的访问权限服务器角色的角色服务。

> [!NOTE]
> 默认情况下，NPS RADIUS 流量端口 1812年 1813年、 1645年，1646年所有已安装的网络适配器侦听。 如果高级安全 Windows 防火墙处于启用状态，当你安装 NPS，在安装过程中的 Internet 协议版本 6 \(IPv6\) 和 IPv4 交通自动创建防火墙例外，以便这些端口。 如果你的网络访问权限服务器配置为通过以外这些默认值端口发送 RADIUS 通信，删除 NPS 安装期间创建高级安全 Windows 防火墙在例外，并创建例外，请使用 RADIUS 交通端口。

**管理凭据**

若要完成此过程，你必须**域管理员**组。

## <a name="to-install-nps-by-using-windows-powershell"></a>若要使用的 Windows PowerShell 安装 NPS

若要执行此过程，方法是使用 Windows PowerShell，以管理员身份运行的 Windows PowerShell 键入以下命令，，然后按 ENTER。

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>若要安装 NPS 使用服务器管理器

1.  在 NPS1，在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 添加角色和功能向导将打开。

2.  在**开始之前**，单击**下一步**。

    > [!NOTE]
    > **开始之前**添加角色和功能向导中的页面未显示如果你之前已选择**默认情况下跳过此页**运行添加角色并功能向导。

3.  在**选择安装类型**，确保**角色基于或功能的安装**选中，则，然后单击**下一步**。

4.  在**选择目标服务器**，确保**从服务器池选择服务器**选择。 在**服务器池**，确保在本地计算机处于选中状态。 单击**下一步**。

5.  在**选择服务器角色**中**角色**、 选择**网络策略和访问服务**。 对话框中打开要求将应添加所需的网络策略和服务的访问权限的功能。 单击**添加功能**，然后单击**接下来**

6.  在**选择功能**，单击**下一步**，并在**网络策略和访问服务**，检查信息，提供了，然后单击**下一步**。

7.  在**选择角色服务**，单击**网络策略服务器**。  在**添加所需的网络策略服务器的功能**，单击**添加功能**。 单击**下一步**。

8.  在**确认安装选择**，单击**必要时自动重新启动目标服务器**。 当提示你确认选择此选项时，请单击**是**，然后单击**安装**。 安装进度页面显示在安装过程中的状态。 该过程完成后，该消息"上的已成功安装*ComputerName*"显示时，其中*ComputerName*是你已安装在其网络策略服务器计算机的名称。 单击**关闭**。

有关详细信息，请参阅[管理 NPS 服务器](nps-manage-servers.md)。