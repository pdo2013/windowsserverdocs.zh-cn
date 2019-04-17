---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: "创建发送索赔自定义规则的使用规则"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f0eef89e651585d48ba87d14bc782efa49087669
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>创建发送索赔自定义规则的使用规则

>适用于：Windows Server 2016，Windows Server 2012 R2

通过使用**发送索赔使用自定义规则**模板 Active Directory 联合身份验证服务 (广告 FS) 中的时，你可以创建自定义声明规则标准规则模板不满足你的组织的要求的情况。 自定义声明规则编写的索赔规则语言和必须然后复制到**自定义规则**才可以使用规则集中的文本框。 有关连建造的语法高级规则的信息，请参阅[索赔规则语言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。  
  
可以使用下面的过程中使用广告 FS 管理 snap\ 中创建索赔规则。  
  
在会员**管理员**，或等效，在本地计算机上的已完成此过程的最低要求。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则通过或筛选依赖方信任在 Windows Server 2016 上的传入声明 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送索赔使用自定义规则**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 下**自定义规则**、 键入或粘贴所需的此规则索赔规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则以通过或筛选索赔提供商信任在 Windows Server 2016 上传入的声明 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送索赔使用自定义规则**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 下**自定义规则**、 键入或粘贴所需的此规则索赔规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要创建规则发送使用自定义的索赔声称在 Windows Server 2012 R2 
  
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
  
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送索赔使用自定义规则**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  在**配置规则**页面上下,**声明规则名称**，键入的显示名称为此规则。 下**自定义规则**、 键入或粘贴所需的此规则索赔规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
