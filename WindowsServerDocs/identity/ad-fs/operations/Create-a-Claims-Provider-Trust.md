---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: "创建索赔提供商信任"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>创建索赔提供商信任

>适用于：Windows Server 2016，Windows Server 2012 R2

若要使用广告 FS 管理 snap\ 中添加新的索赔提供商信任并手动配置设置，请资源合作伙伴组织中资源合作伙伴联合身份验证的服务器上执行以下步骤。  
  
在会员**管理员**，或等效，在本地计算机上的已完成此过程的最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>若要手动创建索赔提供商信任  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  下**操作**，单击**添加索赔提供商信任**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在**欢迎**页上，单击**开始**。 
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  上**选择数据源**页上，单击**Enter 声明提供商信任数据手动**，然后单击**下一步**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  在**指定的显示名称**页上，键入**显示名称**下**笔记**，对此声明的提供商信任键入描述，然后单击**下一步**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  在**配置 URL**页面上，指定**WS 联盟被动 URL**如果适用，然后单击**下一步**。
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. 在**配置标识符**页面上下,**索赔提供商信任标识符**、 键入相应的标识符，然后单击**下一步**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. 在**配置证书**页上，单击**添加**以找到证书文件并将其添加到列表中的证书，然后单击**下一步**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. 在**添加信任准备已就绪**页上，单击**下一步**保存你的索赔提供商信任信息。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. 在**完成**页上，单击**关闭**。 此操作将自动显示**编辑索赔规则**对话框。 有关如何继续添加详细信息声称本声明的提供商信任规则，请参阅下面的其他参考。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>若要创建索赔提供商信任使用联盟元数据
若要添加新的索赔提供商信任，使用广告 FS 管理贴靠中，通过从联盟元数据的合作伙伴已发布到本地网络或 Internet，自动导入的合作伙伴的配置数据资源合作伙伴组织中联盟服务器上执行以下步骤。

>[!NOTE]
>它早已通常使用具有不合格的主机名称，如 https://myserver 证书，也是如此这些证书没有安全值，并且可以攻击者模拟发布联盟元数据联合身份验证服务。 因此，查询时联盟元数据，你应仅使用合法的域名 https://myserver.contoso.com 如。

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  下**操作**，单击**添加依赖方信任**。  
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在**欢迎**页上，单击**开始**。 
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在**选择数据源**页上，单击**导入的索赔提供有关的数据发布在线或本地网络上**。 在联盟元数据的地址 （主机名称或 URL），键入**联盟元数据 URL**或主机的合作伙伴公司名，然后单击**下一步**。
![索赔提供商信任](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  上指定的显示名称页面类型**显示名称**，这索赔提供商信任的则在笔记下，键入描述，然后单击**下一步**。

6.  在已准备好添加信任页面上，单击**下一步**保存你的索赔提供商信任信息。

7.  在完成页上，单击**关闭**。 这将自动显示在编辑索赔规则对话框。 有关如何继续添加详细信息声称本声明的提供商信任规则，请参阅下面的更多参考部分。



    
## <a name="additional-references"></a>其他参考  
[清单： 配置资源合作伙伴公司](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[信任清单：创建索赔提供商的索赔规则](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
  
