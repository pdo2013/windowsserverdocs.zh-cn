---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "启用定位下一步最近的域控制器的客户端"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>启用定位下一步最近的域控制器的客户端

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

如果你有运行 Windows Server 2008 或 Windows Server 2008 R2 域控制器，你可以使客户端计算机的运行 Windows Vista、Windows 7、Windows Server 2008 或 Windows Server 2008 R2 找不到更高效地通过启用域控制器**尝试下一个最近的站点**组策略设置。 此设置可域控制器定位器（DC 定位器）提高通过帮助与简化网络通信，尤其是在有许多分支机构和网站的大型企业。  
  
此新的设置可能会影响如何配置网站链接成本，因为它会影响域控制器位于的顺序。 对于企业中有许多中心站点和分支机构，可通过确保的客户端故障转移到下一步最近的中心站点时在最近的中心站点找不到某个域控制器显著减少网络上的 Active Directory 交通。  
  
作为常规的最佳做法，你应简化了网站拓扑和网站的链接金额尽可能多地如果您启用了**尝试下一个最近的站点**设置。 在企业中使用多个中心网站，这样可以简化你处理客户端，一个站点中的需要故障转移到另一台站点中的域控制器的情况下进行任何套餐。  
  
默认情况下，**尝试下一个最近的站点**未启用设置。 当未启用该设置时，直流定位器使用以下算法域控制器：  
  
-   尝试域控制器在同一的站点。  
  
-   如果没有域控制器在同一站点可用，请尝试在域中发现任何域控制器。  
  
> [!NOTE]  
> 这是直流定位器的 Active Directory 的以前版本中使用的相同算法。 有关详细信息，请参阅 DNS 如何 Active Directory 有效的支持 ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587))。  
  
如果您启用了**尝试下一个最近的站点**设置，直流定位器使用以下算法域控制器：  
  
-   尝试域控制器在同一的站点。  
  
-   如果没有域控制器在同一站点可用，请尝试域控制器在下一步最近的站点。 站点是接近是否与的费用更高版本站点链接比另一个站点成本较低站点链接。  
  
-   如果没有域控制器在下一步最近的站点可用，请尝试在域中发现任何域控制器。  
  
默认情况下，直流定位器不会考虑只读域控制器 (RODC) 中包含了时它决定接下来最近的站点的任何站点。 此外，只有 Windows Server 2008 和 Windows Server 2008 R2 域控制器支持的下一步最近站点功能，当客户端从运行较早版本的 Windows Server 的域控制器获取回复，因为直流定位器行为等同于时再设置未启用。  
  
例如，假设站点拓扑具有站点链接值，在下图中使用的四个站点。 在此示例中，所有域控制器都是可写的域控制器上运行 Windows Server 2008 或 Windows Server 2008 R2。  
  
![启用定位直流的客户端](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
当**尝试下一个最近的站点**如果客户端计算机运行 Windows Vista、Windows 7、Windows Server 2008、组策略设置处于启用状态在此示例中，或 Windows Server 2008 R2 Site_B 在尝试查找某个域控制器，首先尝试查找域控制器在其自身 Site_B。 如果没有可用 Site_B 中，它将尝试中 Site_A 查找域控制器。  
  
如果未启用该设置，客户端尝试 Site_A、Site_C 或 Site_D 中查找域控制器，如果没有域控制器在 Site_B 可用。  
  
> [!NOTE]  
> **尝试下一个最近的站点**设置的工作方式配合自动站点覆盖率。 例如，如果没有域控制器下一步最近的站点，直流定位器会尝试查找该站点执行自动站点覆盖率域控制器。  
  
将应用**尝试下一个最近的站点**设置，你可以创建组策略对象 (GPO)，并将其链接到你的组织的相应对象或可以修改默认域策略，使其影响运行 Windows Vista、Windows 7、Windows Server 2008 或 Windows Server 2008 R2 域中的所有客户端。 有关如何设置的详细信息**尝试下一个最近的站点**设置，请参阅[使客户端定位域控制器，在下一步最近的站点](https://technet.microsoft.com/library/cc772592.aspx)。  
  


