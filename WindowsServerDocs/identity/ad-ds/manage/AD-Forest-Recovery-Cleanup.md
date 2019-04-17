---
title: "广告森林恢复-清理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>广告森林恢复-清理 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 根据需要请执行以下文章恢复步骤：  
  
-   恢复整个森林之后，可以恢复为原始 DNS 配置，包括配置每个域控制器上的首选和备用 DNS 服务器。 前故障配置了在 DNS 服务器后，将恢复其以前名称分辨率功能。 对于尚未恢复的 Dc 删除任何 DNS 记录。  
  
-   对于尚未恢复的所有 Dc 删除 Windows Internet 名称服务 (WINS) 记录。  
  
-   你可以传输到其他 Dc 域或森林中的操作主机角色并添加更多全球目录服务器基于之前故障配置。  
  
-   整个森林还原到以前的状态，因为已添加的任何物体（例如，用户和计算机）和对现有的对象此后所做的所有更新（诸如密码更改）都将丢失。 因此，你应该重新创建找对象，并重新根据缺失的更新的应用。  
  
-   你可能还需要还原传出信任使用外部域和森林，因为这些外部信任关系都不会自动从备份还原。

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)  



