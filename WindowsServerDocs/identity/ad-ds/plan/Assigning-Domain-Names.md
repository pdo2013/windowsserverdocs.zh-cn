---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: 分配域名
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866558"
---
# <a name="assigning-domain-names"></a>分配域名

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在计划中，必须将一个名称分配给每个域。 Active Directory 域服务 (AD DS) 域具有两种类型的名称：域名系统 (DNS) 和 NetBIOS 名称。 一般情况下，两个名称都对最终用户可见。 Active Directory 域的 DNS 名称包括两个部分、 前缀和后缀。 创建域名称时，首先确定 DNS 前缀。 这是域的中的 DNS 名称的第一个标签。 选择林根级域的名称时，确定该后缀。 下表列出了命名规则的 DNS 名称的前缀。  
  
|规则|说明|  
|--------|---------------|  
|选择的前缀，则不可能变得过时。|避免使用如某个产品系列或将来可能会更改的操作系统的名称。 我们建议使用地理名称。|  
|选择包括仅限 Internet 标准字符的前缀。|A-Z、 a-z、 0-9、 和 （-），但不是完全数字。|  
|包含 15 个字符或更短的前缀中。|如果您选择的前缀长度小于或等于的 15 个字符，NetBIOS 名称是与该前缀相同。|  
  
有关详细信息，请参阅 Active Directory 中的计算机、 域、 站点和 Ou 的命名约定 ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629))。  
  
> [!NOTE]  
>  虽然 Windows Server 2008 和 Windows Server 2003 中的 Dcpromo.exe 可让你创建单标签的 DNS 域名，但由于几种原因，你不得对域使用单标签 DNS 域名。 在 Windows Server 2008 R2 中，Dcpromo.exe 不允许你为域创建单标签 DNS 域名。 有关详细信息，请参阅[ https://go.microsoft.com/fwlink/?LinkId=92467。](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
如果当前域的 NetBIOS 名称不适合用于表示的区域，或未能满足前缀命名规则，选择一个新的前缀。 在这种情况下，域的 NetBIOS 名称是不同于域的 DNS 前缀。  
  
对于你部署的每个新域，请选择适合的区域并满足前缀命名规则的前缀。 我们建议在域的 NetBIOS 名称是 DNS 前缀相同。  
  
记录的 DNS 前缀和选择用于在您的林中每个域的 NetBIOS 名称。 可以将 DNS 和 NetBIOS 名称信息添加到您创建的用于记录你计划对新的和升级域的"域规划"工作表。 若要打开它，请从作业辅助工具为 Windows Server 2003 部署工具包下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然后打开"域规划"(DSSLOGI_5.doc)。  
  


