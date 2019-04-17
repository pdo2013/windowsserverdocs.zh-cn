---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: "创建信赖聚会信任"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>创建信赖聚会信任

>适用于：Windows Server 2016，Windows Server 2012 R2

下面的文档提供有关手动创建信赖的方信任和使用联盟元数据的信息。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>若要创建索赔注意到信赖方手动信任 

若要使用广告 FS 管理 snap\ 中添加新信赖的方信任并手动配置设置，请联合身份验证的服务器上执行以下步骤。  

在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。
  
1. 在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  下**操作**，单击**添加依赖方信任**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**欢迎**页面上，选择**注意到的索赔**单击**开始**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在**选择数据源**页上，单击**手动输入信赖方有关的数据**，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  在**指定的显示名称**页上中, 键入一个名称**显示名称**下**笔记**键入这个信赖的方信任的说明，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. 在**配置证书**页面上，如果你有一个可选令牌加密证书，请单击**浏览**以查找证书文件时，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  在**配置 URL**页上，执行以下一项或两项操作，单击**下一步**，然后转到第 8 步：  
  
    -   选择**启用 WS\ 联盟被动协议的支持**复选框。 下**信赖方 WS\ 联盟被动协议 URL**，请键入该信赖的方信任的 URL，然后单击**下一步**。  
  
    -   选择**启用 SAML 2.0 WebSSO 协议的支持**复选框。 下**信赖方 SAML 2.0 SSO 服务 URL**，请键入该信赖的方信任的安全肯定标记语言 \(SAML\) 服务端点 URL，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. 在**配置标识符**页、 为该信赖方指定一个或多个标识符上，单击**添加**以将其添加到列表，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  在**选择访问控制策略**选择某个策略，然后单击**下一步**。  有关访问控件策略的详细信息，请参阅[广告 FS 中访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![依赖方](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. 在**添加信任准备已就绪**页上，查看设置，，然后单击**下一步**保存你依赖方信任的信息。  
   ![依赖方](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. 在**完成**页上，单击**关闭**。 此操作将自动显示**编辑索赔规则**对话框。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>若要创建索赔注意到信赖方信任使用联盟元数据

若要添加的新信赖方信任，使用广告 FS 管理贴靠中，通过从联盟元数据的合作伙伴发布到本地网络或 Internet，自动导入的合作伙伴的配置数据执行以下步骤，在帐户合作伙伴公司联盟服务器上。

>[!NOTE]
>它早已通常使用具有不合格的主机名称，如 https://myserver 证书，也是如此这些证书没有安全值，并且可以攻击者模拟发布联盟元数据联合身份验证服务。 因此，查询时联盟元数据，你应仅使用合法的域名 https://myserver.contoso.com 如。

在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。


1. 在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  下**操作**，单击**添加依赖方信任**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  在**欢迎**页面上，选择**注意到的索赔**单击**开始**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在**选择数据源**页上，单击 **导入信赖方有关的数据发布在线或本地网络上*。 在**联合身份验证 （主机名称或 URL） 的元数据地址**、 键入合作伙伴，联盟元数据 URL 或主名称，然后单击**下一步**。  
![依赖方](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  在指定的显示名称页面中键入一个名称**显示名称**键入这个信赖的方信任的说明下笔记,，然后单击**下一步**。

6.  在选择颁发授权规则页上，选择**允许所有用户访问此依赖方**或**拒绝此信赖方所有用户访问**，然后单击**下一步**。

7.  在已准备好添加信任页面上，查看设置，，然后单击**下一步**保存你依赖方信任的信息。

8.  在完成页上，单击**关闭**。 此操作将自动显示编辑索赔规则对话框。 有关如何继续添加索赔规则此信赖的方信任的详细信息，请参阅其他参考。




## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
