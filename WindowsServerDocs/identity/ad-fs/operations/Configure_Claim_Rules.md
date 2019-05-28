---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: 配置声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7eedd907c07c2aaef1670c5db3a6892ca3e650d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189627"
---
# <a name="configure-claim-rules"></a>配置声明规则

在声明中\-基于的标识模型中，联合身份验证服务作为 Active Directory 联合身份验证服务 (AD FS) 的功能是颁发令牌，其中包含一组声明。 声明规则控制 AD FS 颁发的声明方面的决策。 声明规则和数据存储在 AD FS 配置数据库中的所有服务器配置。  
  
AD FS 做出颁发决策基于为其声明的形式提供的标识信息和其他上下文信息。 在高级别中，AD FS 会像通过让其中一个规则处理器的声明集作为输入、 执行多种转换，并返回一组不同的声明作为输出。 

以下主题将帮助你创建 AD FS 将处理的规则： 
  
-   [创建规则，以传递或筛选传入声明](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [创建规则以允许所有用户](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [创建规则以根据传入声明允许或拒绝用户](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [创建规则以声明方式发送组成员身份](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [创建规则以转换传入声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [创建规则以发送身份验证方法声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [创建规则以使用自定义规则发送声明](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
