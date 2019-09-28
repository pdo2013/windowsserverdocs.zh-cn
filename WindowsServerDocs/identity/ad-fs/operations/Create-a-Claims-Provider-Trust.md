---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: 创建声明提供方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4539e8abd1af1eca7bacb51971e6d355bb0aab28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407650"
---
# <a name="create-a-claims-provider-trust"></a>创建声明提供方信任

若要使用 AD FS 管理 snap @ no__t-0in 添加新的声明提供方信任并手动配置设置，请在资源伙伴组织中的资源伙伴联合服务器上执行以下过程。  
  
在本地计算机上， **Administrators**中的成员身份或同等身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>手动创建声明提供方信任的步骤  
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在 "**操作**" 下，单击 "**添加声明提供方信任**"。  
@no__t 0claims 提供程序信任 @ no__t-1   
  
3.  在“欢迎使用”页面上，单击“启动”。 
@no__t 0claims 提供程序信任 @ no__t-1    
  
4.  在“选择数据源”页面上，单击“手动输入声明提供方信任数据”，然后单击“下一步”。  
@no__t 0claims 提供程序信任 @ no__t-1     

5.  在“指定显示名称”页面上键入一个显示名称，在“注释”下键入此声明提供方信任的描述，然后单击“下一步”。  
@no__t 0claims 提供程序信任 @ no__t-1     

6.  在 "**配置 URL** " 页面上，指定**WS 联合身份验证被动 URL** （如果适用），然后单击 "**下一步**"。
@no__t 0claims 提供程序信任 @ no__t-1     

8. 在“配置标识符”页面上的“声明提供方信任标识符”下，键入相应的标识符，然后单击“下一步”。  
@no__t 0claims 提供程序信任 @ no__t-1    

9. 在“配置证书”页面上，单击“添加”找到证书文件并将它添加到证书列表，然后单击“下一步”。  
@no__t 0claims 提供程序信任 @ no__t-1    

10. 在“准备好添加信任”页面上，单击“下一步”保存声明提供方信任信息。  
@no__t 0claims 提供程序信任 @ no__t-1    

11. 在“完成”页面上，单击“关闭”。 执行此操作会自动显示“编辑声明规则”对话框。 有关如何继续添加此声明提供方信任的声明规则的详细信息，请参阅以下附加引用。  
@no__t 0claims 提供程序信任 @ no__t-1

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>使用联合元数据创建声明提供方信任
若要添加新的声明提供方信任，请使用 AD FS 管理单元，通过从合作伙伴发布到本地网络或 Internet 的联合元数据自动导入有关伙伴的配置数据，请在资源伙伴组织中的联合服务器。

>[!NOTE]
>尽管使用具有非限定主机名的证书（例如 https： \//myserver）通常是一种很常见的做法，但这些证书没有安全价值，并且可以使攻击者模拟正在发布联合的联合身份验证服务新元. 因此，在查询联合元数据时，只应使用完全限定的域名，例如 `https://myserver.contoso.com`。

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在 "**操作**" 下，单击 "**添加声明提供方信任**"。  
@no__t 0claims 提供程序信任 @ no__t-1   
  
3.  在“欢迎使用”页面上，单击“启动”。 
@no__t 0claims 提供程序信任 @ no__t-1    
  
4.  在“选择数据源”页面上，单击“导入有关在线或在本地网络上发布的声明提供方的数据”。 在 "联合元数据地址（主机名或 URL）" 中，键入伙伴的**联合元数据 URL**或主机名，然后单击 "**下一步**"。
@no__t 0claims 提供程序信任 @ no__t-1    

5.  在 "指定显示名称" 页上，键入**显示名称**，在 "注释" 中键入此声明提供方信任的描述，然后单击 "**下一步**"。

6.  在 "准备添加信任" 页上，单击 "**下一步**" 以保存声明提供程序信任信息。

7.  在 "完成" 页上，单击 "**关闭**"。 这会自动显示 "编辑声明规则" 对话框。 有关如何继续为此声明提供方信任添加声明规则的详细信息，请参阅下面的 "其他参考" 部分。



    
## <a name="additional-references"></a>其他参考  
[清单：配置资源伙伴组织 @ no__t-0  
  
[清单：为声明提供方信任创建声明规则](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
  
