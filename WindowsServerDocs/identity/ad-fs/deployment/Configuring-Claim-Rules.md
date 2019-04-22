---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: 配置声明规则
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814658"
---
# <a name="configuring-claim-rules"></a>配置声明规则

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在声明\-基于的标识模型、 Active Directory 联合身份验证服务的函数\(AD FS\)作为联合身份验证服务是颁发令牌，其中包含一组声明。 声明规则控制在方面的 AD FS 颁发的声明的决定。 声明规则和数据存储在 AD FS 配置数据库中的所有服务器配置。  
  
AD FS 做出颁发决策基于为其声明的形式提供的标识信息和其他上下文信息。 在高级别中，AD FS 会像通过让其中一个规则处理器的声明集作为输入、 执行多种转换，并返回一组不同的声明作为输出。  
  
-   [创建规则，以传递或筛选传入声明](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [创建一个规则以允许所有用户](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [创建一个规则以发送 AD FS 1.x 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [创建一个规则以允许或拒绝用户根据传入声明](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [创建规则，以便以声明方式发送组成员身份](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [创建一个规则以转换传入声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [创建一个规则以发送身份验证方法声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [创建规则，以使用自定义规则发送声明](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>其他参考  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
