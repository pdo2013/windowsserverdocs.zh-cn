---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: 了解 AD DS 设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828788"
---
# <a name="understanding-ad-ds-design"></a>了解 AD DS 设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

组织可以使用 Windows Server 中 Active Directory 域服务 (AD DS) 来创建可缩放、 安全及可管理基础结构时简化用户和资源管理。 可以使用 AD DS 管理网络基础结构，包括分支机构、 Microsoft Exchange Server 和多个林环境。  
  
AD DS 部署项目涉及到三个阶段： 设计阶段、 部署阶段和操作阶段。 在设计阶段，设计团队创建最符合组织将使用目录服务中的每个部门的需求的 AD DS 逻辑结构的设计。 设计经过批准后，部署团队对设计进行测试的实验室环境中，然后在生产环境中实现设计。 由于测试由部署团队执行，并且它可能会影响设计阶段，它是重叠的设计和部署的期间性活动。 部署完成后，操作团队负责维护目录服务。  
  
尽管本指南中介绍的 Windows Server AD DS 设计和部署策略都基于大量的实验室和试验计划测试以及在客户环境中的成功实现，您可能必须自定义你的 AD DS 设计和若要更好地满足特定的复杂环境的部署。
  
- 有关分支机构环境中部署 AD DS 的详细信息，请参阅[只读域控制器 (RODC) 分支机构计划指南](https://go.microsoft.com/fwlink/?LinkId=100207)。  
- 有关 Exchange 环境中部署 AD DS 的详细信息，请参阅文章[Exchange 2007 的规划 Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904)。  
- 有关多林环境中部署 AD DS 的详细信息，请参阅文章[Windows 2000 和 Windows Server 2003 中的多个林注意事项](https://go.microsoft.com/fwlink/?LinkId=88905)。  
