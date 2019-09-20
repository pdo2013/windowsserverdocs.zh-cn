---
title: 限制 Web 访问
description: 了解如何在 MultiPoint Services 中限制用户对 Internet 的访问权限
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 044f2fd5-5b87-42bb-ba0d-c06516ac48c8
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9f3524261e8e93439ff48a3e6666fa7680a76a29
ms.sourcegitcommit: ccec91c1d32a978159f9b8bb5e39ead5805c26c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71143739"
---
# <a name="limit-web-access"></a>限制 Web 访问
除了监视单个桌面上的用户活动，作为管理用户，你还可以通过指示要阻止用户访问的允许网站和网站，限制用户对指定网站的访问权限。  
  
## <a name="to-limit-web-access-on-a-station"></a>在工作站上限制 Web 访问  
  
1. 在 MultiPoint 仪表板中的 " **Web 限制**" 选项卡上，单击 "**配置**"。 **配置 Web 限制**页将打开。 列出了用户可以访问的站点。  
  
2. 单击要在其上限制 Web 访问的用户工作站的缩略图。  
  
3. 在**选定项任务**之下，单击**在此工作站上限制 Web 访问**。 **配置 Web 限制**页将打开。 列出了用户可以访问的站点。  
  
4. 若要添加允许的站点，请键入 Web 地址，然后单击**添加**。  
  
   > [!NOTE]
   > 例如，输入 "Contoso.com" 允许或阻止相对于 www\.contoso.com 的站点（例如 www\.newpage.contoso.com）。 输入 "Contoso" 将允许或限制与 Contoso 相关的所有站点（包括 contoso.com、contoso.uk，等等）。  
  
5. 若要从允许的站点列表中删除 Web 地址，请单击要删除对其访问权限的 Web 地址，然后单击**删除**。  
  
## <a name="to-limit-web-access-on-all-stations"></a>在所有工作站上限制 Web 访问  
  
1. 在 MultiPoint 仪表板中的 " **Web 限制**" 选项卡上，\-单击 "开始" 下拉菜单，然后单击 "**限制所有桌面上的 Web 访问**"。  
  
   **配置 Web 限制**页将打开。 列出了用户可以访问的站点。 执行下列操作之一：  
  
2. 若要添加允许的站点，请单击**仅允许这些站点**，键入允许的 Web 地址，然后单击**添加**。  
  
   若要添加不希望用户访问的站点，请单击 "**仅禁止这些网站**"，键入不希望用户访问的 web 地址，然后单击 "**添加**"。  
  
   > [!NOTE]
   > 例如，输入 "Contoso.com" 允许或阻止相对于 www.contoso.com 的站点（例如 www\.newpage.contoso.com）。 输入 "Contoso" 将允许或限制与 Contoso 相关的所有站点（包括 contoso.com、contoso.uk，等等）。  
  
3. 若要从允许或禁止的站点列表中删除 Web 地址，请单击该 Web 地址，然后单击**删除**。  
  
## <a name="see-also"></a>请参阅  
[管理用户桌面](manage-user-desktops-using-multipoint-dashboard.md)  
