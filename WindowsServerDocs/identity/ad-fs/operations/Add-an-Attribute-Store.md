---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: 添加属性存储
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0f5c9d3b0f856ab72a16930ddb5c50686d747ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358395"
---
# <a name="add-an-attribute-store"></a>添加属性存储


需要访问受 Active Directory 联合身份验证服务 @no__t 的资源的用户帐户和计算机帐户将存储在属性存储区中，如 Active Directory 域服务 \(AD DS @ no__t。 声明颁发引擎使用属性存储来收集发出声明所必需的数据。 然后，将属性存储中的数据投影为声明。  
  
你可以使用以下过程将属性存储添加到联合身份验证服务。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-an-attribute-store"></a>添加属性存储  
  
1.  打开**AD FS 管理**。  
  
2.  在 "**操作**" 下，单击 "**添加属性存储**"。  

![添加属性存储](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. 在 "**添加属性存储**" 对话框中，为要添加的属性存储配置以下属性：  
  
   -   在 "**显示名称**" 中，键入要用于标识属性存储的名称。  
  
   -   在 "**属性存储类型**" 中，选择支持的属性存储类型（ **Active Directory**、 **LDAP**或**SQL**）。  
  
   -   在 "**连接字符串**" 中，如果选择了轻型目录访问协议 \(LDAP @ no__t-2 存储或结构化查询语言 \(SQL @ no__t 存储，请输入用于建立与属性的连接的字符串。店. 对于 Active Directory 属性存储，无需连接字符串;因此，此字段处于禁用状态。  
  
       > [!NOTE]  
       > 默认情况下，AD FS 会自动创建 Active Directory 属性存储。  
 
![添加属性存储](media/Add-an-Attribute-Store/addstore2.PNG) 

4. 单击 **“确定”** 。  
  
## <a name="additional-references"></a>其他参考  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
