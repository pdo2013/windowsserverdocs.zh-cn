---
title: MultiPoint 服务用户帐户
description: 了解 MultiPoint Services 中的用户帐户，尤其是在不同方案中使用的类型
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 67aa84244aa9ebfcd8375bd82f9d6ae431fc5eac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871425"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>示例方案：MultiPoint 服务用户帐户
要实现为 MultiPoint 服务环境选择的用户帐户方案，需要执行哪些操作？ 下表描述了为在独立的 MultiPoint 计算机上或在工作组或 Active Directory 域中的网络服务器上配置用户帐户以及为共享或个人用户帐户准备工作站所要执行的每个任务。 选择适用于您的环境的方案。 然后，按照表中的链接来完成每个必需的配置任务。  
  
> [!NOTE]  
> 如果尚未决定如何设置用户帐户，请参阅为[MultiPoint 服务环境规划用户帐户](Plan-user-accounts-for-your-MultiPoint-services-environment.md)，以了解有关每个选项如何影响用户的详细信息。  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>独立环境中的单个 MultiPoint 服务计算机（无网络）  
  
|||  
|-|-|  
|**我的用户不需要登录。** 可以向任何用户提供这些工作站。 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.创建单个本地用户帐户（有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />2.[允许一个帐户具有多个会话](Allow-one-account-to-have-multiple-sessions.md)<br />3.[为自动登录配置工作站](Configure-stations-for-automatic-logon.md)|  
|**我的用户可以共享相同的用户登录名。** 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.创建单个本地用户帐户（有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />2.[允许一个帐户具有多个会话](Allow-one-account-to-have-multiple-sessions.md)|  
|**我的用户必须拥有自己的 Windows 桌面体验。**|为每个用户创建一个本地用户帐户（有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>网络上的多个 MultiPoint 服务计算机，但没有域  
  
|||  
|-|-|  
|**我的用户不需要登录。** 可以向任何用户提供这些工作站。 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.在每个服务器上创建一个本地用户帐户。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />2.允许一个帐户在每个服务器上[都有多个会话](Allow-one-account-to-have-multiple-sessions.md)<br />3.为每个服务器上[的自动登录配置工作站](Configure-stations-for-automatic-logon.md)|  
|**我的用户可以共享相同的用户登录名。** 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.在每个服务器上创建一个本地用户帐户。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />2.允许一个帐户在每个服务器上[都有多个会话](Allow-one-account-to-have-multiple-sessions.md)。|  
|**我的用户必须拥有自己的 Windows 桌面体验。**<br /><br />-   **选项 A** -我的用户将始终使用连接到同一 MultiPoint 服务计算机的本地工作站。<br />-   **选项 B** -我的用户将在多个 MultiPoint 服务计算机上使用本地工作站。<br />-   **选项 C** -我的用户将使用 LAN 上的远程客户端。|-   **选项 A** -在每个服务器上为该服务器的用户创建单个本地用户帐户。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />-   **选项 B** -为每个服务器上的每个用户创建本地用户帐户。 **注意：** 这意味着每个用户在每个服务器上都有一个配置文件。 换句话说，如果用户在登录到服务器 A 的工作站时在 "我的文档" 中保存了文件，则在登录到服务器 B 的工作站时，它们将不会看到该文件。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br />-   **选项 C** -将每个用户分配给特定的 MultiPoint 服务计算机。 为每个服务器上已分配的用户创建本地用户帐户。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>域网络环境中的一个或多个 MultiPoint 服务计算机  
  
|||  
|-|-|  
|**我的用户不需要登录。** 可以向任何用户提供这些工作站。 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.创建一个用于登录到服务器的域帐户。<br />2.允许一个帐户在每个服务器上[都有多个会话](Allow-one-account-to-have-multiple-sessions.md)。<br />3.[将工作站配置为](Configure-stations-for-automatic-logon.md)在每台服务器上自动登录。|  
|**我的用户可以共享相同的用户登录名。** 它们不需要单独的 Windows 桌面体验，其中包含用于存储数据或个性化桌面的专用文件夹。|1.为组或每个用户创建域帐户。<br />2.允许一个帐户在每个服务器上[都有多个会话](Allow-one-account-to-have-multiple-sessions.md)。|  
|**我的用户必须拥有自己的 Windows 桌面体验。**<br /><br />-   **选项 A** -具有域帐户的任何用户都可以使用 MultiPoint 服务计算机。<br />-   **选项 B** -我想限制哪些域帐户可以访问服务器。|-   **选项 A** -无需设置。 默认情况下，所有域用户都有权访问网络上的任何 MultiPoint 服务计算机。<br />-   **选项 B** -限制域用户帐户对 MultiPoint 服务计算机的访问。 有关说明，请参阅[限制用户对服务器的访问权限](limit-users--access-to-the-server-in-multipoint-services.md)。|  
|**我想使用本地用户帐户，并将其与域帐户分开管理。** 例如，你希望某人管理 MultiPoint 服务，而不是域，或者你不希望向所有 MultiPoint 服务用户提供域帐户。|在每个服务器上创建一个或多个本地用户帐户。 （有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。）<br /><br />**注意：** 这意味着每个用户帐户在每个服务器上都有一个配置文件。 换句话说，如果用户在登录到服务器 A 的工作站时在 "我的文档" 中保存了文件，则在登录到服务器 B 的工作站时，它们将不会看到该文件。|  