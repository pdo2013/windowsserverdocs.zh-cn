---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: 添加属性存储
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837858"
---
# <a name="add-an-attribute-store"></a>添加属性存储

>适用于：Windows Server 2016, Windows Server 2012 R2

用户帐户和计算机帐户需要对受 Active Directory 联合身份验证服务资源的访问\(AD FS\)属性存储，如 Active Directory 域服务中存储\(AD DS\). 声明颁发引擎使用属性存储中收集数据所需发出声明。 从属性存储的数据然后投影以声明方式。  
  
可以使用以下过程将属性存储添加到联合身份验证服务。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
#### <a name="to-add-an-attribute-store"></a>若要添加属性存储  
  
1.  打开**AD FS 管理**。  
  
2.  下**操作**单击**添加属性存储**。  

![添加属性存储](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  在中**添加属性存储**对话框框中，配置想要添加的属性存储的以下属性：  
  
    -   在中**显示名称**，键入想要用于标识属性存储的名称。  
  
    -   在**属性存储区类型**，选择受支持的属性存储类型，即**Active Directory**， **LDAP**，或者**SQL**。  
  
    -   在中**连接字符串**，如果选择了任一轻型目录访问协议\(LDAP\)应用商店或结构化查询语言\(SQL\)存储，请输入字符串用于建立与属性存储的连接。 对于 Active Directory 属性存储的连接字符串是必需的;因此，此字段已禁用。  
  
        > [!NOTE]  
        > 默认情况下，AD FS 会自动创建 Active Directory 属性存储。  
 
![添加属性存储](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  单击 **“确定”**。  
  
## <a name="additional-references"></a>其他参考  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[属性存储的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
