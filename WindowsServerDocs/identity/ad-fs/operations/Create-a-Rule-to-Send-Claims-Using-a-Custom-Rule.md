---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: 创建规则以使用自定义规则发送声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ade2a8304288d102608c81a0c29155478e5a4b7b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189430"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>创建规则以使用自定义规则发送声明


通过使用**使用自定义规则发送声明**Active Directory 联合身份验证服务 (AD FS) 中的模板，您可以创建自定义声明规则的标准规则模板不满足的要求的情况下你组织。 自定义声明规则声明规则语言编写而成，然后必须复制到**自定义规则**文本框中才可以在规则集中使用。 有关构造高级规则的语法的信息，请参阅[的声明规则语言角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。  
  
可以使用以下过程使用 AD FS 管理管理单元创建声明规则\-中。  
  
中的成员身份**管理员**，或在本地计算机上等效身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则，以传递或筛选传入声明上了信赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称。 下**自定义规则**，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击 **“完成”** 。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则，以传递或筛选传入声明上 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称。 下**自定义规则**，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击 **“完成”** 。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>若要创建一个规则以在 Windows Server 2012 R2 中使用的自定义声明发送声明 
  
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
  
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**使用自定义规则发送声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  上**配置规则**页面上，在**声明规则名称**，键入此规则的显示名称。 下**自定义规则**，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  单击 **“完成”** 。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
