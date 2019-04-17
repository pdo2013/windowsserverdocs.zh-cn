---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: "标识你正常工作的级别升级"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9527a186cb20c470e0b5644fff58f90786520f97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-your-functional-level-upgrade"></a>标识你正常工作的级别升级

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以提升域和森林功能级别之前，你必须评估你当前的环境，并找出功能级别要求最符合你的组织的需求。 通过识别您的森林中的域位于每个域、 操作系统和 service pack 的支持运行的每个域控制器，并且你计划升级域控制器日期域控制器评估你当前的环境。 如果你打算停用域控制器，因此请确保你了解这样做会对您的环境的完整影响。  
  
以下情况下可能会阻止你升级到 Windows Server 2008 或 Windows Server 2008 R2 的功能级别早期版本的 Windows Server 操作系统：  
  
-   没有足够的硬件  
  
-   运行的是 Windows Server 2008 或 Windows Server 2008 R2 兼容的防病毒程序域控制器   
  
-   使用不运行 Windows Server 2008 或 Windows Server 2008 R2 特定版本的程序   
  
-   需要升级的具有最新 service pack 的程序  
  
记录此信息可帮助你确定需要确保你拥有全功能的 Windows Server 2008 或 Windows Server 2008 R2 环境步骤。  
  
评估你当前的环境后，你需要确定适用于你的组织功能级别升级。 这些选项将可用：  
  
-   对 Windows Server 2008 或 Windows Server 2008 R2 的 Windows 2000 本机模式环境   
  
-   对 Windows Server 2008 或 Windows Server 2008 R2 的 Windows Server 2003 森林   
  
-   新的 Windows Server 2008 森林  
  
-   新的 Windows Server 2008 R2 森林  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升级原始的 Windows 2000 活动目录森林中的功能级别  
Windows 2000 原始的环境中仅包括基于 Windows 2000 的域控制器，功能级别设置为以下级别，默认情况下，并它们一直在这些级别手动筹集：  
  
-   Windows 2000 原始域功能级别  
  
-   Windows 2000 森林功能级别  
  
若要使用在 Windows Server 2008 或 Windows Server 2008 R2 的所有森林级别和域级别功能，你需要升级到 Windows Server 2008 或 Windows Server 2008 R2 此 Windows 2000 环境。 你可以通过以下方法之一来执行此升级：  
  
-   引入新安装的 Windows Server 2008-基于 Windows Server 2008 R2 或-基于到森林，域控制器，然后停用的所有运行 Windows 2000 的域控制器。  
  
-   执行所有现有域控制器到运行 Windows Server 2003 域控制器树林中运行 Windows 2000 就地的升级。 然后，执行就地升级到 Windows Server 2008 或 Windows Server 2008 R2 这些域控制器。 有关详细信息，请参阅[对 Windows Server 2008 广告 DS 域 \[LH\ 升级 Active Directory 域]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你服务器运行的基于 x64 的 Windows Server 2003 版本，你可以成功执行就地升级的 Windows Server 2008 R2 到此计算机的操作系统。 服务器运行的基于 x86 的 Windows Server 2003 版本，如果你无法升级到 Windows Server 2008 R2 的此计算机。  
  
若要使用的 Windows Server 2008 或 Windows Server 2008 R2 域级别功能，无需升级到 Windows Server 2008 或 Windows Server 2008 R2 的你的整个 Windows 2000 森林，提升仅域功能级别对 Windows Server 2008 或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 筹集域功能级别之前，你必须升级到 Windows Server 2008 或 Windows Server 2008 R2 的域中的所有基于 Windows 2000 的域控制器。  
  
森林中的所有基于 Windows 2000 的域控制器替换运行 Windows Server 2008 或 Windows Server 2008 R2 域控制器后，你可以提升对 Windows Server 2008 或 Windows Server 2008 R2 森林功能级别。 会自动引发树林中所有本机或更高为 Windows Server 2008 或 Windows Server 2008 R2 设置为 Windows 2000 的域的功能级别。  
  
有关提升森林和域功能级别，以及有关以执行这些任务的步骤，请参阅[部署 Windows Server 2008 森林根域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升级 Windows Server 2003 Active Directory 森林中的功能级别  
在 Windows Server 2003 环境中组成仅在基于 Windows Server 2003 的域控制器，功能级别设置为以下级别，默认情况下，并它们一直在这些级别手动筹集：  
  
-   Windows 2000 原始域功能级别  
  
-   Windows 2000 森林功能级别  
  
若要使用在 Windows Server 2008 或 Windows Server 2008 R2 的所有森林级别和域级别功能，你需要升级到 Windows Server 2008 或 Windows Server 2008 R2 此 Windows Server 2003 环境。 你可以通过以下方法之一来执行此升级：  
  
-   引入新安装的 Windows Server 2008-基于 Windows Server 2008 R2 或-基于到森林，域控制器然后停用的所有运行 Windows Server 2003 的域控制器，或将其升级到 Windows Server 2008 或 Windows Server 2008 R2。  
  
-   执行所有运行 Windows Server 2003 到运行 Windows Server 2008 或 Windows Server 2008 R2 域控制器的现有域控制器就地的升级。 有关详细信息，请参阅[对 Windows Server 2008 广告 DS 域 \[LH\ 升级 Active Directory 域]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你服务器运行的基于 x64 的 Windows Server 2003 版本，你可以成功执行就地升级的 Windows Server 2008 R2 到此计算机的操作系统。 服务器运行的基于 x86 的 Windows Server 2003 版本，如果你不能升级这台计算机运行 Windows Server 2008 R2。  
  
无需升级到 Windows Server 2008 或 Windows Server 2008 R2 的你的整个 Windows Server 2003 树林中使用的 Windows Server 2008 或 Windows Server 2008 R2 域级别功能，提升仅域功能级别对 Windows Server 2008 或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 筹集域功能级别之前，你必须升级到 Windows Server 2008 或 Windows Server 2008 R2 的域中的所有基于 Windows Server 2003 的域控制器。  
  
你升级到 Windows Server 2008 或 Windows Server 2008 R2 的树林中的所有基于 Windows Server 2003 的域控制器后，你可以提升对 Windows Server 2008 或 Windows Server 2008 R2 森林功能级别。 会自动引发的功能均已设置为 Windows Server 2008 或 Windows Server 2008 R2 的 Windows Server 2003 树林中的所有域级别。  
  
有关提升森林和域功能级别，以及有关以执行这些任务的步骤，请参阅[部署 Windows Server 2008 森林根域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>新的 Windows Server 2008 森林中的升级功能级别  
当你在新的 Windows Server 2008 森林安装的第一个域控制器时，功能级别默认设置为以下级别，和它们一直在这些级别手动筹集：  
  
-   Windows 2000 原始域功能级别  
  
-   Windows 2000 森林功能级别  
  
在这些默认级别上设置功能级别来为你提供新的 Windows Server 2008 林到添加 Windows 2000 或基于 Windows Server 2003 的域控制器上的选项。 创建森林根域后，你将添加到 Windows Server 2008 森林每个域域功能级别设置为 Windows 2000 原始。 但是，如果你希望在运行 Windows Server 2008 你新的 Windows Server 2008 环境中的所有域控制器，林功能级别，然后设置域功能级别，对 Windows Server 2008 第一个域控制器安装在你森林时。 执行此操作节省时间，并启用 Windows Server 2008 中的所有森林级别和域级别功能。  
  
> [!IMPORTANT]  
> 如果森林都将在 Windows Server 2008 功能运行级别和你尝试在基于 Windows Server 2003 成员服务器安装 Active Directory，或者基于 Windows 2000 的成员服务器，在安装将失败。  
  
有关提升森林和域功能级别，以及有关以执行这些任务的步骤，请参阅[部署 Windows Server 2008 森林根域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>新的 Windows Server 2008 R2 森林中的升级功能级别  
当你在新的 Windows Server 2008 R2 森林安装的第一个域控制器时，功能级别默认设置为以下级别，和它们一直在这些级别手动筹集：  
  
-   Windows Server 2003 域功能级别  
  
-   Windows Server 2003 森林功能级别  
  
在这些默认级别上设置功能级别可让您选择添加到你的新 Windows Server 2008 R2 林基于 Windows Server 2003 的域控制器。 创建森林根域后，你将添加到 Windows Server 2008 R2 森林每个域域功能级别设置为 Windows Server 2003。 但是，如果你希望在运行 Windows Server 2008 R2 你新的 Windows Server 2008 R2 环境中的所有域控制器，林功能级别，然后设置域功能级别，对 Windows Server 2008 R2 的第一个域控制器安装在你的森林时。 执行此操作节省时间，并启用 Windows Server 2008 R2 中的所有森林级别和域级别功能。  
  
> [!IMPORTANT]  
> 如果在 Windows Server 2008 R2 功能级别表示森林和你尝试安装在 Windows Server 2008 Active Directory-基于或者基于 Windows Server 2003 成员服务器，或在基于 Windows 2000 的成员服务器上，则安装将失败。  
  
有关提升森林和域功能级别，以及有关以执行这些任务的步骤，请参阅[部署 Windows Server 2008 森林根域 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
> [!NOTE]  
> 尽管 Windows Server 2008 上必须安装 ADMT 3.1 版，你可以使用 ADMT 3.1 版迁移到某个域，由一个或多个 Windows Server 2008 R2 域控制器托管的对象。 有关详细信息，请参阅[文章 976659](https://go.microsoft.com/fwlink/?LinkId=180398) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=180398) 中。  
  


