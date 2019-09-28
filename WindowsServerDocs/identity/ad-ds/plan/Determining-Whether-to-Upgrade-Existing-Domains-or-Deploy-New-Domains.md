---
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: 确定是升级现有域还是部署新域
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0effdb3ae3ba6294e8a28f4f6b780f4d0c6a8582
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402614"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>确定是升级现有域还是部署新域

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

设计中的每个域都是新域或现有升级域。 必须将尚未升级的现有域中的用户移到新域中。  
  
在域之间移动帐户会影响最终用户。 在决定是将用户移动到新域还是升级现有域之前，请根据将用户移动到域中的成本评估新 AD DS 域的长期管理优势。  
  
有关将 Active Directory 域升级到 Windows Server 2008 的详细信息，请参阅[将 Active Directory 域升级到 Windows server 2008 和 Windows server 2008 R2 AD DS 域](https://technet.microsoft.com/library/cc731188.aspx)。  
  
若要详细了解如何在林内和林之间重新构建 AD DS 域，请参阅 Active Directory 迁移工具版本3.1 迁移指南（[https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)）。  
  
要使工作表可以帮助你记录新域和升级域的计划，请从 Windows Server 2003 部署工具包的作业助手（[https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)）下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "域规划 "（DSSLOGI_5）。  
  


