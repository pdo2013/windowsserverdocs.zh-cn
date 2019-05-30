---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: 创建声明提供方信任
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 909be9e4bbcd12b00fd60ff061b1f4e2ede34546
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308538"
---
# <a name="create-a-claims-provider-trust"></a>创建声明提供方信任

若要添加新声明提供方信任使用 AD FS 管理管理单元\-并且手动配置设置，资源伙伴组织中的资源伙伴联合身份验证服务器上执行以下过程。  
  
中的成员身份**管理员**，或在本地计算机上等效身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>手动创建声明提供方信任的步骤  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  下**操作**，单击**添加声明提供方信任**。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在“欢迎使用”  页面上，单击“启动”  。 
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在“选择数据源”  页面上，单击“手动输入声明提供方信任数据”  ，然后单击“下一步”  。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  在“指定显示名称”页面上  键入一个显示名称  ，在“注释”  下键入此声明提供方信任的描述，然后单击“下一步”  。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  上**配置 URL**页上，指定**WS 联合身份验证被动 URL**如果适用，单击**下一步**。
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. 在“配置标识符”页面上的“声明提供方信任标识符”下，   键入相应的标识符，然后单击“下一步”  。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. 在“配置证书”页面上，单击“添加”找到证书文件并将它添加到证书列表，然后单击“下一步”    。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. 在“准备好添加信任”页面上，  单击“下一步”保存声明提供方信任信息  。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. 在“完成”  页面上，单击“关闭”  。 执行此操作会自动显示“编辑声明规则”对话框  。 详细了解如何以继续添加声明规则，此声明提供方信任，请参阅以下其他引用。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>若要创建声明提供程序信任使用联合身份验证元数据
若要添加新声明提供方信任，请使用 AD FS 管理管理单元中，通过从伙伴已发布到本地网络或 Internet 的联合身份验证元数据自动导入有关伙伴的配置数据执行以下过程上资源伙伴组织中的联合身份验证服务器。

>[!NOTE]
>尽管它很长时间以来常见的做法将证书与非限定的主机名称，例如 https:\//myserver，这些证书没有安全价值，并可以让攻击者模拟正在发布联合身份验证的联合身份验证服务元数据。 因此，当查询联合元数据，您应仅使用完全限定的域名如`https://myserver.contoso.com`。

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  下**操作**，单击**添加声明提供方信任**。  
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  在“欢迎使用”  页面上，单击“启动”  。 
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  在“选择数据源”页面上，  单击“导入有关在线或在本地网络上发布的声明提供方的数据”  。 在联合身份验证元数据地址 （主机名或 URL），键入**联合身份验证元数据 URL**或主机名称对于合作伙伴，然后单击**下一步**。
![声明提供方信任](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  指定显示名称在页上，键入**显示名称**，此声明提供方信任，则在说明下键入的描述，然后单击**下一步**。

6.  在已准备好添加信任页上，单击**下一步**保存声明提供方信任信息。

7.  在完成页上，单击**关闭**。 这将自动显示编辑声明规则对话框。 详细了解如何以继续添加声明规则，此声明提供方信任，请参阅下面的其他参考部分。



    
## <a name="additional-references"></a>其他参考  
[清单：配置资源伙伴组织](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[清单：为声明提供方信任创建声明规则](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>请参阅  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
  
