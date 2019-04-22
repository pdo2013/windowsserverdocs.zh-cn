---
title: 无线访问部署概述
description: 本主题是 Windows Server 2016 网络指南"部署基于密码的 802.1 X 身份验证无线访问"的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818918"
---
# <a name="wireless-access-deployment-overview"></a>无线访问部署概述

>适用于：Windows 服务器 （半年频道），Windows Server 2016

下图显示了所需组件的部署 802.1x 身份验证无线访问与 PEAP\-MS\-CHAP v2。  

![802.1x 部署基础结构概述](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>无线访问部署组件
以下基础结构，则需要为此无线访问部署：

### <a name="8021x-capable-wireless-access-points"></a>802.1x\-支持无线访问点
支持无线局域网的所需的网络基础结构服务就位后，可以开始无线访问点的位置的设计过程。 无线 AP 部署设计过程涉及以下步骤：

- 标识无线用户可以覆盖的区域。 确定区域覆盖范围，请务必确定你是否要提供构建，外部无线服务时，如果是这样，确定的专门位置外部的那些区域。

- 确定如何很多无线 Ap 部署以确保足够覆盖范围。

- 确定放置无线访问点的位置。

- 选择无线 Ap 的通道频率。

### <a name="active-directory-domain-services"></a>Active Directory 域服务
AD DS 的以下元素是无线访问部署所必需的。

#### <a name="users-and-computers"></a>用户和计算机

使用 Active Directory 用户和计算机管理单元\-中创建和管理用户帐户，并创建一个包含要向其授予无线访问每个域成员的无线安全组。

#### <a name="wireless-network-ieee-80211-policies"></a>无线网络\(IEEE 802.11\)策略

你可以使用无线网络\(IEEE 802.11\)策略扩展的组策略管理来配置当用户尝试访问网络时应用于无线计算机的策略。

在组策略管理编辑器中，当您右键\-单击**无线网络\(IEEE 802.11\)策略**，已创建的无线策略的类型的以下两个选项。

- **创建新的无线网络策略适用于 Windows Vista 和更高版本**

- **创建新的 Windows XP 策略**

>[!TIP]
>配置新的无线网络策略时，您可以通过更改名称和策略的说明。 如果更改策略的名称，更改都反映在**详细信息**窗格中的组策略管理编辑器和上的无线网络策略对话框的标题栏。 如何重命名你的策略，无论新 XP 无线策略，都将列出在组策略管理编辑器与**类型**显示**XP**。 列出的其他策略**类型**显示**Vista 和更高发行版本**。  

无线网络策略适用于 Windows Vista 和更高发行版本，可配置、 设置优先级别和管理多个无线配置文件。 无线配置文件是一系列连接和用于连接到特定的无线网络的安全设置。 当无线客户端计算机上更新组策略时，在无线网络策略中创建的配置文件是自动添加到你的无线网络策略应用到的无线客户端计算机上的配置。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允许连接到多个无线网络

如果必须在你的组织中的物理位置之间移动的无线客户端，如总部和分支机构，你可能想计算机连接到多个无线网络。 在这种情况下，可以配置包含每个网络的特定连接和安全设置的无线配置文件。

例如，假设你的公司有一个无线网络的企业总部，与服务集标识符\(SSID\) WlanCorp。

在分支机构还具有到还想要连接的无线网络。 分支机构都可以配置为 WlanBranch SSID。

在此方案中，可以配置每个网络和计算机或使用这两个公司总部的其他设备的配置文件和分支机构可以连接到的无线网络时它们的物理网络的覆盖区域范围内。

##### <a name="mixed-mode-wireless-networks"></a>混合\-模式的无线网络

或者，假设您的网络具有的无线计算机和设备支持不同的安全标准的混合。 某些较旧的计算机可能具有仅使用 WPA 的无线适配器\-企业，而较新的设备可以使用更强的 WPA2\-企业标准。

您可以创建两个不同的配置文件使用相同的 SSID 和几乎完全相同的连接和安全设置。

在一个配置文件，可以将无线身份验证设置为 WPA2\-企业使用 AES，和其他配置文件中可以指定 WPA\-TKIP 与企业。

这就通常所谓作为混合\-模式部署，使不同的类型和无线功能共享同一无线网络的计算机。

### <a name="network-policy-server-nps"></a>网络策略服务器\(NPS\)
NPS 可以创建和强制执行连接请求身份验证和授权的网络访问策略。

将 NPS 用作 RADIUS 服务器，配置网络访问服务器，如无线访问点、 为 NPS 中的 RADIUS 客户端。 您还可以配置 NPS 用于对访问客户端进行身份验证并授权其连接请求的网络策略。  

### <a name="wireless-client-computers"></a>无线客户端计算机
在本指南中，无线客户端计算机为计算机和其他设备，配备了 IEEE 802.11 无线网络适配器和运行 Windows 客户端或 Windows Server 操作系统。

#### <a name="server-computers-as-wireless-clients"></a>为无线客户端的服务器计算机

默认情况下，运行 Windows Server 的计算机上禁用 802.11 无线的功能。

若要启用运行服务器操作系统的计算机上的无线连接，必须安装并启用无线 LAN \(WLAN\)服务功能通过在服务器中使用 Windows PowerShell 或添加角色和功能向导管理器。

当你安装**无线 LAN 服务**功能，新的服务**WLAN 自动配置**中安装**Services**。 安装完成后，必须重新启动服务器。

重新启动服务器后，单击时，可以访问 WLAN 自动配置**启动**， **Windows 管理工具**，并**Services**。

安装和服务器重新启动，服务处于停止状态的启动类型 WLAN 自动配置后**自动**。 若要启动服务，双击**WLAN 自动配置**。 上**常规**选项卡上，单击**启动**，然后单击**确定**。

WLAN 自动配置服务枚举无线适配器，并管理无线连接和无线配置文件包含的配置服务器以连接到无线网络所需的设置。

无线访问部署的概述，请参阅[无线访问部署过程](c-wireless-access-deploy-process.md)。
