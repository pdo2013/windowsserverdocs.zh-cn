---
title: 将 MultiPoint 服务加入域（可选）
Description: 提供将 MultiPoint 服务加入到域的步骤
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 79bd9e594f94c7b3acd06265891dd646b3853b50
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394739"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>将 MultiPoint 服务计算机加入域（可选）
如果你将通过 Active Directory 域访问你的 MultiPoint 服务计算机，则下一步是将计算机添加到域。  
  
> [!IMPORTANT]  
> 在将计算机加入域之前，必须验证你的时区。 有关说明，请参阅[设置日期、时间和](Set-the-date--time--and-time-zone.md)时区。  
   
1.  从“开始” 屏幕打开“控制面板”。 单击 "**系统和安全**"，然后单击 "**系统**"。  
  
2.  在“计算机名、域和工作组设置”下，单击“更改设置”。  
  
3.  在 "**计算机名称**" 选项卡上，单击 "**更改**"。  
  
4.  在 "**计算机名/域更改**" 对话框中，选择 "**域**"，输入域的名称，然后单击 **"确定"** ，然后按照向导中的步骤完成此过程。  
  
5.  计算机重启后，以管理员身份登录并等待 MultiPoint 管理器打开。  
  
> [!IMPORTANT]  
> 若要确保 MultiPoint 服务域部署正常工作，需要配置几个组策略并更新注册表。 有关信息，请参阅为[域部署配置组策略](https://technet.microsoft.com/library/dn265982.aspx)。  