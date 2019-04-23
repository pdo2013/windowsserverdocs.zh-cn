---
title: MultiPoint 服务加入域 （可选）
Description: 提供要加入到域的 MultiPoint 服务的步骤
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827038"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>将在 MultiPoint Services 计算机加入到域 （可选）
如果将 Active Directory 域上访问你的 MultiPoint Services 计算机下, 一步是将计算机添加到域。  
  
> [!IMPORTANT]  
> 将计算机加入到域之前，必须验证你的时区。 有关说明，请参阅[设置日期、 时间和时区](Set-the-date--time--and-time-zone.md)。  
   
1.  从“开始”  屏幕打开“控制面板” 。 单击**系统和安全**，然后单击**系统**。  
  
2.  在“计算机名、域和工作组设置” 下，单击“更改设置” 。  
  
3.  上**计算机名**选项卡上，单击**更改**。  
  
4.  在中**计算机名/域更改**对话框中，选择**域**，输入域的名称，然后单击**确定**，然后按照向导来完成中的步骤进行操作过程。  
  
5.  在计算机重新启动后，以管理员身份登录，并等待 MultiPoint 管理器以打开。  
  
> [!IMPORTANT]  
> 若要确保你的 MultiPoint Services 域部署工作正常，将需要配置几个组策略和更新注册表。 有关信息，请参阅[配置为域部署组策略](https://technet.microsoft.com/library/dn265982.aspx)。  