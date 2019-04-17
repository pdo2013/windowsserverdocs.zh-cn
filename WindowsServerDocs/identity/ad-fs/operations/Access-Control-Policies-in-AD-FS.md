---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: "广告 FS 中访问控制策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>在 Windows Server 2016 广告 FS 访问控制策略

>适用于：Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>广告 FS 中访问控制策略模板  
Active Directory 联合身份验证服务现在支持使用访问控件策略模板。  通过访问控制策略模板管理员可以通过分配给信赖方 (RPs) 一组策略模板强制策略设置。 管理员可以还进行更新的策略模板和所做的更改将应用于信赖方自动是否需要无用户交互。  
  
## <a name="what-are-access-control-policy-templates"></a>访问控制策略模板有哪些？  
策略处理广告 FS 核心管道有三个阶段： 身份验证、 授权和索赔颁发。 当前，广告 FS 管理员具有单独的每个阶段配置策略。  这还涉及了解这些策略的影响，以及这些策略是否间的相关性。 另外，需要理解的索赔规则语言和作者自定义规则启用 （例如一些简单/常用策略管理员 阻止外部的访问权限）。  
  
管理员需要配置颁发授权规则使用此旧模型模板执行何种访问控制策略是替换索赔语言。  颁发授权规则旧的 PowerShell cmdlet 仍然适用，但它不相互包括新模型。 管理员可以选择要使用的新模型或旧模型。  新模型允许管理员，要控制何时授予访问权限，包括履行多重身份验证。  
  
访问控制策略模板使用允许型号。  这意味着，默认情况下，任何人都有访问和必须明确授予访问权限。  但是，这不是只需全或允许执行任何操作。  管理员可以添加到允许规则的异常。  例如，管理员可能会想要授予根据选择此选项，并指定 IP 地址范围的特定网络的访问权限。  但管理员可能会添加和例外，例如，则管理员可能会添加的特定网络的例外，并指定相应的 IP 地址范围。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>内置访问控制策略模板 vs 自定义访问控件策略模板  
广告 FS 包括几个内置访问控制策略模板。  这些目标拥有策略要求，如 Office 365 的客户端访问策略的同一组的一些常见情况。  这些模板无法修改。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
为了提供增强的灵活性解决你的需求，管理员可以创建自己访问策略模板。  这些可以进行修改后创建和自定义策略模板更改将适用于所有受这些策略模板 RPs。  添加自定义策略模板将只需内的广告 FS 管理单击从添加访问控件的策略。  
  
若要创建策略模板，需要先指定的情况下将令牌发放和/或委派授权请求管理员。 下表中显示条件和操作的选项。   使用不同的或新值管理员可以进一步配置粗体条件。 如果没有任何管理员可以指定异常。 符合条件时, 异常指定是否不会触发允许操作和传入请求符合条件异常中指定。  
  
|允许用户|除了| 
| --- | --- | 
 |从**特定**网络|从**特定**网络<br /><br />从**特定**组<br /><br />从设备**特定**信任级别<br /><br />与**特定**请求中的索赔|  
|从**特定**组|从**特定**网络<br /><br />从**特定**组<br /><br />从设备**特定**信任级别<br /><br />与**特定**请求中的索赔|  
|从设备**特定**信任级别|从**特定**网络<br /><br />从**特定**组<br /><br />从设备**特定**信任级别<br /><br />与**特定**请求中的索赔|  
|与**特定**请求中的索赔|从**特定**网络<br /><br />从**特定**组<br /><br />从设备**特定**信任级别<br /><br />与**特定**请求中的索赔|  
|和需要多重身份验证|从**特定**网络<br /><br />从**特定**组<br /><br />从设备**特定**信任级别<br /><br />与**特定**请求中的索赔|  
  
如果管理员选择多个条件，它们将的**和**关系。 操作为相互独家和一个策略规则，你仅可以选择一项操作。 如果管理员选择多个异常，它们将的**或者**关系。 几个策略规则示例如下所示：  
  
|**策略**|**策略规则**|
| --- | --- |  
|外部访问需要 MFA<br /><br />允许所有用户|**规则 #1**<br /><br />从**联网**<br /><br />和 MFA<br /><br />允许<br /><br />**规则 #2**<br /><br />从**intranet**<br /><br />允许|  
|除非 FTE 不允许使用外部访问<br /><br />对于 FTE 加入工作区设备上的 intranet 访问获准|**规则 #1**<br /><br />从**联网**<br /><br />以及从**非 FTE**组<br /><br />允许<br /><br />**规则 #2**<br /><br />从**intranet**<br /><br />以及从**加入工作区**设备<br /><br />以及从**FTE**组<br /><br />允许|  
|外部访问需要 MFA 除"服务管理"<br /><br />允许所有用户访问|**规则 #1**<br /><br />从**联网**<br /><br />和 MFA<br /><br />允许<br /><br />除**的服务管理员组**<br /><br />**规则 #2**<br /><br />始终<br /><br />允许|  
|工作位置连接的设备访问来自联网需要 MFA<br /><br />允许 intranet 和外部访问广告的结构|**规则 #1**<br /><br />从**intranet**<br /><br />以及从**广告结构**组<br /><br />允许<br /><br />**规则 #2**<br /><br />从**联网**<br /><br />以及从**非工作区加入**设备<br /><br />以及从**广告结构**组<br /><br />和 MFA<br /><br />允许<br /><br />**规则 3**<br /><br />从**联网**<br /><br />以及从**加入工作区**设备<br /><br />以及从**广告结构**组<br /><br />允许|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>参数化的策略模板与非参数化策略模板  
可访问控制策略  
  
参数化的策略模板是已参数策略模板。 管理员需要输入的值为这些参数分配给 RPs.An 管理员此模板时不能后对进行更改参数化的策略模板创建它。  参数化策略的一个示例是内置的策略，允许特定组。  此策略应用到 RP，只要此参数需要指定。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
非参数化策略模板是没有参数策略模板。 管理员可以不需要任何输入向 RPs 分配此模板，并可以后创建它非参数化策略模板对进行更改。  它的一个示例是内置的策略，允许任何人需要 MFA。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>如何创建非参数化访问控制策略  
若要创建一个非参数化访问控制策略使用以下步骤  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>若要创建非参数化访问控制策略  
  
1.  从左侧广告 FS 管理选择访问控制策略，然后单击在右侧的添加访问控制策略。  
  
2.  输入姓名和描述。  例如： 与设备经过身份验证允许用户。  
  
3.  下**满足以下规则任一时允许访问**，单击**添加**。  
  
4.  下允许、 打勾在框中旁边**从具有特定信任级别的设备**  
  
5.  在底部，选择带下划线**特定**  
  
6.  从了弹出窗口中，选择**验证**从下拉列表。  单击**确定**。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  单击**确定**。 单击**确定**。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>如何创建参数化的访问控制策略  
若要创建参数化的访问控件策略，请使用以下步骤  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要创建参数化的访问控制策略  
  
1.  从左侧广告 FS 管理选择访问控制策略，然后单击在右侧的添加访问控制策略。  
  
2.  输入姓名和描述。  例如： 允许用户使用特定的索赔。  
  
3.  下**满足以下规则任一时允许访问**，单击**添加**。  
  
4.  下允许、 打勾在框中旁边**与特定索赔请求中**  
  
5.  在底部，选择带下划线**特定**  
  
6.  从了弹出窗口中，选择**指定访问控制策略分配时的参数**。  单击**确定**。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  单击**确定**。 单击**确定**。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>如何与异常创建自定义访问控制策略  
若要创建访问控制因异常策略，请使用下面的过程。  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>因异常创建自定义访问控制策略  
  
1.  从左侧广告 FS 管理选择访问控制策略，然后单击在右侧的添加访问控制策略。  
  
2.  输入姓名和描述。  例如： 使用允许用户身份验证的设备，但未管理。  
  
3.  下**满足以下规则任一时允许访问**，单击**添加**。  
  
4.  下允许、 打勾在框中旁边**从具有特定信任级别的设备**  
  
5.  在底部，选择带下划线**特定**  
  
6.  从了弹出窗口中，选择**验证**从下拉列表。  单击**确定**。  
  
7.  下除外、 旁边的框中打勾**从具有特定信任级别的设备**  
  
8.  在底部下除外、 选择带下划线**特定**  
  
9. 从了弹出窗口中，选择**管理**从下拉列表。  单击**确定**。  
  
10. 单击**确定**。 单击**确定**。  
  
    ![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>如何使用多个允许条件创建自定义访问控制策略  
若要使用多个允许创建访问控制策略条件，请使用以下步骤  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>若要创建参数化的访问控制策略  
  
1.  从左侧广告 FS 管理选择访问控制策略，然后单击在右侧的添加访问控制策略。  
  
2.  输入姓名和描述。  例如： 允许特定声明的和特定组的用户。  
  
3.  下**满足以下规则任一时允许访问**，单击**添加**。  
  
4.  下允许、 打勾在框中旁边**从特定组**和**与特定索赔请求中**  
  
5.  在底部，选择带下划线**特定**的第一个条件，旁边组  
  
6.  从了弹出窗口中，选择**指定策略分配时的参数**。  单击**确定**。  
  
7.  在底部，选择带下划线**特定**的索赔旁边的第二个条件  
  
8.  从了弹出窗口中，选择**指定访问控制策略分配时的参数**。  单击**确定**。  
  
9. 单击**确定**。 单击**确定**。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>如何为新的应用程序分配访问控制策略  
分配一个新的应用程序访问控制策略是非常直接向前和已现在集成到向导添加 RP。  从依赖方信任向导中，你可以选择你想要分配的访问权限控件策略。  创建新信赖的方信任时，这是要求。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>如何向现有的应用程序分配访问控制策略  
访问控制为分配策略现有的应用程序只需选择该应用程序从信赖方信任和右键单击**编辑访问控制策略**。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
从此处，你可以选择访问控制策略，并将其应用于该应用程序。  
  
![访问控制策略](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>请参阅  
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 

