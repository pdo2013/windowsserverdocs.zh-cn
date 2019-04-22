---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: 标识功能级别升级
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812038"
---
# <a name="identifying-your-functional-level-upgrade"></a>标识功能级别升级

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

你可以将提升域和林功能级别之前，必须评估当前环境并确定最符合组织的需求的功能级别要求。 通过标识了林中域位于每个域、 操作系统和 service pack 的每个域控制器正在运行和你计划升级域的日期中的域控制器来评估当前环境控制器。 如果你打算停用的域控制器，请确保您了解执行此操作将对你的环境的全面影响。  
  
在以下情况下可能会阻止您升级到 Windows Server 2008 或 Windows Server 2008 R2 功能级别的早期版本的 Windows Server 操作系统：  
  
-   硬件不足  
  
-   运行防病毒程序不兼容的 Windows Server 2008 或 Windows Server 2008 R2 的域控制器   
  
-   使用特定于版本的程序，但不在 Windows Server 2008 或 Windows Server 2008 R2 上运行   
  
-   需要升级具有最新 service pack 的程序  
  
此信息可以帮助你确定要采取确保已经有一个完全正常运行的 Windows Server 2008 或 Windows Server 2008 R2 环境的步骤。  
  
评估当前环境后，您必须确定适用于你的组织功能级别升级。 这些选项才可用：  
  
-   Windows Server 2008 或 Windows Server 2008 R2 到 Windows 2000 纯模式环境   
  
-   Windows Server 2008 或 Windows Server 2008 R2 到 Windows Server 2003 林   
  
-   新的 Windows Server 2008 林  
  
-   新的 Windows Server 2008 R2 林  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>升级中的本机 Windows 2000 Active Directory 林功能级别  
在 Windows 2000 本机环境中仅包含的基于 Windows 2000 的域控制器，功能级别设置为以下级别中，默认情况下，它们始终在这些级别手动提升它们之前：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有林级别和域级功能，必须将此 Windows 2000 环境升级到 Windows Server 2008 或 Windows Server 2008 R2。 您可以通过以下方式之一执行此升级：  
  
-   引入新安装的 Windows Server 2008-基于或 Windows Server 2008 R2 的基于到林中，域控制器，然后停用所有运行 Windows 2000 的域控制器。  
  
-   执行就地升级到域控制器运行 Windows Server 2003 林中运行 Windows 2000 的所有现有域控制器。 然后，执行这些域控制器到 Windows Server 2008 或 Windows Server 2008 R2 就地升级。 有关详细信息，请参阅[到 Windows Server 2008 AD DS 域升级 Active Directory 域\[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你的服务器正在运行 Windows Server 2003 的基于 x64 的版本，可以成功执行此计算机的操作系统为 Windows Server 2008 R2 就地升级。 如果你的服务器正在运行 Windows Server 2003 的基于 x86 的版本，您无法升级到 Windows Server 2008 R2 的此计算机。  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 域级别的功能无需升级到 Windows Server 2008 或 Windows Server 2008 R2 在整个 Windows 2000 林中，仅域功能级别提升到 Windows Server 2008 或 Windows Server 2008R2。  
  
> [!NOTE]  
> 提升域功能级别之前，你必须升级到 Windows Server 2008 或 Windows Server 2008 R2 的所有基于 Windows 2000 的域控制器，该域中。  
  
在林中的所有基于 Windows 2000 的域控制器替换运行 Windows Server 2008 或 Windows Server 2008 R2 的域控制器后，可以提升到 Windows Server 2008 或 Windows Server 2008 R2 林功能级别。 因此自动引发所有林中的域设置为 Windows 2000 本机或更高版本到 Windows Server 2008 或 Windows Server 2008 R2 的功能的级别。  
  
提升林和域功能级别，有关详细信息和过程来执行这些任务，请参阅[部署 Windows Server 2008 目录林根域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>升级在 Windows Server 2003 Active Directory 林功能级别  
在 Windows Server 2003 环境中只有基于 Windows Server 2003 的域控制器组成，功能级别设置为以下级别中，默认情况下，它们始终在这些级别手动提升它们之前：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
若要使用 Windows Server 2008 或 Windows Server 2008 R2 中的所有林级别和域级功能，必须将此 Windows Server 2003 环境升级到 Windows Server 2008 或 Windows Server 2008 R2。 您可以通过以下方式之一执行此升级：  
  
-   引入新安装的 Windows Server 2008-基于或 Windows Server 2008 R2 的基于域控制器到林，然后停用所有运行 Windows Server 2003 的域控制器或将其升级到 Windows Server 2008 或 Windows Server 2008 R2。  
  
-   执行就地升级到运行 Windows Server 2008 或 Windows Server 2008 R2 的域控制器运行 Windows Server 2003 的所有现有域控制器。 有关详细信息，请参阅[到 Windows Server 2008 AD DS 域升级 Active Directory 域\[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61)。  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 是基于 x64 的操作系统。 如果你的服务器正在运行 Windows Server 2003 的基于 x64 的版本，可以成功执行此计算机的操作系统为 Windows Server 2008 R2 就地升级。 如果你的服务器正在运行 Windows Server 2003 的基于 x86 的版本，无法升级此计算机运行 Windows Server 2008 R2。  
  
若要使用无需升级到 Windows Server 2008 或 Windows Server 2008 R2 在整个 Windows Server 2003 目录林中的所有 Windows Server 2008 或 Windows Server 2008 R2 域级别的功能，仅域功能级别提升到 Windows Server 2008 或 Windows Server 2008 R2。  
  
> [!NOTE]  
> 提升域功能级别之前，你必须升级到 Windows Server 2008 或 Windows Server 2008 R2 的所有基于 Windows Server 2003 的域控制器，该域中。  
  
在林中的所有基于 Windows Server 2003 的域控制器升级到 Windows Server 2008 或 Windows Server 2008 R2 之后，可以提升到 Windows Server 2008 或 Windows Server 2008 R2 林功能级别。 因此自动引发所有林中的域设置为 Windows Server 2003 到 Windows Server 2008 或 Windows Server 2008 R2 的功能的级别。  
  
提升林和域功能级别，有关详细信息和过程来执行这些任务，请参阅[部署 Windows Server 2008 目录林根域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>升级中新的 Windows Server 2008 林功能级别  
在新的 Windows Server 2008 林中安装第一个域控制器时，功能级别设置为以下级别中，默认情况下，它们保留在这些级别手动提升它们之前：  
  
-   Windows 2000 本机域功能级别  
  
-   Windows 2000 林功能级别  
  
在这些默认级别设置功能级别为您提供将 Windows 2000 或基于 Windows Server 2003 的域控制器添加到新的 Windows Server 2008 林的选项。 创建目录林根域后，您将添加到 Windows Server 2008 林的每个域的域功能级别设置为 Windows 2000 本机。 但是，如果想在新的 Windows Server 2008 环境以运行 Windows Server 2008 中的所有域控制器，林功能级别，设置，然后设置域功能级别，为 Windows Server 2008 时你 fores 中安装第一个域控制器t。 执行此操作可节省时间，并启用 Windows Server 2008 中的所有林级别和域级别功能。  
  
> [!IMPORTANT]  
> 如果在 Windows Server 2008 功能进行操作林级别和您尝试在基于 Windows Server 2003 的成员服务器上安装 Active Directory 或基于 Windows 2000 的成员服务器，安装将失败。  
  
提升林和域功能级别，有关详细信息和过程来执行这些任务，请参阅[部署 Windows Server 2008 目录林根域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>升级中新的 Windows Server 2008 R2 林功能级别  
当在新的 Windows Server 2008 R2 林中安装第一个域控制器时，功能级别设置为以下级别中，默认情况下，并且它们保持在这些级别手动提升它们之前：  
  
-   Windows Server 2003 域功能级别  
  
-   Windows Server 2003 林功能级别  
  
在这些默认级别设置功能级别为您提供将基于 Windows Server 2003 的域控制器添加到新的 Windows Server 2008 R2 林的选项。 创建目录林根域后，您将添加到 Windows Server 2008 R2 林的每个域的域功能级别设置为 Windows Server 2003。 但是，如果想在新的 Windows Server 2008 R2 环境以运行 Windows Server 2008 R2 中的所有域控制器，林功能级别，设置，然后设置域功能级别，为 Windows Server 2008 R2 中安装的第一个域控制器在 yo 时你的林。 执行此操作可节省时间，并启用 Windows Server 2008 R2 中的所有林级别和域级别功能。  
  
> [!IMPORTANT]  
> 如果在林中运行 Windows Server 2008 R2 功能级别，尝试进行 Windows Server 2008 上安装 Active Directory-基于或基于 Windows Server 2003 的成员服务器，或在基于 Windows 2000 的成员服务器上，则安装将失败。  
  
提升林和域功能级别，有关详细信息和过程来执行这些任务，请参阅[部署 Windows Server 2008 目录林根域\[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1)。  
  
> [!NOTE]  
> 虽然必须在 Windows Server 2008 上安装 ADMT v3.1，但可以使用 ADMT 3.1 版将迁移到由一个或多个 Windows Server 2008 R2 域控制器承载域的对象。 有关详细信息，请参阅[一文 976659](https://go.microsoft.com/fwlink/?LinkId=180398)在 Microsoft 知识库文章 (https://go.microsoft.com/fwlink/?LinkId=180398)。  
  


