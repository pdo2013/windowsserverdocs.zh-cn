---
title: AD 林恢复-清理
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868958"
---
# <a name="ad-forest-recovery---cleanup"></a>AD 林恢复-清理

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 根据需要请执行以下恢复后步骤：  
  
- 恢复整个林后，你可以还原到原始的 DNS 配置，包括每个域控制器上的首选和备用 DNS 服务器的配置。 DNS 服务器进行配置之前不正常工作后，将还原其以前的名称解析功能。 适用于未恢复的 Dc 中删除任何 DNS 记录。  
- 删除未恢复的所有 Dc 的 Windows Internet 名称服务 (WINS) 记录。  
- 可以将操作主机角色传输到其他域或林中的域控制器，并添加更多全局编录服务器基于在发生故障之前的配置。  
- 因为整个林还原到以前的状态，已添加任何对象 （如用户和计算机） 以及此点后，对现有对象所做的所有更新 （例如密码更改） 都将丢失。 因此，应重新创建这些缺少的对象，并重新应用根据缺少的更新。  
- 您可能还需要恢复传出信任关系与外部域和林，因为这些外部信任关系将不会自动从备份还原。

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)  
