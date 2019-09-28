---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: 标识功能级别升级
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e43bec8a8d61cd0f6fd82982d5e3a0f01984fc65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408786"
---
# <a name="identifying-your-functional-level-upgrade"></a>标识功能级别升级

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在提升域和林功能级别之前，必须先评估当前环境，并确定最符合组织需求的功能级别要求。 通过标识林中的域、每个域中的域控制器、每个域控制器正在运行的操作系统和 service pack 以及计划升级域的日期，来评估当前环境控制器. 如果打算停用域控制器，请确保您了解对环境所做的完全影响。  
  
以下情况可能会阻止你将早期版本的 Windows Server 操作系统升级到 Windows Server 2008 或 Windows Server 2008 R2 功能级别：  
  
-   硬件不足  
  
-   运行与 Windows Server 2008 或 Windows Server 2008 R2 不兼容的防病毒程序的域控制器   
  
-   使用不在 Windows Server 2008 或 Windows Server 2008 R2 上运行的特定于版本的程序   
  
-   使用最新的 Service Pack 升级程序的需求  
  
记录此信息可帮助你确定要执行的步骤，以确保具有功能完备的 Windows Server 2008 或 Windows Server 2008 R2 环境。  
  
评估当前环境后，你必须确定适用于你的组织的功能级别升级。 可用选项如下：  
  
-   Windows 2000 本机模式环境到 Windows Server 2008 或 Windows Server 2008 R2   
  
-   Windows Server 2003 林到 Windows Server 2008 或 Windows Server 2008 R2   
  
-   新的 Windows Server 2008 林  
  
-   新的 Windows Server 2008 R2 林  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升级本机 Windows 2000 Active Directory 林中的功能级别  
在仅包含基于 Windows 2000 的域控制器的 Windows 2000 本机环境中，功能级别在默认情况下设置为以下级别，在这些级别上保持不变，直到你手动对其进行提升：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有林级和域级功能，必须将此 Windows 2000 环境升级到 Windows Server 2008 或 Windows Server 2008 R2。 可以通过以下方式之一执行此升级：  
  
-   将基于 Windows Server 2008 或 Windows Server 2008 R2 的域控制器引入到林中，然后停用所有运行 Windows 2000 的域控制器。  
  
-   执行将林中运行 Windows 2000 的所有现有域控制器到运行 Windows Server 2003 的域控制器的就地升级。 然后，将这些域控制器就地升级到 Windows Server 2008 或 Windows Server 2008 R2。 有关详细信息，请参阅[将 Active Directory 域升级到 Windows Server 2008 AD DS 域 \[LH @ no__t-2](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你的服务器运行的是基于 x64 版本的 Windows Server 2003，你可以成功地将此计算机的操作系统就地升级到 Windows Server 2008 R2。 如果你的服务器运行的是基于 x86 版本的 Windows Server 2003，则你无法将此计算机升级到 Windows Server 2008 R2。  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 域级功能而不将整个 Windows 2000 林升级到 Windows Server 2008 或 Windows Server 2008 R2，只会将域功能级别提升到 Windows Server 2008 或 Windows Server 2008R2.  
  
> [!NOTE]  
> 提升域功能级别之前，必须将该域中的所有基于 Windows 2000 的域控制器升级到 Windows Server 2008 或 Windows Server 2008 R2。  
  
将林中所有基于 Windows 2000 的域控制器替换为运行 Windows Server 2008 或 Windows Server 2008 R2 的域控制器后，可以将林功能级别提升到 Windows Server 2008 或 Windows Server 2008 R2。 这样做会自动引发林中所有域（设置为 Windows 2000 本机或更高版本）到 Windows Server 2008 或 Windows Server 2008 R2 的功能级别。  
  
有关提升林和域功能级别的详细信息，以及执行这些任务的过程的详细信息，请参阅[部署 Windows Server 2008 目录林根级域 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Active Directory 林中升级 Windows Server 2003 中的功能级别  
在仅包含基于 Windows Server 2003 的域控制器的 Windows Server 2003 环境中，默认情况下会将功能级别设置为以下级别，这些级别在这些级别上保持不变：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有林级和域级功能，必须将此 Windows Server 2003 环境升级到 Windows Server 2008 或 Windows Server 2008 R2。 可以通过以下方式之一执行此升级：  
  
-   将新安装的基于 Windows Server 2008 或 Windows Server 2008 的域控制器引入到林中，然后停用所有运行 Windows Server 2003 或升级到 Windows server 2008 或 Windows Server 2008 R2 的域控制器。  
  
-   对运行 windows server 2003 的所有现有域控制器执行就地升级，并将其部署到运行 Windows Server 2008 或 Windows Server 2008 R2 的域控制器。 有关详细信息，请参阅[将 Active Directory 域升级到 Windows Server 2008 AD DS 域 \[LH @ no__t-2](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你的服务器运行的是基于 x64 版本的 Windows Server 2003，你可以成功地将此计算机的操作系统就地升级到 Windows Server 2008 R2。 如果你的服务器运行的是基于 x86 版本的 Windows Server 2003，则你无法将此计算机升级为运行 Windows Server 2008 R2。  
  
若要使用所有 Windows Server 2008 或 Windows Server 2008 R2 域级功能，而无需将整个 Windows Server 2003 林升级到 Windows Server 2008 或 Windows Server 2008 R2，请仅将域功能级别提升到 Windows Server 2008 或 Windows Se) 2008 R2。  
  
> [!NOTE]  
> 提升域功能级别之前，必须将该域中的所有基于 Windows Server 2003 的域控制器升级到 Windows Server 2008 或 Windows Server 2008 R2。  
  
将林中所有基于 Windows Server 2003 的域控制器升级到 Windows Server 2008 或 Windows Server 2008 R2 后，可以将林功能级别提升到 Windows Server 2008 或 Windows Server 2008 R2。 这样做会自动引发林中所有域的功能级别，这些域设置为 Windows server 2003 到 Windows server 2008 或 Windows Server 2008 R2。  
  
有关提升林和域功能级别的详细信息，以及执行这些任务的过程的详细信息，请参阅[部署 Windows Server 2008 目录林根级域 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>在新的 Windows Server 2008 林中升级功能级别  
当你在新的 Windows Server 2008 林中安装第一个域控制器时，默认情况下会将功能级别设置为以下级别，它们将保持为这些级别，直到你手动对其进行提升：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
功能级别在这些默认级别上设置，可让你选择将基于 Windows 2000 或 Windows Server 2003 的域控制器添加到新的 Windows Server 2008 林。 创建目录林根级域后，你添加到 Windows Server 2008 林的每个域的域功能级别将设置为 Windows 2000 native。 但是，如果你希望新的 Windows Server 2008 环境中的所有域控制器都运行 Windows Server 2008，请在 fores 中安装第一个域控制器时，将林功能级别和域功能级别设置为 Windows Server 2008关心. 这样做可以节省时间，并启用 Windows Server 2008 中的所有林级和域级功能。  
  
> [!IMPORTANT]  
> 如果林在 Windows Server 2008 功能级别上运行，并且您尝试在基于 Windows Server 2003 的成员服务器或基于 Windows 2000 的成员服务器上安装 Active Directory，则安装将失败。  
  
有关提升林和域功能级别的详细信息，以及执行这些任务的过程的详细信息，请参阅[部署 Windows Server 2008 目录林根级域 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>在新的 Windows Server 2008 R2 林中升级功能级别  
当你在新的 Windows Server 2008 R2 林中安装第一个域控制器时，默认情况下会将功能级别设置为以下级别，它们将保持为这些级别，直到你手动对其进行提升：  
  
-   Windows Server 2003 域功能级别  
  
-   Windows Server 2003 林功能级别  
  
功能级别在这些默认级别上设置，可让你选择将基于 Windows Server 2003 的域控制器添加到新的 Windows Server 2008 R2 林。 创建目录林根级域后，你添加到 Windows Server 2008 R2 林的每个域的域功能级别设置为 Windows Server 2003。 但是，如果你希望新的 Windows Server 2008 R2 环境中的所有域控制器都运行 Windows Server 2008 R2，请将林功能级别和域功能级别设置为 Windows Server 2008 R2，当你在 yo 中安装第一个域控制器时林。 这样做可以节省时间，并在 Windows Server 2008 R2 中启用所有林级和域级功能。  
  
> [!IMPORTANT]  
> 如果林在 Windows Server 2008 R2 功能级别上运行，并且您尝试在基于 Windows Server 2008 或 Windows Server 2003 的成员服务器上或基于 Windows 2000 的成员服务器上安装 Active Directory，则安装将失败。  
  
有关提升林和域功能级别的详细信息，以及执行这些任务的过程的详细信息，请参阅[部署 Windows Server 2008 目录林根级域 \[LH @ no__t-2](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
> [!NOTE]  
> 尽管必须在 Windows Server 2008 上安装 ADMT 3.1 版，但你可以使用 ADMT 3.1 将对象迁移到由一个或多个 Windows Server 2008 R2 域控制器托管的域。 有关详细信息，请参阅 Microsoft 知识库中的[文章 976659](https://go.microsoft.com/fwlink/?LinkId=180398) （ https://go.microsoft.com/fwlink/?LinkId=180398) 。  
  


