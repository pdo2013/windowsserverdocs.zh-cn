---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: "创建规则转换传入的声明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>创建规则转换传入的声明

>适用于：Windows Server 2016，Windows Server 2012 R2

通过使用**转换传入声称**规则模板 Active Directory 联合身份验证服务 \(AD FS\) 在你可以选择传入的声明、 更改其索赔类型，并更改其声明值。 例如，可以使用此规则模板创建规则发送相同的索赔值为传入的组声明角色索赔。 你还可以使用此规则发送一组声称索赔值的购买者时传入组声明值的管理员，或者你可以发送仅主体用户名 \(UPN\) 索赔以结尾, @fabrikam。  
  
可以使用下面的过程中与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的已完成此过程的最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则转换依赖方信任在 Windows Server 2016 上传入的声明 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 在**传入声称类型**，在列表中选择一种类型的索赔。 在**传出声称类型**，在列表中，选择索赔类型，然后依次选择以下选项，取决于你的组织的要求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。
  
> [!NOTE]  
> 如果你要设置使用广告 FS\ 颁发索赔动态访问控制方案，首先创建转换规则在索赔提供商信任，或在**传入声称类型**、 为传入的声明中，键入名称或以前创建索赔描述，如果它从列表中选择。 第二个，在**传出声称类型**选择你想的索赔 URL，然后创建转换规则信赖的方信任处理设备索赔。  
>   
> 有关方案动态访问控制的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/dynamic-access-control--scenario-overview.md)或[与广告 FS 使用广告 DS 索赔](https://technet.microsoft.com/library/hh831504.aspx)。 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则转换上在 Windows Server 2016 信任索赔提供商传入声明 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 在**传入声称类型**，在列表中选择一种类型的索赔。 在**传出声称类型**，在列表中，选择索赔类型，然后依次选择以下选项，取决于你的组织的要求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

> [!NOTE]  
> 如果你要设置使用广告 FS\ 颁发索赔动态访问控制方案，首先创建转换规则在索赔提供商信任，或在**传入声称类型**、 为传入的声明中，键入名称或以前创建索赔描述，如果它从列表中选择。 第二个，在**传出声称类型**选择你想的索赔 URL，然后创建转换规则信赖的方信任处理设备索赔。  
>   
> 有关方案动态访问控制的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/dynamic-access-control--scenario-overview.md)或[与广告 FS 使用广告 DS 索赔](https://technet.microsoft.com/library/hh831504.aspx)。   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>若要创建规则，将在 Windows Server 2012 R2 传入声明 
  
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

6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 在**传入声称类型**，在列表中选择一种类型的索赔。 在**传出声称类型**，在列表中，选择索赔类型，然后依次选择以下选项，取决于你的组织的要求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> 如果你要设置使用广告 FS\ 颁发索赔动态访问控制方案，首先创建转换规则在索赔提供商信任，或在**传入声称类型**、 为传入的声明中，键入名称或以前创建索赔描述，如果它从列表中选择。 第二个，在**传出声称类型**选择你想的索赔 URL，然后创建转换规则信赖的方信任处理设备索赔。  
>   
> 有关方案动态访问控制的详细信息，请参阅[动态访问控制内容路线图](../../solution-guides/dynamic-access-control--scenario-overview.md)或[与广告 FS 使用广告 DS 索赔](https://technet.microsoft.com/library/hh831504.aspx)。  
  
7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
