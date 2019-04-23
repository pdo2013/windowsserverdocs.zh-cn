---
title: 网络注意事项和用户帐户
description: 通过 MultiPoint Services 提供不同的网络和用户方案的规划信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880798"
---
# <a name="network-considerations-and-user-accounts"></a>网络注意事项和用户帐户
MultiPoint 服务可以部署在各种网络环境中，在它可以支持本地用户帐户和域用户帐户。 通常情况下，将在以下网络环境中管理 MultiPoint 服务用户帐户：  
  
-   使用本地用户帐户运行 MultiPoint 服务的单台计算机  
  
-   多台计算机运行 MultiPoint 服务，每个本地用户帐户  
  
-   多台计算机运行 MultiPoint Services 并使用域用户帐户

根据定义，*本地用户帐户*仅可以从其创建它们的计算机访问。 本地用户帐户是正在运行 MultiPoint 服务的特定计算机创建的用户帐户。 与此相反，*域用户帐户*是域控制器上驻留的用户帐户和可以从任何连接到域的计算机访问它们。 在决定哪种类型的网络环境，以使用，考虑以下方面：  
  
-   将在服务器之间共享资源  
  
-   将服务器之间切换用户？  
  
-   用户访问数据库服务器要求进行身份验证？  
  
-   用户访问内部 web 服务器需要身份验证？  
  
-   在位置是否有现有的 Active Directory 域基础结构？  
  
-   谁将使用 MultiPoint 管理器控制台来管理用户桌面，查看缩略图，将用户添加、 限制网站，以及其他操作？ 将此人管理多台服务器？ 此人必须在服务器上具有管理权限。  
  
以下各节介绍这些网络环境中的用户帐户管理。  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>使用本地用户帐户进行单一 MultiPoint Server  
在与运行 MultiPoint 服务的单一计算机环境中，不是要求必须有一个网络。 但是，若要充分利用 Internet 资源，网络要求可能是基本的路由器和 Internet 服务提供商 (ISP) 的连接。 默认情况下，若要获取的 IP 地址和 DNS 服务器地址通过 DHCP 自动配置与 MultiPoint 服务上的网络适配器相关联的网络连接。 Internet 路由器通常配置为 DHCP 服务器，并提供到内部网络连接到这些计算机的专用 IP 地址。 因此，一台运行 MultiPoint 服务计算机可以连接到路由器的内部接口、 获取自动 IP 信息，并连接到 Internet 而无需大量工作或由管理员配置。  
  
在这种环境中管理用户的常用方法是为每个人员想要访问系统创建本地用户帐户。 任何具有该计算机上的本地用户帐户的人都可以登录到 MultiPoint 服务从任何工作站与系统相关联。 可以创建和管理 MultiPoint 管理器从本地用户帐户。  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>使用本地用户帐户的多个 MultiPoint Server 系统  
假设本地用户帐户是仅可从其创建了它们，当您部署的环境中的多个 MultiPoint 服务系统的计算机访问，你可以管理本地用户帐户在两种方式之一：  
  
-   你可以在运行 MultiPoint 服务的特定计算机上的特定个人创建用户帐户。  
  
-   可以使用 MultiPoint 管理器在运行 MultiPoint 服务的每台计算机上创建的每个用户帐户。  
  
例如，如果您计划要将用户分配到运行 MultiPoint 服务的特定计算机，你可能会创建四个本地用户帐户计算机 A 上 （user01、 user02、 user03 和 user04） 和四个本地用户帐户在计算机 B （user05、 user06、 user07 和 user08） 上。 在此方案中，用户 01\-04 可以登录到计算机 A 从任何工作站连接到它; 但是，他们无法登录到计算机 b。同样适用于用户 05\-08，他们将能够登录仅计算机 B 上，但不适用于计算机 a。 具体情况取决于特定部署环境，这可以是可接受或甚至需要。  
  
但是，如果每个用户必须能够登录到的任何运行 MultiPoint 服务计算机上，必须为每个用户正在运行 MultiPoint 服务的每台计算机上创建本地用户帐户。 选择来管理用户的这种方式引入了某些复杂性。 例如，如果 user01 登录到计算机 A 上星期一，并将保存在文档文件夹中的文件，然后在用户登录到计算机 B 上星期二，计算机 A 上的文档文件夹中保存的文件无法访问计算机 B 上  
  
此外，如果用户具有在计算机 A 和计算机 B 上的帐户，将无法自动同步帐户的密码。 这可能导致用户使用登录应在一台计算机，而另一个没有更改帐户密码时遇到困难。 可以通过将每个用户分配到一台运行 MultiPoint 服务来简化这种类型的网络环境中的用户帐户管理。 这样一来，用户可以登录到任何与该计算机相关联并访问相应的文件的工作站。  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>使用域帐户的多个 MultiPoint 服务系统  
在包含多个服务器的大型网络环境中常见的域环境。 例如，您可能会加入到域中，运行 MultiPoint 服务角色的一个或多个计算机，然后使用 Microsoft Active Directory 管理可以从域中的任何计算机访问的用户帐户。 这允许单个域用户帐户创建和访问从任何工作站加入到域任何 MultiPoint 服务系统中。  
 
在域环境中部署 MultiPoint 服务时，有几个因素需要考虑：  
  
-   如果使用域帐户，它们不能从 MultiPoint 管理器管理。  
  
-   默认情况下，MultiPoint 服务配置为每个用户的权限一次登录到只有一个工作站。 如果你决定允许用户在同一时间使用单个帐户登录到多个工作站，则可以使用**编辑服务器设置**MultiPoint 管理器中的选项。  
  
-   速度和可靠性的用户将能够使用域进行身份验证，并查找资源，则可能会影响域控制器的位置。  
  
## <a name="single-user-account-for-multiple-stations"></a>多个工作站的单个用户帐户  
MultiPoint 服务能够登录到多个工作站上同时使用单个用户帐户在同一台计算机上。 此功能可在环境中用户可以不使用唯一的用户名称，并且使用单个用户帐户可以简化对 MultiPoint 服务系统的管理。  
  
