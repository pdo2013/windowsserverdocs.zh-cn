---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: 分配域名
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 357c136f108c6d8e9e2a15dd9449ab61663079e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408994"
---
# <a name="assigning-domain-names"></a>分配域名

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您必须为您的计划中的每个域分配一个名称。 Active Directory 域服务（AD DS）域具有两种类型的名称：域名系统（DNS）名称和 NetBIOS 名称。 通常，这两个名称对最终用户可见。 Active Directory 域的 DNS 名称包括两个部分：前缀和后缀。 创建域名时，请首先确定 DNS 前缀。 这是域的 DNS 名称中的第一个标签。 此后缀是在选择目录林根级域的名称时确定的。 下表列出了 DNS 名称的前缀命名规则。  
  
|规则|说明|  
|--------|---------------|  
|选择不可能过时的前缀。|避免将来可能会更改的名称，如产品线或操作系统。 建议使用地理名称。|  
|选择仅包含 Internet 标准字符的前缀。|A-z、a-z、0-9 和（-），但并不完全是数字。|  
|前缀中包含15个或更少的字符。|如果选择的前缀长度不超过15个字符，则 NetBIOS 名称与前缀相同。|  
  
有关详细信息，请参阅 Active Directory 中的计算机、域、站点和 Ou 的命名约定（[https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)）。  
  
> [!NOTE]  
>  虽然 Windows Server 2008 和 Windows Server 2003 中的 Dcpromo.exe 可让你创建单标签的 DNS 域名，但由于几种原因，你不得对域使用单标签 DNS 域名。 在 Windows Server 2008 R2 中，Dcpromo.exe 不允许你为域创建单标签 DNS 域名。 有关详细信息，请参阅[https://go.microsoft.com/fwlink/?LinkId=92467 。](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
如果域的当前 NetBIOS 名称不适合表示区域，或者无法满足前缀命名规则，请选择新的前缀。 在这种情况下，域的 NetBIOS 名称不同于域的 DNS 前缀。  
  
对于你部署的每个新域，请选择适用于该区域并且满足前缀命名规则的前缀。 建议域的 NetBIOS 名称与 DNS 前缀相同。  
  
记录为林中每个域选择的 DNS 前缀和 NetBIOS 名称。 您可以将 DNS 和 NetBIOS 名称信息添加到您创建的 "域计划" 工作表中，以便记录新域和升级域的计划。 若要打开它，请从 Windows Server 2003 部署工具包的作业助手（[https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)）下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，并打开 "域计划" （DSSLOGI_5）。  
  


