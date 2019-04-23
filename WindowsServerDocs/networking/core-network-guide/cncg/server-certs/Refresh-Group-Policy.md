---
title: 刷新组策略
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863448"
---
# <a name="refresh-group-policy"></a>刷新组策略

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用此过程来手动刷新组策略在本地计算机上。 刷新组策略时，如果配置证书自动注册并正常运行，本地计算机会自动注册证书颁发机构 (CA) 证书。  
  
> [!NOTE]  
> 当重新启动域成员计算机，或者当用户登录到域成员计算机自动刷新组策略。 此外，定期刷新组策略。 默认情况下，具有最多 30 分钟的随机的偏移量每 90 分钟执行此定期刷新。  
  
中的成员身份**管理员**，或等效身份是完成此过程所需的最小。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>若要在本地计算机上刷新组策略  
  
1.  在安装了 NPS 的计算机上打开 Windows PowerShell&reg;在任务栏上使用的图标。  
  
2.  在 Windows PowerShell 提示符处，键入**gpupdate**，然后按 ENTER。  
  


