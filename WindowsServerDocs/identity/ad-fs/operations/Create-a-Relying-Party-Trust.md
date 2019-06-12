---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: 创建信赖方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444358"
---
# <a name="create-a-relying-party-trust"></a>创建信赖方信任


以下文档提供有关手动创建信赖方信任和使用联合身份验证元数据信息。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>若要创建声明感知信赖方手动信任 

若要使用 AD FS 管理管理单元添加新的信赖方信任\-并且手动配置设置，联合身份验证服务器上执行以下过程。  

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。
  
1. 在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  下**操作**，单击**添加信赖方信任**。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  上**欢迎**页上，选择**声明感知**然后单击**启动**。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  在“选择数据源”  页上，单击“手动输入信赖方数据”  ，然后单击“下一步”  。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  上**指定显示名称**页上，键入一个名称中的**显示名称**下**说明**键入此信赖方信任的描述，然后单击**下一步**.  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. 上**配置证书**页上，如果您拥有一个可选的令牌加密证书，单击**浏览**以找到证书文件，然后单击**下一步**。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  上**配置 URL**页上，执行一个或两个以下操作，单击**下一步**，然后转到步骤 8:  
  
    -   选择**启用支持 WS\-联合身份验证被动协议**复选框。 下**信赖方 WS\-联合身份验证被动协议 URL**，键入此信赖方信任的 URL，然后单击**下一步**。  
  
    -   选中“启用对 SAML 2.0 WebSSO 的支持”  复选框。 下**信赖方 SAML 2.0 SSO 服务 URL**，键入安全断言标记语言\(SAML\)服务终结点 URL 为此信赖方信任，然后单击**下一步**.  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. 在“配置标识符”  页上，为此信赖方指定一个或多个标识符、单击“添加”  以将其添加到列表中，然后单击“下一步”  。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  上**选择访问控制策略**选择一个策略，然后单击**下一步**。  有关访问控制策略的详细信息，请参阅[AD FS 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![信赖方](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. 在“准备好添加信任”  页上，复查设置，然后单击“下一步”  来保存信赖方信任的信息。  
   ![信赖方](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. 在“完成”  页面上，单击“关闭”  。 执行此操作会自动显示“编辑声明规则”对话框  。  
![信赖方](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>若要创建声明感知信赖方信任使用联合身份验证元数据

若要添加新的信赖方信任，通过从伙伴发布到本地网络或 Internet 的联合身份验证元数据自动导入有关伙伴的配置数据使用 AD FS 管理管理单元中，执行以下过程在帐户伙伴组织中的联合身份验证服务器。

>[!NOTE]
>尽管它很长时间以来常见的做法将证书与非限定的主机名如 https://myserver，这些证书没有安全价值，并可以让攻击者模拟正在发布联合元数据联合身份验证服务。 因此，当查询联合元数据，您应仅使用完全限定的域名如 https://myserver.contoso.com。

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。


1. 在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2. 下**操作**，单击**添加信赖方信任**。  
   ![信赖方](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. 上**欢迎**页上，选择**声明感知**然后单击**启动**。  
   ![信赖方](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. 上**选择数据源**页上，单击<strong>联机或在本地网络 * 上发布的有关信赖方的导入数据。在 * * （主机名或 URL） 的联合身份验证元数据地址</strong>、 键入合作伙伴的联合身份验证元数据 URL 或主机名，然后单击**下一步**。  
   ![信赖方](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. 在指定显示名称页中键入一个名称**显示名称**，在说明下键入此信赖方信任的说明，然后单击**下一步**。

6. 在选择颁发授权规则页上选择任一**允许所有用户访问此信赖方**或**拒绝所有用户访问此信赖方**，然后单击**下一步**.

7. 在已准备好添加信任页上，查看设置，并单击**下一步**来保存信赖方信任信息。

8. 在完成页上，单击**关闭**。 此操作将自动显示编辑声明规则对话框。 有关如何为此信赖方信任添加声明规则的详细信息，请参阅“其他参考”。




## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
