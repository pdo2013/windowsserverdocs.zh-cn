---
title: "对于更大部署的支持"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>对于更大部署的支持

>适用于：Windows Server 2016 软件包

> [!IMPORTANT]  
> 本主题中所述的功能仅适用于 Windows Server 2016 Essentials 体验角色启用，而不是 Windows Server 2016 Essentials SKU。


Windows Server 软件包现在支持与变得更大部署：

- 多个域
- 多个域控制器
- 若要指定指定的域控制器的功能
- 对 500 用户和 500 设备的支持

##<a name="support-for-multiple-domains"></a>多个域的支持

Windows server 2012 R2 Essentials 支持的每个服务器，需要时，只有一个域，该软件包服务器必须是森林根。 虽然域和林仍然需要，现在可以在 Windows Server 2016 标准或 Datacenter 支持多个域部署 Windows Server 2016 Essentials 体验角色。

## <a name="support-for-multiple-domain-controllers"></a>多个域控制器的支持

 Windows Server 软件包 2012 R2 将阻止任何充分利用 Azure Active Directory，如 Office 365、多个域控制器部署的位置的服务。 原因是，在本地域控制器和 Azure Active Directory 之间的帐户和密码同步可能会导致帐户凭据与未同步的密码。 在 Windows Server 2016 Essentials 已删除此项限制。

##<a name="ability-to-specify-a-designated-domain-controller"></a>若要指定指定的域控制器的功能

你现在可以选择指定的域控制器，这将提高检索时间对于域的 Active Directory 对象，以及跨域中的其他域控制器坐标更改帐户的同步。

指定域控制器默认将运行 Windows Server Essentials 体验服务器角色同一服务器。 如果该服务器成员服务器，意味着它不会域控制器，然后指定的域控制器自动取决于基于测试在域中的哪个域控制器上的默认有运行 Windows Server 体验服务器角色服务器最低网络延迟。 如果你想要手动更改哪个服务器是指定的域控制器，你可以在执行**设置**中**Windows Server Essentials 仪表板**如下所示。

![屏幕截图显示了设置来控制面板在前台和后台中的 Windows Server Essentials 仪表板。 当前未选择设置控制面板命名域控制器页面。](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>对 500 用户和 500 设备的支持
-------------------------------------

受支持的用户和 Windows Server 2012 R2 Essentials 中的设备的最大数是 25 50，分别。 引入了 Windows Server Essentials 体验服务器角色，该限制增加到 100 用户和 200 设备。

Windows Server 2016 软件包支持 500 用户和 500 设备。 使此可能是对的更新的提供商框架和对象列表控制，因此它们缓存并快速地呈现大型用户和设备对象的列表。 此外，添加的搜索和筛选器功能快速查找可能寻找（请参阅图 5-2）设备的用户。 你将找到中的搜索和筛选器功能**Windows Server Essentials 仪表板**，**远程访问 Web**，Windows 和 Windows Phone 应用商店和**我服务器**应用程序。

![屏幕截图显示了使用搜索功能的 Windows Server Essentials 仪表板中搜索字符串"d5c。" 此搜索的结果包括两个文件和文件夹以及两位用户。](media/larger-deployments-2.PNG)

屏幕截图显示了使用搜索功能的 Windows Server Essentials 仪表板中搜索字符串"d5c"。 此搜索的结果包括两个文件和文件夹以及两位用户。

> [!NOTE]  
> 而对于 Windows Server Essentials 服务器角色，受支持的客户端备份保持为 75 限制增加了支持的用户和设备的限制。

<a name="see-also"></a>请参阅
--------
[Windows Server Essentials 入门](get-started.md)