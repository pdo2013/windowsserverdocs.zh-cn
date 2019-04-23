---
title: 规划 MultiPoint 服务环境的用户帐户
description: 规划 MultiPoint Services 中的用户帐户信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864168"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>规划 MultiPoint 服务环境的用户帐户
在 MultiPoint Services 中实现的用户帐户的最佳方法取决于大小和你的部署的复杂性：  
  
-   **本地用户帐户**-在小型部署中使用只有少数计算机正在运行 MultiPoind Services 和几个用户，你可能会发现它使用最方便*本地用户帐户*MultiPoint 服务上创建的。 可以为每个用户都将使用该系统，或创建一个通用的每个工作站，任何人都可以使用登录帐户创建个人帐户。 MultiPoint 服务管理员创建和使用 MultiPoint 管理器管理本地用户帐户。 本地帐户可以是管理员，具有有限的管理权限，也是与 MultiPoint 服务桌面或 MultiPoint 管理器不能访问的普通用户。  
  
-   **域帐户**-如果你的环境有多台计算机运行 MultiPoint 服务和多个用户，您可能会发现它更有用设置 Active Directory 域服务\(AD DS\)域并使用*域用户帐户*，这使用户能够从域中的任何工作站访问她自己的用户配置文件和设置。 由域管理员，必须在域控制器上创建域用户帐户。  
  
> [!NOTE]  
> 以下各节讨论您可以为 MultiPoint Services 中的本地用户帐户实现的方案。 如果使用域用户帐户，请参阅中的"一个或多个 MultiPoint server 域网络环境中的"方案[示例方案：MultiPoint 服务用户帐户](Example-scenarios--MultiPoint-Services-user-accounts.md)。  
  
## <a name="planning-local-user-accounts"></a>规划本地用户帐户  
优点、 缺点和多种方法来实现在 Windows MultiPoint Services 环境中的单个或共享的本地用户帐户的要求，请考虑以下各节。  
  
### <a name="use-individual-local-user-accounts"></a>使用单独的本地用户帐户  
创建本地用户帐户时，必须在选项的两种方法。  每个用户分配到特定服务器运行 MultiPoint 服务并创建用于每个用户的单一帐户。 或在运行 Multipoint 服务的每台计算机上创建所有用户的本地用户帐户。 实现单个用户帐户的一个主要优点是每个用户具有他或她自己 Windows 桌面体验，包括用于存储数据的私有文件夹。 
  
从系统管理的角度来看，将用户分配到特定的 MultiPoint 服务计算机可能会更方便。 例如，如果您有两个具有五个工作站的 MultiPoint server，可能在下表中所示创建本地用户帐户。  
  
**表 1:将本地用户帐户分配到运行 MultiPoint 服务的特定计算机**  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
在此方案中，每个用户都有特定计算机上的单个帐户。 因此，每个人在计算机 A 可以从任何工作站与计算机 a。 关联到她或他的帐户登录的本地帐户但是，这些用户无法访问其帐户，如果他们使用工作站关联与计算机 B，反之亦然。 此方法的优点是，通过始终连接到同一台计算机，用户可始终找到并访问他们的文件。  
  
与此相反，还有可能要复制运行 MultiPoint 服务的所有计算机上的单个用户帐户下, 表中所示。  
  
**表 2:复制运行 MultiPoint 服务的所有计算机上的用户帐户**  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
此方法的优点是用户在每个可用的 MultiPoint 服务上具有本地用户帐户。 但是，缺点可能胜过这种优势。 例如，即使用户名和密码的某个特定的人是两台计算机上相同的对每个其他未链接帐户。 因此，如果用户登录到他或她的帐户在计算机 A 上的星期一，保存文件，然后登录到其在计算机 B 上的帐户在星期二，他或她将无法再访问文件以前保存的上此外计算机 a。复制多台计算机上的用户帐户会增加管理开销和存储要求。  
  
### <a name="use-generic-local-user-accounts"></a>使用通用的本地用户帐户  
如果你的 MultiPoint 服务系统未连接到域，并且您不希望创建单独为每个用户帐户，可以创建每个工作站的通用帐户。 例如，如果您有两台运行 MultiPoint 服务计算机，五个测量站都与每台计算机，您可能会决定创建用户帐户类似于下表中所示。  
  
**表 3:创建通用用户帐户，每个工作站的一个帐户**  
  
|计算机 A|计算机 B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
在此方案中，每个工作站帐户具有相同的密码，并且密码和一般用户帐户名可供所有用户。 这种方法的优点之一是管理用户帐户的开销很可能是小于如果使用个人帐户，因为通常比用户更少的工作站。 此外，也将复制的每个服务器上的用户帐户，从而导致的开销。  
  
另一种方法是在每个服务器上创建通用帐户。 每个用户登录到服务器上相同的帐户。 若要允许此操作，必须启用每个帐户的多个会话。 您可以进一步简化所有服务器上使用同一帐户名和密码。 这简化了用户，只需知道帐户名称和密码以使用任何服务器上的任何工作站的登录。 应注意，在此方案中的所有用户可以都看到的任何用户进行任何更改。 例如，如果将文件保存到桌面，所有用户可以都看到该文件。  
  
> [!IMPORTANT]  
> 请务必了解，当用户共享的用户帐户，是每个服务器的一个或一个每个工作站，– 甚至文件保存在我的文档-在服务器上保存的文件不是专用。 任何与帐户登录的用户有权访问这些文件。 如果用户将文件保存到我的文档，其中的一个工作站上每个工作站，使用一个帐户，用户不会对不同的工作站上这些文件具有访问权限。 不同的 MultiPoint 服务计算机进行日志记录时，会出现相同情况。  
  
若要使用户能够从任何工作站访问其文件，可以使用文件服务器、 创建文件共享的每个用户帐户，或让用户将其个人文档存储在 USB 闪存驱动器或其他专用存储设备上。 单个 USB 闪存驱动器启用单个用户来存储私有文档，即使它们共享 MultiPoint 服务上的用户帐户。