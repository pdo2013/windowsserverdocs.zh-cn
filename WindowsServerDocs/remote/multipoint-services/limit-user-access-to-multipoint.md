---
title: 限制用户访问服务器
description: 了解如何允许或拒绝访问的 MultiPoint 服务的用户和组
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830808"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>限制对 Multipoint server 的用户的访问
是否将多点服务器加入到 Active Directory 域或使用本地用户帐户，所有用户默认情况下都有权访问多点服务器。 允许用户登录到 MultiPoint Services 环境中的工作站上之前，应限制对服务器的访问权限。  
  
Remote Desktop Users 组中的任何用户可以登录到 MultiPoint server。 默认情况下用户组每个人都是 Remote Desktop Users 组的成员，因此每个本地用户和域用户可以登录到 MultiPoint Server。 若要限制对 MultiPoint Server 的访问权限，请删除 Everyone 用户 Remote Desktop Users 组中，从组，然后将特定用户或组添加到 Remote Desktop Users 组。  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>添加或删除用户或组添加到 Remote Desktop Users 组  
  
1.  从**启动**屏幕上，打开**计算机管理**。  
  
2.  在控制台树中下,**本地用户和组**，单击**组**。  
  
3.  双击**Remote Desktop Users**，然后按照说明进行添加或删除用户。  
  
    -   若要限制对服务器的常规访问权限，删除 Everyone 组。  
  
    -   若要使工作站 MultiPoint server 的用户访问，添加到 Remote Desktop Users 组的每个本地帐户或每个域用户或组帐户。  