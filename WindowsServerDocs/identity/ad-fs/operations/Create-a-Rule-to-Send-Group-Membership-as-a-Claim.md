---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: 创建规则以声明方式发送组成员身份
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c9c4cdb881d77fe902776551b4e99061e67660ea
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189382"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>创建规则以声明方式发送组成员身份

使用发送组成员身份作为声明规则模板在 Active Directory 联合身份验证服务\(AD FS\)，可以创建一个规则，将使你可以选择 Active Directory 安全组以声明方式发送。 此规则，根据你选择的组，将发出单个声明。 例如，可以使用此规则模板来创建一个规则，如果用户是 Domain Admins 安全组的成员将发送包含值为管理员的组声明。 应仅为本地 Active Directory 域中的用户使用此规则。  
  
可以使用以下过程以使用 AD FS 管理管理单元创建声明规则\-中。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建一个规则以发送组成员身份作为声明在信赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送组成员身份**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   上**配置规则**页**声明规则名称**中键入此规则的显示名称**用户的组**单击**浏览**和选择组中下,**传出声明类型**选择所需的声明类型，然后在**传出声明类型**键入一个值。
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建一个规则以发送组成员身份作为声明在 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送组成员身份**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   上**配置规则**页**声明规则名称**中键入此规则的显示名称**用户的组**单击**浏览**和选择组中下,**传出声明类型**选择所需的声明类型，然后在**传出声明类型**键入一个值。 
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>若要创建一个规则以发送组成员身份作为 Windows Server 2012 R2 中的声明 
  
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

6.  上**配置规则**页**声明规则名称**中键入此规则的显示名称**用户的组**单击**浏览**和选择组中下,**传出声明类型**选择所需的声明类型，然后在**传出声明类型**键入一个值。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  单击 **“完成”** 。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  



## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
