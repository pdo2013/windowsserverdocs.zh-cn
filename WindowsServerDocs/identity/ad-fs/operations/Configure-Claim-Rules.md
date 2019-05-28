---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: 配置声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d0dd8528e5fbd6829b313a3e6bc47f5a17f6a12f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189723"
---
# <a name="configure-claim-rules"></a>配置声明规则

在声明\-基于的标识模型、 Active Directory 联合身份验证服务的函数\(AD FS\)作为联合身份验证服务是颁发令牌，其中包含一组声明。 声明规则控制 AD FS 颁发的声明方面的决策。 声明规则和数据存储在 AD FS 配置数据库中的所有服务器配置。  
  
AD FS 做出颁发决策基于为其声明的形式提供的标识信息和其他上下文信息。 在高级别中，AD FS 会像通过让其中一个规则处理器的声明集作为输入、 执行多种转换，并返回一组不同的声明作为输出。 

以下主题将帮助你创建 AD FS 将处理的规则： 
  
-   [创建规则，以传递或筛选传入声明](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [创建规则以允许所有用户](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [创建规则以根据传入声明允许或拒绝用户](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [创建规则以声明方式发送 LDAP 属性](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [创建规则以声明方式发送组成员身份](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [创建规则以转换传入声明](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [创建规则以发送身份验证方法声明](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [创建一个规则以发送 AD FS 1.x 兼容声明](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [创建规则以使用自定义规则发送声明](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
