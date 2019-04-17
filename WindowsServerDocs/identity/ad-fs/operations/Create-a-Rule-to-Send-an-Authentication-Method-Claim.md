---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: "创建发送身份验证方法索赔规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>创建发送身份验证方法索赔规则

>适用于：Windows Server 2016，Windows Server 2012 R2

您可以使用两个**作为索赔发送组成员**规则模板或**转换传入声称**规则模板发送身份验证方法索赔。 依赖方可用于身份验证方法索赔确定用户用于验证和从 Active Directory 联合身份验证服务 \(AD FS\) 获取索赔登录机制。 你还可以使用 Active Directory 联合身份验证服务 \(AD FS\) 的身份验证机制保证功能在 Windows Server 2012 R2 作为输入生成的情况下信赖方要确定根据智能卡登录的访问权限的级别身份验证方法索赔。 例如，开发人员可以联盟用户信赖方应用程序的分配不同级别的访问权限。 是否用户使用他们用户的名称和密码凭据登录，而不是其智能卡基于级别的访问权限。  
  
根据你的组织的要求，使用以下步骤之一：  
  
-   通过创建该规则**作为索赔发送组成员**规则模板 \-要指定最终确定哪些身份验证方法此模板中的组声称问题时，你可以使用此规则模板。  
  
-   通过创建该规则**转换传入声明**规则模板 \-你想要更改的现有的身份验证方法为新的身份验证方法适用于的无法识别标准广告 FS 身份验证方法声明的产品时，你可以使用此规则模板。  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用发送组成员作为上依赖方信任 Windows Server 2016 中的索赔规则模板创建 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送作为声明的组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，请选择的该组其是应会收到此身份验证方法索赔，，然后单击**确定**。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 在**传出声称值**、 键入默认值即可统一资源标识符 \(URI\) 之一在下表中，具体取决于你的首选身份验证方法、 单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性 \(TLS\) 相互身份验证 X.509 证书的使用|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>在 Windows Server 2016 中信任索赔提供商的索赔规则模板为使用发送组成员创建 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送作为声明的组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，请选择的该组其是应会收到此身份验证方法索赔，，然后单击**确定**。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 在**传出声称值**、 键入默认值即可统一资源标识符 \(URI\) 之一在下表中，具体取决于你的首选身份验证方法、 单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性 \(TLS\) 相互身份验证 X.509 证书的使用|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用转换创建此规则传入声称规则模板上依赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**身份验证方法**列表中。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 选择**替换传入的索赔值为不同的传出索赔值**，，然后执行以下操作：  
  
    1.  在**传入声称值**、 键入根据实际身份验证方法使用最初的 URI 以下 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
    2.  在**传出声称值**，在下表中，这取决于你的新首选身份验证方法选择中键入默认 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 对于显示 URI 值上一个表格反映依赖方接受默认 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用转换创建此规则传入的声称规则模板基于 Windows Server 2016 中信任索赔提供程序 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**身份验证方法**列表中。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 选择**替换传入的索赔值为不同的传出索赔值**，，然后执行以下操作：  
  
    1.  在**传入声称值**、 键入根据实际身份验证方法使用最初的 URI 以下 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
    2.  在**传出声称值**，在下表中，这取决于你的新首选身份验证方法选择中键入默认 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的索赔规则模板为使用发送组成员创建此规则  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在**编辑索赔规则**对话框中，选择一个以下选项卡，具体取决于正在编辑，并哪个规则设置您的信任想要创建该，规则，然后单击**添加规则**启动程序与该规则集规则向导：  
  
    -   **验收转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**作为索赔发送组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  在**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，请选择的该组其是应会收到此身份验证方法索赔，，然后单击**确定**。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 在**传出声称值**、 键入默认值即可统一资源标识符 \(URI\) 之一在下表中，具体取决于你的首选身份验证方法、 单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性 \(TLS\) 相互身份验证 X.509 证书的使用|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 如上表所示的 URI 值反映依赖方接受默认 Uri。  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>此规则使用创建转换传入的索赔规则模板在 Windows Server 2012 R2  
   
  
  
1.  在服务器管理器中，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  在**编辑索赔规则**对话框中，选择一个以下选项卡，这取决于你正在编辑，并将你的规则中设置信任想要创建此规则，，然后单击**添加规则**启动程序与该规则集规则向导：  
  
    -   **验收转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**身份验证方法**列表中。  
  
8.  在**传出声称类型**、 选择**身份验证方法**列表中。  
  
9. 选择**替换传入的索赔值为不同的传出索赔值**，，然后执行以下操作：  
  
    1.  在**传入声称值**、 键入根据实际身份验证方法使用最初的 URI 以下 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
    2.  在**传出声称值**，在下表中，这取决于你的新首选身份验证方法选择中键入默认 URI 值之一上，单击**完成**，然后单击**确定**保存规则。  
  
|实际身份验证方法|相应 URI|  
|--------------------------------|---------------------|  
|用户的名称和密码的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|不使用 TLS X.509\ 基于身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 对于显示 URI 值上一个表格反映依赖方接受默认 Uri。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
