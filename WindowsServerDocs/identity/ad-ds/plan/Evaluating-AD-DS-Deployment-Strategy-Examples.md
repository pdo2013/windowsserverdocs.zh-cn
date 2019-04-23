---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 评估 AD DS 部署策略示例
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 169d0a55f9fb167390c13ac1c89f8d68427f318d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842108"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>评估 AD DS 部署策略示例

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

请考虑下面的示例的虚构公司 Contoso Pharmaceuticals，在其环境中部署 Active Directory 域服务 (AD DS)。 Contoso 环境由四个域组成。 林功能级别是 Windows Server 2003。 下图显示了 Contoso 组织的当前域结构。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
在查看其现有环境并且确定部署目标后, Contoso 建立了以下 AD DS 部署策略：  
  
-   Windows Server 2003 域升级到 Windows Server 2008 的域。  
  
-   通过提升到 Windows Server 2008 域和林功能级别启用高级的 AD DS 功能。  
  
-   重组 africa.concorp.contoso.com 林中的域中可以将该域用于 emea.concorp.contoso.con 域合并。  
  
林功能级别提升到 Windows Server 2008 将允许 Contoso 充分利用新的 AD DS 功能。 在林内重构域林下, 图中所示将减少管理所必需的管理域的量。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


