---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: "配置索赔规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="configure-claim-rules"></a>配置索赔规则

>适用于：Windows Server 2016，Windows Server 2012 R2

在 claims\ 基于身份模型，作为联合身份验证服务 Active Directory 联合身份验证服务 (广告 FS) 的功能是发出包含一组的索赔一个标记。 索赔规则控制与广告 FS 问题的索赔相关决策。 广告 FS 配置数据库中存储索赔规则和配置服务器的所有数据。  
  
广告 FS 可根据提供给它的索赔形式的身份信息以及其他上下文信息的颁发决策。 在较高级别广告 FS 表示按规则处理器通过让其中一个作为输入设置的索赔、 执行的许多转换，并返回的索赔输出为另一组。 

以下主题将帮助你创建处理广告 FS 将规则： 
  
-   [创建规则通过或筛选传入的声明](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [创建一个规则以允许所有用户](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [创建规则要允许或拒绝用户基于传入的声明](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [创建规则作为索赔发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [创建规则应用于发送组成员作为声明](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [创建规则转换传入的声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [创建发送身份验证方法索赔规则](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [创建发送索赔自定义规则的使用规则](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
