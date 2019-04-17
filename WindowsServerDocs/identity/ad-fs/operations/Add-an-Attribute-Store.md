---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: "添加属性应用商店"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>添加属性应用商店

>适用于：Windows Server 2016，Windows Server 2012 R2

用户帐户和需要访问受 Active Directory 联合身份验证服务 \(AD FS\) 资源的计算机帐户在特性商店中，如 Active Directory 域服务 \(AD DS\) 存储。 索赔颁发引擎使用特性官方商城发出索赔所需的数据的收集。 然后作为索赔投影特性官方商城中的数据。  
  
可以使用下面的过程添加到联合身份验证服务属性应用商店。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
#### <a name="to-add-an-attribute-store"></a>若要添加的特性官方商城  
  
1.  打开**广告 FS 管理**。  
  
2.  下**操作**单击**添加特性存储**。  

![添加特性官方商城](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  在**添加特性存储**对话框中，将配置你想要添加的特性官方商城的以下属性：  
  
    -   在**显示名称**，键入你想要使用找出特性官方商城的名称。  
  
    -   在**特性官方商城类型**，选择受支持的特性商店类型，则**Active Directory**，**LDAP**，或**SQL**。  
  
    -   在**连接字符串**，如果您选择轻型目录访问协议 \(LDAP\) 应用商店或结构化查询语言 \(SQL\) 应用商店中，输入你用于建立连接特性官方商城的字符串。 对于 Active Directory 特性商店，任何连接字符串不是必要;因此，此字段将处于禁用状态。  
  
        > [!NOTE]  
        > 默认情况下，广告 FS 会自动创建 Active Directory 特性应用商店。  
 
![添加特性官方商城](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  单击**确定**。  
  
## <a name="additional-references"></a>其他参考  

[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
  
[特性官方商城的作用](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
