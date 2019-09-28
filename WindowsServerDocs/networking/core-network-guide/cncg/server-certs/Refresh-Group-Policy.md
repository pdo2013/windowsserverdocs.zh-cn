---
title: 刷新组策略
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 88d39f41f61ae7c7f6a1fb84aa99806c4796c8cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356185"
---
# <a name="refresh-group-policy"></a>刷新组策略

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用此过程在本地计算机上手动刷新组策略。 刷新组策略时，如果已配置证书自动注册并且正常运行，则证书颁发机构（CA）会自动注册本地计算机的证书。  
  
> [!NOTE]  
> 重新启动域成员计算机或用户登录域成员计算机时，将自动刷新组策略。 此外，组策略会定期刷新。 默认情况下，此定期刷新每90分钟执行一次，随机偏移最多30分钟。  
  
**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>刷新本地计算机上的组策略  
  
1.  在安装了 NPS 的计算机上，使用任务栏上的图标打开 Windows PowerShell @ no__t-0。  
  
2.  在 Windows PowerShell 提示符下，键入**gpupdate**，然后按 enter。  
  


