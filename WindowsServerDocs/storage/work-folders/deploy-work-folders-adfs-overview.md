---
title: "使用 AD FS 和 Web 应用程序代理部署工作文件夹概述"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 48c7d771c7ec75a4bc340608a96410ea388418e9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>使用 AD FS 和 Web 应用程序代理部署工作文件夹：概述

>适用于：Windows Server（半年频道）、Windows Server 2016

本节中的主题提供了使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的说明。 这些说明旨在帮助你使用准备好开始在本地或 Internet 上开始使用工作文件夹的客户端计算机创建一个功能齐全的工作文件夹设置。  
  
工作文件夹是在 Windows Server 2012 R2 中引入的组件，允许信息工作者在其设备之间同步工作文件。 有关工作文件夹的详细信息，请参阅[工作文件夹概述](Work-Folders-Overview.md)。  
  
若要使用户能够通过 Internet 同步其工作文件夹，需要通过反向代理发布工作文件夹，以便在 Internet 上从外部使用工作文件夹。 AD FS 中包含的 Web 应用程序代理是你可以用来提供反向代理功能的一个选项。 Web 应用程序代理通过使用 AD FS 来预先验证对工作文件夹 Web 应用程序的访问，以便任何设备上的用户可以从公司网络外部访问工作文件夹。 

> [!NOTE]
>   本节中包含的说明适用于 Windows Server 2016 环境。 如果你使用的是 Windows Server 2012 R2，请遵循 [Windows Server 2012 R2 说明](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。
  
这些主题提供以下内容：  
  
-   通过 Windows Server 用户界面使用 AD FS 和 Web 应用程序代理设置和部署工作文件夹的分步说明。 这些说明介绍了如何使用自签名证书设置简单的测试环境。 然后，你可以使用测试示例作为指导，以帮助你创建使用公共可信信任的证书的生产环境。  
  
## <a name="prerequisites"></a>先决条件  
要遵循这些主题中的过程和示例，需要准备好以下组件：  
  
-   在 Windows Server 2012 R2 中提供一个包含架构扩展的 Active Directory® 域服务林，以便在使用多个文件服务器时支持将电脑和设备自动定向到正确的文件服务器 最好在林中启用 DNS，但这不是必需的。  
  
-   域控制器：启用 AD DS 角色并配置了域（对于测试示例为 contoso.com）的服务器。  
  
    需要至少运行 Windows Server 2012 R2 的域控制器，以便支持 Workplace Join 的设备注册。 如果不想使用 Workplace Join，则可以在域控制器上运行 Windows Server 2012。  
  
-   两个已加入域（例如 contoso.com）并运行 Windows Server 2016 的服务器。 一个服务器将用于 AD FS，另一个将用于工作文件夹。  
  
-   一个未加入域且运行 Windows Server 2016 的服务器。 该服务器将运行 Web 应用程序代理，并且必须为网络域（例如 contoso.com）提供一个网卡，并为外部网络提供另一个网卡。  
  
-   一个已加入域的客户端计算机，且运行的是 Windows 7 或更高版本。  
  
-   一个未加入域的客户端计算机，且运行的是 Windows 7 或更高版本。  
  
对于我们在本指南中介绍的测试环境，应该具有下图所示的拓扑。 这些计算机既可以是物理计算机也可以虚拟机 (VM)。 
  
![显示 Internet、外围网络和 Contoso 网段的图表。 在 Internet 段中：Client2；在外围网络中：一个 WAP 服务器；在 Contoso 段中：工作文件夹控制器、域控制器、AD FS 服务器和 Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>部署概述  
在这一组主题中，你将在测试环境中演练设置 AD FS、Web 应用程序代理和工作文件夹的分步示例。 将按以下顺序设置组件：  
  
1.  AD FS  
  
2.  工作文件夹  
  
3.  Web 应用程序代理  
  
4.  加入域的工作站和未加入域的工作站  
  
你还将使用 Windows PowerShell 脚本创建自签名证书。  
  
## <a name="deployment-steps"></a>部署步骤  
要使用 Windows Server 用户界面执行部署，请按照以下主题中的步骤操作：  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 1，设置 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 2，AD FS 配置后工作](deploy-work-folders-adfs-step2.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 3，设置工作文件夹](deploy-work-folders-adfs-step3.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 4，设置 Web 应用程序代理](deploy-work-folders-adfs-step4.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 5，设置客户端](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>另请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
[设计工作文件夹实施方案](Plan-Work-Folders.md)  
[部署工作文件夹](Deploy-Work-Folders.md)  
  

