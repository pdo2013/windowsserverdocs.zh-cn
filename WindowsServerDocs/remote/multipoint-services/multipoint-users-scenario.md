---
title: MultiPoint 服务用户帐户
description: 了解如何在 MultiPoint Services 中，尤其是针对不同方案使用哪些类型的用户帐户
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
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855358"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>示例方案：MultiPoint 服务用户帐户
若要执行的操作实施为你的 MultiPoint Services 环境选择了该用户帐户方案，您需要什么？ 下表描述了每个任务来执行用户帐户进行配置，并在独立的多点计算机上或在工作组或 Active Directory 域中的网络服务器上的共享或个人用户帐户准备工作站。 选择适用于你的环境的方案。 然后按照表完成每个所需的配置任务中的链接。  
  
> [!NOTE]  
> 如果尚不确定如何设置你的用户帐户，请参阅[规划你的 MultiPoint Services 环境的用户帐户](Plan-user-accounts-for-your-MultiPoint-services-environment.md)有关每个选项如何影响用户的详细信息。  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>在独立环境 （无网络） 中的单 MultiPoint Services 计算机  
  
|||  
|-|-|  
|**我的用户不需要登录。** 工作站可以是任何人都可以走到它们。 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.创建一个本地用户帐户 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)<br />3.[配置为自动登录的工作站](Configure-stations-for-automatic-logon.md)|  
|**我的用户都可以共享相同的用户登录。** 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.创建一个本地用户帐户 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)|  
|**我的用户必须具有其自己单独的 Windows 桌面体验。**|创建每个用户的本地用户帐户 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>在网络中，但没有域的多个 MultiPoint 服务计算机  
  
|||  
|-|-|  
|**我的用户不需要登录。** 工作站可以是任何人都可以走到它们。 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.每个服务器上创建一个本地用户帐户。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)每台服务器上<br />3.[配置为自动登录工作站](Configure-stations-for-automatic-logon.md)每台服务器上|  
|**我的用户都可以共享相同的用户登录。** 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.每个服务器上创建一个本地用户帐户。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)每台服务器上。|  
|**我的用户必须具有其自己单独的 Windows 桌面体验。**<br /><br />-   **选项 A** -我的用户将始终使用连接到同一台 MultiPoint 服务计算机的本地工作站。<br />-   **选项 B** -我的用户将多个 MultiPoint 服务计算机上使用本地工作站。<br />-   **选项 C** -我的用户将在 LAN 上使用远程客户端。|-   **选项 A** -每个服务器上为该服务器上的用户创建一个本地用户帐户。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />-   **选项 B** -每个服务器上创建的每个用户的本地用户帐户。 **注意：** 这意味着每个用户将每个服务器上有一个配置文件。 换而言之，如果他们在登录到服务器 A 的工作站上时我的文档中保存的文件，他们将看不该文件时登录到服务器 B 的工作站。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br />-   **选项 C** -每个用户分配到特定的 MultiPoint 服务计算机。 每个服务器上创建本地用户帐户以执行分配的用户。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>域网络环境中的一个或多个 MultiPoint 服务计算机  
  
|||  
|-|-|  
|**我的用户不需要登录。** 工作站可以是任何人都可以走到它们。 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.创建域帐户登录到服务器。<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)每台服务器上。<br />3.[配置为自动登录工作站](Configure-stations-for-automatic-logon.md)每台服务器上。|  
|**我的用户都可以共享相同的用户登录。** 他们不需要单独的 Windows 桌面体验，包括用于存储数据或个性化的桌面的私有文件夹。|1.创建一个组或每个用户的域帐户。<br />2.[允许一个帐户进行了多个会话](Allow-one-account-to-have-multiple-sessions.md)每台服务器上。|  
|**我的用户必须具有其自己单独的 Windows 桌面体验。**<br /><br />-   **选项 A** -具有域帐户的任何用户可以使用在 MultiPoint Services 计算机。<br />-   **选项 B** -我想要限制哪些域帐户可以访问服务器。|-   **选项 A** -无需设置是必需的。 默认情况下，所有域用户都有权访问 MultiPoint 服务的任何计算机在网络上。<br />-   **选项 B** -限制到 MultiPoint 服务计算机的域用户帐户的访问权限。 有关说明，请参阅[限制对服务器的用户访问](limit-users--access-to-the-server-in-multipoint-services.md)。|  
|**我想要使用本地用户帐户和从我的域帐户单独管理它们。** 例如，希望有人管理 MultiPoint 服务，但不是在域管理或不希望向所有 MultiPoint 服务用户提供域帐户。|每个服务器上创建一个或多个本地用户帐户。 (有关说明，请参阅[创建本地用户帐户](Create-local-user-accounts.md)。)<br /><br />**注意：** 这意味着每个用户帐户将每个服务器上具有一个配置文件。 换而言之，如果他们在登录到服务器 A 的工作站上时我的文档中保存的文件，他们将看不该文件时登录到服务器 B 的工作站。|  