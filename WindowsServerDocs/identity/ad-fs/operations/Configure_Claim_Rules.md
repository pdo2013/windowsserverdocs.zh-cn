---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: 配置声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b46e228f202eeae7f8cbcf4c1a6851686f905e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407633"
---
# <a name="configure-claim-rules"></a>配置声明规则

在声明 @ no__t-0based 标识模型中，Active Directory 联合身份验证服务（AD FS）作为联合身份验证服务的功能是颁发包含一组声明的令牌。 声明规则管理有关 AD FS 问题的声明的决策。 声明规则和所有服务器配置数据均存储在 AD FS 配置数据库中。  
  
AD FS 根据声明和其他上下文信息的形式向其提供的标识信息进行颁发决策。 从较高层次来看，AD FS 将一组声明作为输入，执行多个转换，然后返回一组不同的声明作为一个规则处理器。 

以下主题将帮助你创建 AD FS 将处理的规则： 
  
-   [创建传递或筛选传入声明的规则](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [创建规则以允许所有用户](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [创建规则以根据传入声明允许或拒绝用户](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [创建规则以将 LDAP 属性作为声明发送](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [创建规则以声明方式发送组成员身份](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [创建规则以转换传入声明](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [创建规则以发送身份验证方法声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [创建规则以使用自定义规则发送声明](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
