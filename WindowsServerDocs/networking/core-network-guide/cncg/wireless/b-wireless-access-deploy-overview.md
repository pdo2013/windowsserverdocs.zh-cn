---
title: 无线访问部署概述
description: 本主题是 Windows Server 2016 网络指南 "部署基于密码的 802.1 X 身份验证无线访问" 的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 93fb80c550771e4e7d8bc400d647b520b0c67fdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356087"
---
# <a name="wireless-access-deployment-overview"></a>无线访问部署概述

>适用于：Windows Server（半年频道）、Windows Server 2016

下图显示了使用 PEAP @ no__t-0MS @ no__t-1CHAP v2 部署 802.1 X 身份验证无线访问所需的组件。  

![802.1 x 部署基础结构概述](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>无线访问部署组件
此无线访问部署需要以下基础结构：

### <a name="8021x-capable-wireless-access-points"></a>802.1 x @ no__t-0capable 无线访问点
在所需的网络基础结构服务支持无线局域网后，可以开始对无线 Ap 位置的设计过程。 无线 AP 部署设计过程包括以下步骤：

- 确定无线用户的覆盖区域。 确定覆盖范围时，务必确定是否要在大楼外提供无线服务，如果是，则确定这些外部区域的具体位置。

- 确定要部署多少个无线 Ap 以确保充分的覆盖范围。

- 确定无线 Ap 的位置。

- 选择无线 Ap 的通道频率。

### <a name="active-directory-domain-services"></a>Active Directory 域服务
无线访问部署需要 AD DS 的以下元素。

#### <a name="users-and-computers"></a>用户和计算机

使用 "Active Directory 用户和计算机" snap @ no__t-0in 来创建和管理用户帐户，并创建包含要向其授予无线访问权限的每个域成员的无线安全组。

#### <a name="wireless-network-ieee-80211-policies"></a>无线网络 \(IEEE 802.11 @ no__t 策略

你可以使用组策略管理的无线网络 \(IEEE 802.11 @ no__t 策略扩展配置在尝试访问网络时应用到无线计算机的策略。

在组策略管理编辑器中，当你使用 @ no__t-0click**无线网络 \(IEEE 802.11 @ no__t-3 策略**时，你将为你创建的无线策略类型提供以下两个选项。

- **为 Windows Vista 和更高版本创建新的无线网络策略**

- **创建新的 Windows XP 策略**

>[!TIP]
>配置新的无线网络策略时，可以选择更改策略的名称和描述。 如果更改策略的名称，则更改将反映在组策略管理编辑器的**详细信息**窗格和无线网络策略对话框的标题栏中。 不管你如何重命名策略，新的 XP 无线策略将始终在组策略管理编辑器中列出，其**类型**显示为**XP**。 其他策略以显示**Vista 和更高版本**的**类型**列出。  

利用适用于 Windows Vista 和更高版本的无线网络策略，你可以配置、设置优先级和管理多个无线配置文件。 无线配置文件是连接和安全设置的集合，用于连接到特定的无线网络。 在无线客户端计算机上更新组策略时，你在无线网络策略中创建的配置文件会自动添加到无线客户端计算机上的配置，该配置将应用于无线网络策略。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允许连接到多个无线网络

如果你的无线客户端在组织中的物理位置（例如在总公司和分支机构之间）移动，则你可能希望计算机连接到多个无线网络。 在这种情况下，你可以配置包含每个网络的特定连接和安全设置的无线配置文件。

例如，假设你的公司有一个适用于主企业办公室的无线网络，其中包含一个服务集标识符 \(SSID @ no__t-1 WlanCorp。

分支机构还具有一个要连接到的无线网络。 分支机构将 SSID 配置为 WlanBranch。

在这种情况下，你可以为每个网络配置一个配置文件，在企业办公室和分支机构使用的计算机或其他设备在物理上位于网络覆盖区范围内时，可以连接到任一无线网络。

##### <a name="mixed-mode-wireless-networks"></a>Mixed @ no__t-0mode 无线网络

另外，假设你的网络混合了无线计算机和支持不同安全标准的设备。 一些较旧的计算机可能具有只能使用 WPA @ no__t-0Enterprise 的无线适配器，而更新的设备可以使用更强的 WPA2 @ no__t-1Enterprise standard。

你可以创建两个不同的配置文件，这些配置文件使用相同的 SSID，几乎完全相同的连接和安全设置。

在一个配置文件中，可以通过 AES 将无线身份验证设置为 "WPA2 @ no__t-0Enterprise"，在其他配置文件中，你可以指定带有 TKIP 的 WPA @ no__t-1Enterprise。

这通常称为混合的 @ no__t-0mode 部署，它允许不同类型和无线功能的计算机共享同一无线网络。

### <a name="network-policy-server-nps"></a>网络策略服务器 \(NPS @ no__t-1
NPS 使你能够创建和强制执行连接请求身份验证和授权的网络访问策略。

将 NPS 用作 RADIUS 服务器时，可以将网络访问服务器（如无线访问点）配置为 NPS 中的 RADIUS 客户端。 你还可以配置 NPS 用于对访问客户端进行身份验证并授权其连接请求的网络策略。  

### <a name="wireless-client-computers"></a>无线客户端计算机
出于本指南的目的，无线客户端计算机是配备了 IEEE 802.11 无线网络适配器以及运行 Windows 客户端或 Windows Server 操作系统的计算机和其他设备。

#### <a name="server-computers-as-wireless-clients"></a>作为无线客户端的服务器计算机

默认情况下，在运行 Windows Server 的计算机上禁用802.11 无线功能。

若要在运行服务器操作系统的计算机上启用无线连接，必须通过使用 Windows PowerShell 或服务器管理器中的 "添加角色和功能向导" 来安装和启用无线 LAN \(WLAN @ no__t 服务功能。

安装**无线 LAN 服务**功能时，会在**服务**中安装新的服务**WLAN 自动配置**。 安装完成后，必须重新启动服务器。

重新启动服务器后，单击 "**启动**"、" **Windows 管理工具**" 和 "**服务**" 即可访问 WLAN 自动配置。

安装并重新启动服务器后，WLAN 自动配置服务处于停止状态，启动类型为 "**自动**"。 若要启动该服务，请双击 " **WLAN 自动配置**"。 在 "**常规**" 选项卡上，单击 "**开始**"，然后单击 **"确定"** 。

WLAN 自动配置服务枚举无线连接并管理无线连接和无线配置文件，这些配置文件包含配置服务器以连接到无线网络所需的设置。

有关无线访问部署的概述，请参阅[无线访问部署过程](c-wireless-access-deploy-process.md)。
