---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 评估 AD DS 部署策略示例
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3d04530c53150a3222b609a80938d7fdfcdfeff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402583"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>评估 AD DS 部署策略示例

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

请考虑以下示例： Contoso 药物在其环境中部署 Active Directory 域服务（AD DS）。 Contoso 环境由四个域组成。 林功能级别为 Windows Server 2003。 下图显示 Contoso 组织的当前域结构。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
查看其现有环境并确定其部署目标后，Contoso 建立了以下 AD DS 部署策略：  
  
-   将 Windows Server 2003 域升级到 Windows Server 2008 域。  
  
-   通过将域和林功能级别提升到 Windows Server 2008，启用高级 AD DS 功能。  
  
-   重新构建林中的 africa.concorp.contoso.com 域，以将该域与 concorp 域合并在一起。  
  
将林功能级别提升到 Windows Server 2008 将使 Contoso 充分利用新的 AD DS 功能。 在林中重新构建域（如下图所示），将减少管理域所需的管理量。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


