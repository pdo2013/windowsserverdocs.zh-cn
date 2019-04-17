---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: "创建规则要允许或拒绝用户基于传入的声明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: afcbb8c7a08a84eda2a794c9565061d7d61d470b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>创建规则要允许或拒绝用户基于传入的声明 

>适用于：Windows Server 2016，Windows Server 2012 R2

在 Windows Server 2016，可以使用**访问控制策略**创建规则的将允许拒绝根据传入的声明的用户。  在 Windows Server 2012 R2，使用**允许拒绝用户根据或传入声称**规则模板 Active Directory 联合身份验证服务 \(AD FS\)，你可以创建授权规则将授予或拒绝依赖方基于类型和传入的声明的值为用户的访问权限。 

例如，你可以使用此创建规则将允许有声称值为域管理员访问依赖方一组的用户。 如果你想要允许所有用户访问依赖方，请使用**都允许任何人**访问控制策略或**都允许所有用户**具体取决于你的版本的 Windows Server 规则模板。 用户有权访问依赖方从联合身份验证服务可能仍会拒绝服务依赖方。  
  
可以使用下面的过程中与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要创建规则允许用户基于 Windows Server 2016 上传入的声明
 
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**访问控制策略**。 
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 右键单击并选择**添加访问控制策略**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在框中，输入你的策略、 说明和单击的名称**添加**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 在**规则编辑器**下用户，, 将签入**与请求中的特定索赔**单击带下划线**特定**底部。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 在**选择索赔**屏幕上，单击**索赔**单选按钮、 选择**声称类型**、**运营商**，和**声明值**然后单击**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  在**规则编辑器**单击**确定**。  在**添加访问控制策略**屏幕上，单击**确定**。

8. 在**广告 FS 管理**控制台树中下,**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  右键单击**依赖方信任**你想要允许访问权限，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 上访问控制策略中，选择你的策略中，然后单击**应用**和**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要创建规则来拒绝基于 Windows Server 2016 上传入声明的用户
 
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**访问控制策略**。 
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 右键单击并选择**添加访问控制策略**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在框中，输入你的策略、 说明和单击的名称**添加**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 在**规则编辑器**，确保选中了所有人下**除**打勾中的**与请求中的特定索赔**单击带下划线**特定**底部。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 在**选择索赔**屏幕上，单击**索赔**单选按钮、 选择**声称类型**、**运营商**，和**声明值**然后单击**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  在**规则编辑器**单击**确定**。  在**添加访问控制策略**屏幕上，单击**确定**。

8. 在**广告 FS 管理**控制台树中下,**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  右键单击**依赖方信任**你想要允许访问权限，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 上访问控制策略中，选择你的策略中，然后单击**应用**和**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>若要创建规则要允许或拒绝用户基于 Windows Server 2012 R2 上传入的声明
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。    
  
2.  控制台树中，在下**广告 FS\\Trust Relationships\\Relying 方信任**，单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  在**编辑索赔规则**对话框中，单击**颁发授权规则**选项卡或**委派授权规则**选项卡 \ （基于授权规则类型你 require\），然后单击**添加规则**开始**添加授权索赔规则向导**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**允许或用户拒绝根据传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**传入声称类型**下在列表中，选择声明类型**传入声称值**键入值，或单击浏览 \ （如果它是 available\） 和选择一个值，然后选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **允许访问到用户与此传入的声明**  
  
    -   **拒绝访问到用户与此传入的声明**  
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
