---
title: 管理 Windows Server Essentials 中的 SharePoint Online
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b9b12c138e6166684b4b9e87b794444febd3c247
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830378"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 SharePoint Online

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以管理 SharePoint Online 库和团队网站仪表板中，而无需登录到 Office 365，如果将 Office 365 与 Windows Server Essentials 服务器集成。 您获得 SharePoint Online 库和团队网站与任何 Office 365 企业版计划。 [了解如何将 Office 365 与你的服务器集成](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
 另一个好处是，用户将能够使用 My Server 2012 R2 应用从任意位置使用其移动设备或 Windows phone 访问 SharePoint Online 库中的文件。 [在哪里可以获得 My Server 应用？](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
 尚未尝试使用 SharePoint？ [现在可以执行的操作](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>将在仪表板的哪个位置管理我的库和团队网站？  
 将使用新**SharePoint Online**选项卡上，添加到**存储**仪表板将 Office 365 与服务器来管理 SharePoint Online 资源集成的区域。  

  
## <a name="what-can-i-manage-from-the-dashboard"></a>可以从仪表板管理哪些内容？  
  
### <a name="manage-your-online-libraries"></a>管理联机的库  
   
|-|-|  
|添加库 |上**SharePoint 库**选项卡上，使用**添加库**。 你将能够进行所有常用选择：<br /><br /> -选择团队站点和库类型。<br />-确定是否使用版本控制。<br />-分配访问权限。<br /><br /> **更改暂存文件夹路径**若要了解如果没有分配权限，你的库将继承哪些团队网站权限，请使用**查看站点权限**。 |  
|打开库 |若要使用的库内容，您将需要在 Office 365 中打开它。 只需选择库并单击“打开库” 。 可以使用内容执行的操作将取决于你用于登录到 SharePoint Online 的凭据。 |  
|更改版本控制或访问权限 |可以使用**查看库属性**来查看或更改的版本控制或访问库的权限。 |  
|删除库 |**警告：** 在删除 SharePoint Online 库之前，请务必保存要保留到其他位置的任何文件。 从 SharePoint 删除库时，所有内容将永久删除。 无法检索任何内容。<br /><br /> 检查以确保库中未存储任何内容都需要更高版本后，选择库，然后单击**删除库**。 |  
  
### <a name="manage-your-team-sites"></a>管理团队网站  
 
|-|-|  
|管理 SharePoint 团队网站 |**管理团队网站**操作，可以登录到 Office 365 和管理 SharePoint Online 团队网站。 可以在 Office 365 中执行的操作取决于你用于登录的联机帐户。<br /><br /> 当您关闭 Office 365，并返回到仪表板时，单击**刷新**以显示所做的更改。 |查看或更改团队网站权限 |由于库从它的团队网站中继承权限，默认情况下，最好能够轻松地访问团队网站。 若要查看或更改团队网站的权限，请选择该团队网站或任何库，然后单击**查看站点权限**。<br /><br /> **更改暂存文件夹路径**需要有关 SharePoint 团队网站权限细节的帮助吗？ “团队网站权限”中有一个实用的 [了解详细信息](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) 链接。  
  
## <a name="tips"></a>提示  
  
-   **单击刷新以显示最新 Office 365 门户中所做的更改。** 你将需要后打开 Office 365 来管理 SharePoint Online 刷新显示。 如果使仪表板长期保持打开状态，则请单击“刷新”以确保你看到的是最新更改。  
  
-   **可以在 SharePoint Online 中执行的操作取决于你使用仪表板上或在 Office 365 中。** 在仪表板上会使用用于 Office 365 集成的管理员帐户进行 SharePoint Online 的更改。 但当你登录到 Office 365 从仪表板，你使用的联机帐户的访问权限确定可以执行哪些操作。  
  
     若要了解 Office 365 集成的管理员帐户，请打开**Office 365**仪表板上的选项卡。  
  
## <a name="other-things-you-might-want-to-do"></a>你可能希望执行的其他操作  
  
-   [使用 My Server 应用可以使用 SharePoint Online 库中从任何位置](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
-   [了解有关为 SharePoint 团队网站分配权限的详细信息](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)  
  
-   [了解如何使用 SharePoint 功能](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
-   [看看有 Office 365 企业版计划](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)  
  
-   [将 Office 365 与你的服务器集成](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [从仪表板管理其他 Microsoft online services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
