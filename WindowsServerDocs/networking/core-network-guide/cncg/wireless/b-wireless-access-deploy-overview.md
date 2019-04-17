---
title: 无线接入部署概述
description: 本主题介绍 Windows Server 2016 网络指南"部署密码基于 802.1 X 身份验证无线的访问"部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>无线接入部署概述

>适用于：Windows Server（半年通道），Windows Server 2016

下图显示所需部署 802.1 X 的组件进行身份验证 PEAP\ MS\ CHAP v2 与无线接入。  

![802.1 x 部署基础结构概述](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>无线接入部署组件
以下基础结构是此无线接入部署所必需的方法：

### <a name="8021x-capable-wireless-access-points"></a>802.1X\ 的支持的无线接入点
支持无线本地网络所需的网络的基础结构服务到位后，你可以开始的无线接入点位置的设计过程。 无线接入点部署设计过程涉及这些步骤：

- 确定哪些的无线用户的覆盖率。 确定方面的覆盖范围，请务必确定是否想要提供之外生成，无线服务时，如果是这样，确定专门这些外部领域所在。

- 确定多少的无线接入点部署以确保足够覆盖范围。

- 确定放置无线接入点的位置。

- 选择无线接入点的信道频率。

### <a name="active-directory-domain-services"></a>Active Directory 域服务
广告 DS 的以下元素是必需的无线接入部署。

#### <a name="users-and-computers"></a>用户和计算机

请使用创建和管理用户帐户和要创建无线安全组，其中包括你希望授予无线的访问权限的每个成员域的 Active Directory 用户和 snap\ 中的计算机。

#### <a name="wireless-network-ieee-80211-policies"></a>无线网络 \ (IEEE 802.11\) 策略

你可以使用无线网络 \ (IEEE 802.11\) 策略扩展的组策略管理配置适用于无线计算机在尝试访问该网络时的策略。

在组策略管理编辑器中，当你 right\ 单击**无线网络 \ (IEEE 802.11\) 策略**，有以下两种类型的无线你创建的策略选项。

- **创建新的无线网络策略的 Windows Vista 和更高版本**

- **创建新的 Windows XP 策略**

>[!TIP]
>在新的无线网络策略配置时，你可以选择要更改的名称和策略的说明。 如果你改变策略的名称，在反映更改**详细信息**窗格中的组策略管理编辑器中和标题栏上的无线网络策略对话框。 如何重命名你的策略，无论新 XP 无线策略始终会列出在组策略管理编辑器与**类型**显示**XP**。 与列出了其他策略**类型**显示**Vista 和以后版本**。  

无线网络策略的 Windows Vista 和更高版本的发布，可配置、 优先，以及管理多个无线配置文件。 无线的个人资料是一组连接和用于连接到无线网络特定的安全设置。 组策略更新你的无线的客户端计算机上，当无线网络策略在创建配置文件将自动添加到无线网络策略应用到无线的客户端计算机上的配置。

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>允许连接到多个无线网络

如果你有无线客户端移动在你的组织中的物理位置，如主要 office 之间的分支机构，你可能想计算机连接到无线的多个网络。 在此情况下，您可以配置无线个人资料包含的特定的连接和安全设置为每个网络。

例如，假设你的公司所拥有的主公司 office，服务设置标识符与无线网络 \(SSID\) WlanCorp。

你的分支机构还有这你还想要连接到无线网络。 分支机构已配置为 WlanBranch SSID。

在此情况下，你可以为每个网络，和计算机或企业办公室在使用其他设备配置，并且它们的物理范围的网络的覆盖范围内时，分支机构可以连接到无线网络任一。

##### <a name="mixed-mode-wireless-networks"></a>Mixed\ 模式无线网络

或者，假设你的网络已无线计算机和支持不同的安全标准设备的组合。 假设某些较旧的计算机有只能使用 WPA\-企业版，而较新的设备可以使用强 WPA2\ 企业标准的无线适配器。

你可以创建两个使用相同的 SSID 和几乎相同的连接和安全设置的不同配置文件。

在一配置文件，你可以设置无线身份验证到与 AES，WPA2\ 企业版和其他个人资料中可以使用 TKIP 指定 WPA\ 企业。

这通常称为 mixed\ 模式部署，并允许其他类型和无线共享相同的无线网络的功能的计算机。

### <a name="network-policy-server-nps"></a>网络策略服务器 \(NPS\)
NPS 可以让你创建和履行网络连接请求身份验证和授权的访问策略。

当你 NPS RADIUS 服务器用作，您将配置为在 NPS RADIUS 客户端网络的访问权限服务器，如无线接入点。 您还配置了网络策略 NPS 用来验证访问客户端和授权它们连接的请求。  

### <a name="wireless-client-computers"></a>无线客户端计算机
本指南中，对于无线客户端是计算机和配备 IEEE 802.11 无线网络适配器和所运行的 Windows 客户端或 Windows Server 操作系统的其他设备。

#### <a name="server-computers-as-wireless-clients"></a>作为无线客户端服务器计算机

默认情况下，计算机上运行的 Windows Server，禁用 802.11 无线的功能。

若要启用计算机运行的服务器操作系统上的无线连接，你必须安装并启用无线 LAN \(WLAN\) 服务功能，通过使用 Windows PowerShell 或添加角色和功能向导中服务器管理器。

当您安装**无线 LAN 服务**功能，新的服务**WLAN 自动配置**安装在**服务**。 安装完成后，你必须重新启动的服务器。

当你点击服务器重启后，你可以访问 WLAN 自动配置**开始**， **Windows 管理工具**，并**服务**。

安装并服务器重新启动，请服务处于停止使用启动种状态 WLAN 自动配置后**自动**。 若要启动该服务，请双击**WLAN 自动配置**。 在**常规**选项卡上，单击**开始**，然后单击**确定**。

WLAN 自动配置服务枚举无线适配器，并管理无线连接并包含配置服务器以连接到无线网络所需的设置无线配置文件。

有关的无线接入部署概述，请参阅[无线访问部署过程](c-wireless-access-deploy-process.md)。
