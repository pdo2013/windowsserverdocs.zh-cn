---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: 创建信赖方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0d32edd7ebc23fa724439710c6511642d9c49a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407645"
---
# <a name="create-a-relying-party-trust"></a>创建信赖方信任


以下文档提供有关手动创建信赖方信任和使用联合元数据的信息。
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>手动创建声明感知信赖方信任 

若要使用 AD FS 管理 snap @ no__t-0in 添加新的信赖方信任并手动配置设置，请在联合服务器上执行以下过程。  

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。
  
1. 在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在 "**操作**" 下，单击 "**添加信赖方信任**"。  
![relying 参与方 @ no__t-1   

3.  在**欢迎**页上，选择 "**感知识别**"，然后单击 "**启动**"。  
![relying 参与方 @ no__t-1 
  
4.  在“选择数据源” 页上，单击“手动输入信赖方数据”，然后单击“下一步”。  
![relying 参与方 @ no__t-1 
  
5.  在 "**指定显示名称**" 页上，在 "**显示名称**" 中的 "**备注**" 下键入此信赖方信任的描述，然后单击 "**下一步**"。  
![relying 参与方 @ no__t-1 

6. 在 "**配置证书**" 页上，如果有可选的令牌加密证书，请单击 "**浏览**" 找到证书文件，然后单击 "**下一步**"。  
![relying 参与方 @ no__t-1 

7.  在 "**配置 URL** " 页面上，执行以下一项或两项操作，单击 "**下一步**"，然后继续执行步骤8：  
  
    -   选中 "**启用对 WS @ no__t-1Federation 被动协议的支持**" 复选框。 在 "**信赖方 WS @ no__t-1Federation 被动协议 url**" 下，键入此信赖方信任的 url，然后单击 "**下一步**"。  
  
    -   选中“启用对 SAML 2.0 WebSSO 的支持”复选框。 在 "**信赖方 SAML 2.0 SSO 服务 URL**" 下，键入此信赖方信任的安全断言标记语言 \(SAML @ no__t 服务终结点 url，然后单击 "**下一步**"。  
![relying 参与方 @ no__t-1   

8. 在“配置标识符”页上，为此信赖方指定一个或多个标识符、单击“添加”以将其添加到列表中，然后单击“下一步”。  
![relying 参与方 @ no__t-1
  
9.  在 "**选择访问控制策略**" 中选择一个策略，然后单击 "**下一步**"。  有关访问控制策略的详细信息，请参阅[AD FS 中的访问控制策略](Access-Control-Policies-in-AD-FS.md)。 
![relying 参与方 @ no__t-1

10. 在“准备好添加信任” 页上，复查设置，然后单击“下一步” 来保存信赖方信任的信息。  
   ![relying 参与方 @ no__t-1 
11. 在“完成”页面上，单击“关闭”。 执行此操作会自动显示“编辑声明规则”对话框。  
![relying 参与方 @ no__t-1 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>使用联合元数据创建声明感知信赖方信任

若要添加新的信赖方信任，请使用 AD FS 管理 "管理单元，通过从合作伙伴发布到本地网络或 Internet 的联合元数据自动导入有关合作伙伴的配置数据，请在帐户伙伴组织中的联合服务器。

>[!NOTE]
>尽管使用具有非限定主机名的证书（例如 https://myserver ）通常很常见，但这些证书没有安全价值，并且可以使攻击者模拟正在发布联合元数据的联合身份验证服务。 因此，在查询联合元数据时，只应使用完全限定的域名，例如 https://myserver.contoso.com 。

本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。


1. 在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2. 在 "**操作**" 下，单击 "**添加信赖方信任**"。  
   ![relying 参与方 @ no__t-1   

3. 在**欢迎**页上，选择 "**感知识别**"，然后单击 "**启动**"。  
   ![relying 参与方 @ no__t-1 
  
4. 在 "**选择数据源**" 页上，单击 "@no__t" 1Import "有关联机发布或在本地网络上发布的信赖方的数据"。在 "联合元数据地址（主机名或 URL） </strong> 中，键入伙伴的联合元数据 URL 或主机名，然后单击"**下一步**"。  
   ![relying 参与方 @ no__t-1 

5. 在 "指定显示名称" 页上的 "**显示名称**" 中，在 "备注" 中键入此信赖方信任的描述，然后单击 "**下一步**"。

6. 在 "选择颁发授权规则" 页上，选择 "**允许所有用户访问此信赖方**" 或 "**拒绝所有用户访问此信赖方**"，然后单击 "**下一步**"。

7. 在 "准备添加信任" 页上，查看设置，然后单击 "**下一步**" 以保存信赖方信任信息。

8. 在 "完成" 页上，单击 "**关闭**"。 此操作会自动显示 "编辑声明规则" 对话框。 有关如何为此信赖方信任添加声明规则的详细信息，请参阅“其他参考”。




## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
