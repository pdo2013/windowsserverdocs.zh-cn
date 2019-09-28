---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: 了解 AD DS 设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f46cad23e13edcef57bf00113e601069c02cfd4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402456"
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 设计

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

组织可以使用 Windows Server 中的 Active Directory 域服务（AD DS）来简化用户和资源管理，同时创建可缩放的、安全且可管理的基础结构。 你可以使用 AD DS 来管理网络基础结构，包括分支机构、Microsoft Exchange Server 和多林环境。  
  
AD DS 部署项目涉及三个阶段：设计阶段、部署阶段和操作阶段。 在设计阶段，设计团队为 AD DS 逻辑结构创建一个设计，该设计最符合组织中将使用目录服务的每个部门的需求。 在批准设计后，部署团队在实验室环境中测试设计，然后在生产环境中实现设计。 由于测试是由部署团队执行的，并且它可能会影响设计阶段，因此这是一个与设计和部署重叠的过渡活动。 部署完成后，运营团队负责维护目录服务。  
  
尽管本指南中所述的 Windows Server AD DS 设计和部署策略基于广泛的实验室和试验计划测试以及客户环境中的成功实施，但你可能需要自定义你的 AD DS 设计和部署以更好地适应特定复杂的环境。
  
- 有关在分支机构环境中部署 AD DS 的详细信息，请参阅[只读域控制器（RODC）分支机构规划指南](https://go.microsoft.com/fwlink/?LinkId=100207)。  
- 有关在 Exchange 环境中部署 AD DS 的详细信息，请参阅[exchange 2007-规划 Active Directory 一](https://go.microsoft.com/fwlink/?LinkId=88904)文。  
- 有关在多个林环境中部署 AD DS 的详细信息，请参阅[windows 2000 和 Windows Server 2003 中的多林注意事项](https://go.microsoft.com/fwlink/?LinkId=88905)一文。  
