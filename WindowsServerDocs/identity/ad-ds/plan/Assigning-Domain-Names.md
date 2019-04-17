---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: "分配域名"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>分配域名

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在你的套餐，你必须为每域分配一个名称。 Active Directory 域服务 (广告 DS) 域有两种类型的名称： 域名系统 (DNS) 和 NetBIOS 名称。 一般情况下，这两个名称是最终用户可见。 两个部分，前缀职务，包括 DNS 域 Active Directory 的名称。 创建域名称时，首先确定 DNS 前缀。 这是第一个 DNS 名称的域中标签。 职务确定当你选择的森林根域名称。 下表列出了前缀 DNS 名称命名规则。  
  
|规则|说明|  
|--------|---------------|  
|选择一个前缀，且不太可能时变为过期版本。|避免使用名称，如产品系列或可能在以后更改的操作系统。 我们建议使用地理名称。|  
|选择包含标准字符互联网前缀。|一个 Z、 一 z 0 到 9、 和 （-），但不是会完全数字。|  
|包含 15 字符一些或少一些前缀中。|如果你选择前缀长度 15 个字符的或更少，NetBIOS 名称是前缀相同。|  
  
有关详细信息，请参阅 Active Directory 中的计算机、域、网站和华丽绚烂的命名约定 ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629))。  
  
> [!NOTE]  
>  尽管在 Windows Server 2008 和 Windows Server 2003 Dcpromo.exe 允许您创建单标签 DNS 域名称，你不应以下几个原因使用的单个标签 DNS 域名称。 在 Windows Server 2008 R2、 Dcpromo.exe 不允许你创建的单标签 DNS 域名称。 有关详细信息，请参阅[https://go.microsoft.com/fwlink/?LinkId=92467。](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
如果的域的当前 NetBIOS 名称是不适宜代表地区或未能满足前缀命名规则，请选择新前缀。 在此情况下的域的 NetBIOS 名称是不同于 DNS 前缀的域。  
  
对于每个新域部署，选择适合地区并且满足前缀命名规则前缀。 我们建议的域的 NetBIOS 名称将 DNS 前缀相同。  
  
记录 DNS 前缀以及你选择的每个域你森林中的 NetBIOS 名称。 你可以添加到您创建以记录你的新升级后的域的套餐"域计划"表中的 DNS 和 NetBIOS 名称信息。 若要打开它，请下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 从工作辅助为 Windows Server 2003 部署工具包 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，然后打开"计划域"(DSSLOGI_5.doc)。  
  


