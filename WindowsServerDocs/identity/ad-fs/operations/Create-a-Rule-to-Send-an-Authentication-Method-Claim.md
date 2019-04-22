---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: 创建规则以发送身份验证方法声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4065a61e042f52298da656899289e718e010f932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819088"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>创建规则以发送身份验证方法声明

>适用于：Windows Server 2016, Windows Server 2012 R2

你可以使用**以声明方式发送组成员身份**规则模板或**转换传入声明**要发送的身份验证方法声明规则模板。 信赖方可以使用的身份验证方法声明来确定来自 Active Directory 联合身份验证服务的用户进行身份验证并获取时使用的登录机制声明\(AD FS\)。 此外可以使用 Active Directory 联合身份验证服务的身份验证机制保证功能\(AD FS\) Windows Server 2012 R2 中作为输入要在其中生成的情况下的身份验证方法声明的信赖方想要确定的基于智能卡登录的访问级别。 例如，开发人员可以将不同级别的访问权限分配给信赖方应用程序的联合用户。 级别的访问权限基于是否用户使用其用户名称和密码凭据登录，而不是其智能卡。  
  
根据你的组织的要求，使用以下过程之一：  
  
-   使用创建此规则**以声明方式发送组成员身份**规则模板\-想最终到此模板中指定的组，确定哪些身份验证方法时，可以使用此规则模板要发出声明。  
  
-   使用创建此规则**转换传入声明**规则模板\-想要将现有的身份验证方法更改为适用于产品的新身份验证方法时，可以使用此规则模板无法识别标准 AD FS 身份验证方法声明。  
  


## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要使用发送组成员身份作为信赖方信任 Windows Server 2016 中的声明规则模板创建 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送组成员身份**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，选择的组的成员应接收此身份验证方法声明，然后依次**确定**。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 在中**传出声明值**，键入一个默认的统一资源标识符\(URI\)值在以下表中，具体取决于你首选的身份验证的方法，请单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性\(TLS\)使用 X.509 证书的相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)
  
## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要使用发送组成员身份作为声明规则模板在 Windows Server 2016 中的声明提供方信任创建 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送组成员身份**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，选择的组的成员应接收此身份验证方法声明，然后依次**确定**。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 在中**传出声明值**，键入一个默认的统一资源标识符\(URI\)值在以下表中，具体取决于你首选的身份验证的方法，请单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性\(TLS\)使用 X.509 证书的相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建此规则使用转换传入声明规则了信赖方信任在 Windows Server 2016 上的模板 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**身份验证方法**列表中。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 选择**传入声明值替换为不同的传出声明值**，然后执行以下操作：  
  
    1.  在中**传入声明值**，键入以下基于实际的身份验证方法原来使用的 URI 的 URI 值之一，单击**完成**，然后单击**确定**以保存规则。  
  
    2.  在中**传出声明值**，键入一个默认的 URI 值取决于新的首选身份验证方法选择下表中，单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书的 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)
  
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 显示离子电池的 URI 值上表反映依赖方接受默认的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建此规则使用转换传入声明规则模板基于 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**身份验证方法**列表中。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 选择**传入声明值替换为不同的传出声明值**，然后执行以下操作：  
  
    1.  在中**传入声明值**，键入以下基于实际的身份验证方法原来使用的 URI 的 URI 值之一，单击**完成**，然后单击**确定**以保存规则。  
  
    2.  在中**传出声明值**，键入一个默认的 URI 值取决于新的首选身份验证方法选择下表中，单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书的 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>若要通过使用发送组成员身份作为 Windows Server 2012 R2 中的声明规则模板来创建此规则  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击特定你想要创建此规则的列表中的信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在中**编辑声明规则**对话框中，以下选项卡中选择一个，具体取决于正在编辑的和的规则集的信任想要创建在中，此规则，然后单击**添加规则**启动规则与该规则集关联的向导：  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送组成员身份**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)
  
6.  上**配置规则**页上，键入声明规则名称。  
  
7.  单击**浏览**，选择的组的成员应接收此身份验证方法声明，然后依次**确定**。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 在中**传出声明值**，键入一个默认的统一资源标识符\(URI\)值在以下表中，具体取决于你首选的身份验证的方法，请单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|传输层安全性\(TLS\)使用 X.509 证书的相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)
 
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 上表中所示的 URI 值反映依赖方接受默认的 Uri。  
  
## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要创建此规则使用转换传入声明规则模板在 Windows Server 2012 R2  
   
  
  
1.  在服务器管理器中，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击特定你想要创建此规则的列表中的信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
 
4.  在中**编辑声明规则**以下选项卡，具体取决于你正在编辑和中的规则集的信任对话框中，选择一个想要创建此规则，然后单击**添加规则**启动规则与该规则集关联的向导：  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    
  
6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**身份验证方法**列表中。  
  
8.  在中**传出声明类型**，选择**身份验证方法**列表中。  
  
9. 选择**传入声明值替换为不同的传出声明值**，然后执行以下操作：  
  
    1.  在中**传入声明值**，键入以下基于实际的身份验证方法原来使用的 URI 的 URI 值之一，单击**完成**，然后单击**确定**以保存规则。  
  
    2.  在中**传出声明值**，键入一个默认的 URI 值取决于新的首选身份验证方法选择下表中，单击**完成**，然后单击**确定**以保存规则。  
  
|实际的身份验证方法|对应 URI|  
|--------------------------------|---------------------|  
|用户名和密码身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password|  
|Windows 身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows|  
|使用 X.509 证书的 TLS 相互身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient|  
|X.509\-基于不使用 TLS 的身份验证|https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509|  
![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)
  
> [!NOTE]  
> 除了表中的值，可以使用其他 URI 值。 显示离子电池的 URI 值上表反映依赖方接受默认的 Uri。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供程序创建声明规则信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
