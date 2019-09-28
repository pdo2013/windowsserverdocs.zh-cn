---
title: 网络注意事项和用户帐户
description: 为不同的网络和用户方案提供有关 MultiPoint 服务的规划信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5369776a0341bf1f4d4d1d13569cf0964fdf11f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405032"
---
# <a name="network-considerations-and-user-accounts"></a>网络注意事项和用户帐户
MultiPoint 服务可以在各种网络环境中部署，并且可以支持本地用户帐户和域用户帐户。 通常，MultiPoint 服务用户帐户将在以下网络环境之一中进行管理：  
  
-   使用本地用户帐户运行 MultiPoint 服务的单台计算机  
  
-   多台运行 MultiPoint 服务的计算机，每台计算机都有一个本地用户帐户  
  
-   多台运行 MultiPoint 服务的计算机，以及使用域用户帐户的计算机

按照定义，只能从创建*本地用户帐户*的计算机对其进行访问。 本地用户帐户是在运行 MultiPoint 服务的特定计算机上创建的用户帐户。 与此相反，*域用户帐户*是位于域控制器上的用户帐户，并且可以从任何连接到域的计算机进行访问。 确定要使用哪种类型的网络环境时，请考虑以下事项：  
  
-   是否在服务器之间共享资源？  
  
-   用户是否要在服务器之间切换？  
  
-   用户是否要访问需要身份验证的数据库服务器？  
  
-   用户是否需要访问需要进行身份验证的内部 web 服务器？  
  
-   是否存在现有 Active Directory 域基础结构？  
  
-   谁将使用 MultiPoint 管理器控制台来管理用户桌面、查看缩略图、添加用户、限制网站等等？ 此人是否会管理多台服务器？ 此人必须具有服务器的管理权限。  
  
以下部分介绍了这些网络环境中的用户帐户管理。  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>具有本地用户帐户的单点服务器  
在运行 MultiPoint 服务的单台计算机的环境中，不要求有网络。 但是，若要利用 Internet 资源，网络要求可能与路由器和 Internet 服务提供商（ISP）的连接基本相同。 默认情况下，配置了与 MultiPoint 服务上的网络适配器关联的网络连接，以通过 DHCP 自动获得 IP 地址和 DNS 服务器地址。 通常将 Internet 路由器配置为 DHCP 服务器，并向连接到内部网络上的计算机的计算机提供专用 IP 地址。 因此，运行 MultiPoint 服务的一台计算机可以连接到路由器的内部接口，获取自动 IP 信息，并通过管理员无需大量精力或配置即可连接到 Internet。  
  
管理这种环境中的用户的常见方法是为将访问系统的每个用户创建一个本地用户帐户。 在该计算机上拥有本地用户帐户的任何人都可以从与系统关联的任何工作站登录到 MultiPoint 服务。 可以从 MultiPoint 管理器创建和管理本地用户帐户。  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>具有本地用户帐户的多个 MultiPoint Server 系统  
假设本地用户帐户只能从创建它们的计算机访问，当你在一个环境中部署多个 MultiPoint 服务系统时，你可以通过以下两种方式之一来管理本地用户帐户：  
  
-   你可以在运行 MultiPoint 服务的特定计算机上为特定个人创建用户帐户。  
  
-   在运行 MultiPoint 服务的每台计算机上，可以使用 MultiPoint 管理器为每个用户创建帐户。  
  
例如，如果你计划将用户分配到运行 MultiPoint 服务的特定计算机，你可以在计算机 A 上创建四个本地用户帐户（user01、user02、user03 和 user04）和计算机 B 上的四个本地用户帐户（user05、user06、user07 和 user08）。 在此方案中，用户 01 @ no__t-004 可以从连接到计算机 A 的任何工作站登录到计算机 A;但是，它们不能登录到计算机 B。这同样适用于用户 05 @ no__t-108，用户可以仅登录到计算机 B，而不能登录到计算机 A。具体取决于特定的部署环境，这可能是可接受的，甚至是理想的。  
  
但是，如果每个用户都必须能够登录到运行 MultiPoint 服务的任何计算机，则必须在运行 MultiPoint 服务的每台计算机上为每个用户创建一个本地用户帐户。 选择以这种方式管理用户会带来某些复杂性。 例如，如果 user01 在星期一登录到计算机 A，并将文件保存在 Documents 文件夹中，然后用户在星期二登录到计算机 B，则在计算机 B 上的 "文档" 文件夹中保存的文件将无法访问。  
  
此外，如果用户在计算机 A 和计算机 B 上拥有帐户，则无法自动同步帐户的密码。 这可能会导致用户登录时遇到困难，因为帐户密码在一台计算机上发生更改，而不是在另一台计算机上更改。 可以通过将每个用户分配到运行 MultiPoint 服务的一台计算机，简化此类网络环境中的用户帐户管理。 这样，用户便可以登录到与该计算机相关联的任何工作站并访问相应的文件。  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>具有域帐户的多个 MultiPoint 服务系统  
域环境在包含多个服务器的大型网络环境中很常见。 例如，你可以将运行 MultiPoint 服务角色的一台或多台计算机加入到域，然后使用 Microsoft Active Directory 来管理可从域中的任何计算机访问的用户帐户。 这样，便可以从任何已加入域的 MultiPoint 服务系统中的任何工作站创建和访问单个域用户帐户。  
 
在域环境中部署 MultiPoint 服务时，需要考虑以下几个因素：  
  
-   如果使用域帐户，则不能从 MultiPoint 管理器对其进行管理。  
  
-   默认情况下，MultiPoint 服务配置为允许每个用户一次只登录一个工作站。 如果你决定允许用户使用单个帐户同时登录到多个工作站，则可以使用 MultiPoint 管理器中的 "**编辑服务器设置**" 选项。  
  
-   域控制器的位置可能会影响用户能够在域中进行身份验证并找到资源的速度和可靠性。  
  
## <a name="single-user-account-for-multiple-stations"></a>多个工作站的单个用户帐户  
MultiPoint 服务能够使用单个用户帐户同时登录同一计算机上的多个工作站。 此功能在用户未获得唯一用户名的环境中很有用，其中使用单个用户帐户可以简化 MultiPoint 服务系统的管理。  
  
