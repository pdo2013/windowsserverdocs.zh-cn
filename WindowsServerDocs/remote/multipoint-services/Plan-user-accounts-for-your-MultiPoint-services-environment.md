---
title: 规划 MultiPoint 服务环境的用户帐户
description: MultiPoint Services 中用户帐户的规划信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 056c3b9773387cf00b40baf6f14e4e1f3583f6c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405008"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>规划 MultiPoint 服务环境的用户帐户
在 MultiPoint Services 中实现用户帐户的最佳方式取决于部署的大小和复杂性：  
  
-   **本地用户帐户**-对于只包含几台运行 MultiPoind Services 的计算机和少数用户的小型部署，你可能会发现，使用在 MultiPoint 服务上创建的*本地用户帐户*最为方便。 您可以为将使用系统的每个用户创建一个单独的帐户，也可以为每个工作站创建一个通用帐户，用户可以使用该帐户登录。 MultiPoint 服务管理员使用 MultiPoint 管理器创建和管理本地用户帐户。 本地帐户可以是管理员、具有有限的管理权限，也可以是不具有 MultiPoint 服务桌面或 MultiPoint 管理器访问权限的普通用户。  
  
-   **域帐户**-如果你的环境包含许多运行 MultiPoint 服务的计算机，你可能会发现，设置 Active Directory 域服务 \(AD DS @ no__t 域并使用*域用户帐户*更有用。这允许用户从域中的任何工作站访问自己的用户配置文件和设置。 域管理员必须在域控制器上创建域用户帐户。  
  
> [!NOTE]  
> 以下各节讨论了在 MultiPoint 服务中可能为本地用户帐户实现的方案。 如果使用的是域用户帐户，请参阅 @no__t 0Example 方案中的 "域网络环境中的一个或多个 MultiPoint 服务器" 方案：MultiPoint Services 用户帐户 @ no__t-0。  
  
## <a name="planning-local-user-accounts"></a>规划本地用户帐户  
以下各部分介绍了在 Windows MultiPoint 服务环境中实施单个或共享本地用户帐户的多种方法的优点、缺点和要求。  
  
### <a name="use-individual-local-user-accounts"></a>使用单独的本地用户帐户  
创建本地用户帐户时，有两种方法可供选择。  将每个用户分配到运行 MultiPoint 服务的特定服务器，并为每个用户创建单个帐户。 或为运行 Multipoint 服务的每台计算机上的所有用户创建本地用户帐户。 实现各个用户帐户的主要优点是每个用户都有自己的 Windows 桌面体验，其中包含用于存储数据的专用文件夹。 
  
从系统管理的角度来看，将用户分配到特定的 MultiPoint 服务计算机可能更方便。 例如，如果有两个多点服务器，每个都有五个工作站，则可能创建如下表中所示的本地用户帐户。  
  
@no__t 0Table 1：向运行 MultiPoint 服务 @ no__t 的特定计算机分配本地用户帐户  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
在此方案中，每个用户在特定计算机上都有一个帐户。 因此，在计算机 A 上拥有本地帐户的所有用户都可以从与计算机 A 关联的任何工作站登录到她或他的帐户。但是，如果用户使用与计算机 B 关联的工作站，则这些用户无法访问其帐户，反之亦然。 此方法的优点是，通过始终连接到同一台计算机，用户始终可以查找并访问其文件。  
  
与此相反，还可以在运行 MultiPoint 服务的所有计算机上复制单独的用户帐户，如下表所示。  
  
**Table 2：在运行 MultiPoint 服务 @ no__t 的所有计算机上复制用户帐户  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
此方法的优点是用户在每个可用的 MultiPoint 服务上都有一个本地用户帐户。 但是，缺点可能超过此优势。 例如，即使在这两台计算机上，特定用户的用户名和密码是相同的，也不会彼此链接。 因此，如果用户在星期一登录到计算机 A 上的帐户，保存文件，然后登录到星期二计算机 B 上的帐户，则他/她将无法访问以前在计算机 A 上保存的文件。此外，，在多台计算机上复制用户帐户会增加管理开销和存储要求。  
  
### <a name="use-generic-local-user-accounts"></a>使用通用本地用户帐户  
如果你的 MultiPoint 服务系统未连接到域，并且你不希望为每个用户创建一个单独的帐户，则可以为每个工作站创建通用帐户。 例如，如果有两台运行 MultiPoint 服务的计算机，并且有5个工作站与每台计算机相关联，则你可能会决定创建类似于下表中所示的用户帐户。  
  
**Table 3：创建一般用户帐户，每个工作站一个帐户 @ no__t-0  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
在此方案中，每个工作站帐户都具有相同的密码，并且密码和一般用户帐户名均可供所有用户使用。 此方法的一个优点是，管理用户帐户的开销可能会少于使用单个帐户的开销，因为通常不会有超过用户的工作站。 此外，还消除了在每个服务器上复制用户帐户导致的开销。  
  
另一种方法是在每个服务器上创建通用帐户。 每个用户都以同一帐户登录到服务器。 为此，你必须为每个帐户启用多个会话。 通过在所有服务器上使用相同的帐户名和密码，可以进一步简化。 这简化了用户的登录，用户只需知道一个帐户名和密码即可使用任何服务器上的任何工作站。 应注意的是，在这种情况下，所有用户都可以看到任何用户所做的任何更改。 例如，如果将文件保存到桌面，则所有用户都可以看到该文件。  
  
> [!IMPORTANT]  
> 重要的是，如果用户共享用户帐户（每个服务器或每个工作站一个用户帐户），则服务器上保存的文件–即使是保存在 "我的文档" 中的文件，也不是专用的。 使用该帐户登录的任何用户都可以访问这些文件。 为每个工作站使用一个帐户时，如果用户将文件保存到一个工作站上的 "我的文档"，则该用户无法访问其他工作站上的这些文件。 登录到不同的 MultiPoint 服务计算机时，会出现这种情况。  
  
要使用户能够从任何工作站访问其文件，你可以使用文件服务器、为每个用户帐户创建文件共享，或让用户将其个人文档存储在 USB 闪存驱动器或其他专用存储设备上。 单个 USB 闪存驱动器允许单独用户存储专用文档，即使它们在 MultiPoint 服务上共享用户帐户也是如此。