---
title: 步骤 3 验证部署
description: 本主题是指南的在 Windows Server 2016 中远程管理 DirectAccess 客户端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 89b9e53b7321ebeddda50a448829ad73395eeed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835608"
---
# <a name="step-3-verify-the-deployment"></a>步骤 3 验证部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何验证正确配置了部署的远程管理 DirectAccess 客户端。  
  
### <a name="to-verify-proper-deployment"></a>若要验证适当的部署  
  
1.  DirectAccess 客户端计算机连接到公司网络并获取组策略对象。  
  
2.  在客户端计算机上，单击**网络连接**中要访问 DirectAccess 媒体管理器的通知区域图标。  
  
3.  单击**DirectAccess 连接**，你会看到状态是**本地连接**。  
  
4.  从企业网络中删除计算机并将其连接到公用网络。  
  
5.  在命令提示符下键入**nltest /dsgetdc: [完全限定的域名]**。 此命令将验证向客户端可以访问公司网络。 如果域控制器不可访问，将显示以下错误消息，报告的域不存在：ERROR_NO_SUCH_DOMAIN。  
  


