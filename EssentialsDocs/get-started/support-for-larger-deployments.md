---
title: 对于更大部署的支持
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a99698519524c3b5050dc534d61921560522528c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433877"
---
# <a name="support-for-larger-deployments"></a>对于更大部署的支持

>适用于：Windows Server 2016 Essentials

> [!IMPORTANT]  
> 本主题中所述的功能仅适用于 Windows Server 2016 已启用，Essentials 体验角色而不是与 Windows Server 2016 Essentials SKU。


Windows Server Essentials 现在支持更大部署：

- 多个域
- 多个域控制器
- 指定指定的域控制器功能
- 支持最多 500 个用户和 500 台设备

## <a name="support-for-multiple-domains"></a>支持多个域

Windows server 2012 R2 Essentials 支持每个服务器是必需的只有一个域，Essentials 服务器必须是林的根。 仍然需要域和林时，现在可以在 Windows Server 2016 Standard 或数据中心，以支持多个域中部署 Windows Server 2016 Essentials 体验角色。

## <a name="support-for-multiple-domain-controllers"></a>支持多个域控制器

 Windows Server Essentials 2012 R2 阻止利用 Azure Active Directory，例如 Office 365，其中部署多个域控制器的任何服务。 原因是，本地域控制器与 Azure Active Directory 之间的帐户和密码同步可能会导致到帐户的凭据不同步的密码。已在 Windows Server 2016 Essentials 中删除此限制。

## <a name="ability-to-specify-a-designated-domain-controller"></a>指定指定的域控制器功能

现在可以选择指定的域控制器，这将改善的 Active Directory 域对象的检索时间，以及在域中的其他域控制器间协调的帐户更改同步。

默认的指定域控制器将运行 Windows Server Essentials Experience 服务器角色的同一台服务器。 如果该服务器是成员服务器，这意味着它不是域控制器，则测试基于指定的域控制器将自动确定的默认域中的域控制器都有最低网络延迟到运行的服务器Windows Server 体验服务器角色。 如果你想要手动更改哪个服务器是指定的域控制器，则可以在执行**设置**中**Windows Server Essentials 仪表板**，如下所示。

![屏幕截图显示了设置控制面板在前台和后台中的 Windows Server Essentials 仪表板。 当前选定的设置控件面板指定域控制器页。](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>对 500 个用户和 500 台设备的支持
-------------------------------------

Windows Server 2012 R2 Essentials 中受支持的用户和设备的最大数目是 25 到 50 个，分别。 随着 Windows Server Essentials Experience 服务器角色，该限制提高到 100 个用户和 200 台设备。

Windows Server 2016 Essentials 支持 500 个用户和 500 台设备。 为了使这可能是对提供程序框架的更新和对象列表控件使其缓存并快速呈现大型用户和设备对象列表。 此外，已添加搜索和筛选功能来快速查找用户或设备可能会寻找 （见图 5-2）。 您会发现功能搜索和筛选器的功能**Windows Server Essentials 仪表板**，**远程 Web 访问**，和 Windows 和 Windows Phone 应用商店**My Server**应用程序。

![屏幕截图显示如何使用 Windows Server Essentials 仪表板以搜索字符串"d5c。"的搜索功能 这一搜索结果包括两个文件和文件夹以及两个用户。](media/larger-deployments-2.PNG)

屏幕截图显示如何使用 Windows Server Essentials 仪表板以搜索字符串"d5c"的搜索功能。 这一搜索结果包括两个文件和文件夹以及两个用户。

> [!NOTE]  
> 虽然对于 Windows Server Essentials 服务器角色，支持客户端备份保持在 75 限制增加了支持的用户和设备限制。

<a name="see-also"></a>请参阅
--------
[Windows Server Essentials 入门](get-started.md)