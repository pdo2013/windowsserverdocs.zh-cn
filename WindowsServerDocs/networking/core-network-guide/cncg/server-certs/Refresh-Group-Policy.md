---
title: 刷新组策略
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>刷新组策略

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此过程手动刷新本地计算机上的组策略。 刷新组策略后，如果配置自动证书的注册，并且运行正常，在本地计算机注册证书由未经认证颁发（加拿大）。  
  
> [!NOTE]  
> 当你重启域成员计算机，或用户在登录到的域成员计算机时，将自动刷新组策略。 此外，定期刷新组策略。 默认情况下，此定期刷新执行每个 90 分钟随机的达 30 分钟的时间偏移。  
  
在会员**管理员**，或等效的最低要求完成此过程。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>若要在本地计算机上刷新组策略  
  
1.  在装有 NPS 计算机上，打开 Windows PowerShell&reg;使用在任务栏上的图标。  
  
2.  在 Windows PowerShell 提示符下，键入**gpupdate**，然后按 ENTER。  
  


