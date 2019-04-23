---
ms.assetid: 3d770385-9834-4ebe-b66c-b684e0245971
title: 创建规则以根据传入声明允许或拒绝用户
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 167e43d49c08d0e39549bf46888118f985e3876d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863768"
---
# <a name="create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim"></a>创建规则以根据传入声明允许或拒绝用户 

>适用于：Windows Server 2016, Windows Server 2012 R2

在 Windows Server 2016 中，你可以使用**访问控制策略**若要创建将允许或拒绝用户根据传入声明的规则。  在 Windows Server 2012 R2 中，使用**允许或拒绝用户根据传入声明**Active Directory 联合身份验证服务中的规则模板\(AD FS\)，可以创建将授予的授权规则或根据类型和值的传入声明的信赖方拒绝用户的访问权限。 

例如，您可以使用此创建规则将允许的组声明的值为 Domain Admins 访问信赖方的用户。 如果你想要允许所有用户访问信赖方，请使用**都允许每个人都**访问控制策略或**都允许所有用户**规则模板，具体取决于你的 Windows Server 版本。 有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。  
  
可以使用以下过程以使用 AD FS 管理管理单元创建声明规则\-中。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。  

## <a name="to-create-a-rule-to-permit-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要创建一个规则以允许用户基于 Windows Server 2016 上的传入声明
 
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**访问控制策略**。 
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 右键单击并选择**添加访问控制策略**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在名称框中，输入你的策略、 说明和单击名称**添加**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny5.PNG)

5. 上**规则编辑器**，在用户下放置一个复选标记**与请求中的特定声明**单击带下划线**特定**底部。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny6.PNG)

6. 上**选择声明**屏幕上，单击**声明**单选按钮，选择**声明类型**，则**运算符**，和**声明值**然后单击**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny7.PNG)

7.  上**规则编辑器**单击**确定**。  上**添加访问控制策略**屏幕上，单击**确定**。

8. 在中**AD FS 管理**控制台树中，在**AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  右键单击**信赖方信任**想要允许访问，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在访问控制策略选择你的策略，然后单击**Apply**并**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny8.PNG)

## <a name="to-create-a-rule-to-deny-users-based-on-an-incoming-claim-on-windows-server-2016"></a>若要创建一个规则以拒绝用户基于 Windows Server 2016 上的传入声明
 
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**访问控制策略**。 
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny3.PNG)

3. 右键单击并选择**添加访问控制策略**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny4.PNG)

4. 在名称框中，输入你的策略、 说明和单击名称**添加**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny9.PNG)

5. 上**规则编辑器**，请确保每个人都处于选中状态并在**除**勾选**与请求中的特定声明**单击带下划线**特定**底部。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny10.PNG)

6. 上**选择声明**屏幕上，单击**声明**单选按钮，选择**声明类型**，则**运算符**，和**声明值**然后单击**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny11.PNG)

7.  上**规则编辑器**单击**确定**。  上**添加访问控制策略**屏幕上，单击**确定**。

8. 在中**AD FS 管理**控制台树中，在**AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

9.  右键单击**信赖方信任**想要允许访问，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

10. 在访问控制策略选择你的策略，然后单击**Apply**并**确定**。
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny12.PNG)

  
## <a name="to-create-a-rule-to-permit-or-deny-users-based-on-an-incoming-claim-on-windows-server-2012-r2"></a>若要创建一个规则以允许或拒绝用户基于 Windows Server 2012 R2 上的传入声明
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。    
  
2.  在控制台树中下, **AD FS\\信任关系\\信赖方信任**，单击你想要创建此规则的列表中的特定信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   

4.  在中**编辑声明规则**对话框中，单击**颁发授权规则**选项卡或**委派授权规则**选项卡\(基于的类型所需的授权规则\)，然后单击**添加规则**以启动**添加授权声明规则向导**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**允许或拒绝用户根据传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny1.PNG)

6.  上**配置规则**页**声明规则名称**中键入此规则的显示名称**传入声明类型**下在列表中，选择一种声明类型**传入声明值**键入一个值，或单击浏览\(如果可用\)并选择一个值，然后选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **允许访问具有此传入声明的用户**  
  
    -   **拒绝访问具有此传入声明的用户**  
![创建规则](media/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim/permitdeny2.PNG)  
7.  单击 **“完成”**。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
