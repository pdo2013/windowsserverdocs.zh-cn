---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: "部署中心访问策略（演示步骤）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 16973b32407985ffc3f66bf5ac579384a756b5d0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>部署中心访问策略（演示步骤）

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在此情况下，财经部门安全操作处理指定中心访问策略需，以便他们可以保护存档的财经信息文件服务器上存储的核心信息安全。 每个国家/地区的存档的财经信息可以访问来自相同国家/地区的财经员工为只读。 中央财经管理员组可以从所有国家/地区访问财务信息。  
  
部署中心访问策略包括以下几个阶段：  
  
|阶段|描述  
|---------|---------------  
|[确定需要策略和配置部署所需套餐：](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|确定需要策略和配置所需的部署。 
|[实现：配置组件和策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|配置组件和策略。  
|[部署中心访问策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|部署策略。  
|[保持：更改和阶段策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|策略更改和临时。 
  
## <a name="BKMK_1.1"></a>设置测试环境  
在开始之前，你需要测试此项 scenario 将设置为实验。 步骤适用于设置此实验介绍此内容中的详述[附录 b：设置了测试环境](Appendix-B--Setting-Up-the-Test-Environment.md)。  
  
## <a name="BKMK_1.2"></a>确定需要策略和配置部署所需套餐：  
此部分中提供了高级的一系列步骤处于规划阶段部署的帮助。  
  
||步骤|示例|  
|-|--------|-----------|  
|1.1|企业确定，中心访问策略|保护存储文件服务器的财经信息，财经部门安全操作中心的信息安全指定中心访问策略需要工作。|  
|1.2|快速访问策略|财经部门成员应仅读取财经文档。 财经部门成员应仅访问自己的国家/地区的文档。 只有财经管理员应有写入访问权限。 例外将允许 FinanceException 组中的成员。 此组将具有阅读访问权限。|  
|1.3|快速访问 Windows Server 2012 构造策略|定向：<br /><br />-Resource.Department 包含财经<br /><br />使用规则中：<br /><br />-允许阅读 User.Country=Resource.Country 和 User.department = Resource.Department<br />-允许完全控制 User.MemberOf(FinanceAdmin)<br /><br />例外情况：<br /><br />允许 memberOf(FinanceException) 阅读|  
|1.4|确定的策略所需的文件属性|使用标记的文件：<br /><br />-商业部<br />国家/地区|  
|1.5|确定索赔类型和的策略所需的组|声称类型：<br /><br />国家/地区<br />-商业部<br /><br />用户组：<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|确定要将此策略应用对其服务器|应用所有财经文件服务器上的策略。|  
  
## <a name="BKMK_1.3"></a>实现：配置组件和策略  
此部分中提供用于部署的财经文档中心访问策略示例。  
  
|不|步骤|示例|  
|------|--------|-----------|  
|2.1|创建索赔类型|创建以下索赔类型：<br /><br />-商业部<br />国家/地区|  
|2.2|创建资源属性|创建和启用以下资源属性：<br /><br />-商业部<br />国家/地区|  
|2.3|配置中央使用规则|创建财经文档规则包含确定一节中的策略。|  
|2.4|将中心访问策略（笔帽）配置|创建称为财经策略笔帽和财经文档规则添加到该笔帽。|  
|2.5|文件服务器的目标中心访问策略|发布到文件服务器的财经策略笔帽。|  
|2.6|启用索赔，复合身份验证和 Kerberos 程度 KDC 支持。|启用索赔，复合身份验证和 Kerberos 程度为 contoso.com KDC 支持。|  
  
在下面的过程中，您将创建两个索赔类型：国家/地区和部门。  
  
#### <a name="to-create-claim-types"></a>若要创建索赔类型  
  
1.  打开在 Hyper-V 管理器和日志中服务器 DC1 作为 contoso\administrator，使用密码**pass@word1**。  
  
2.  打开 Active Directory 管理中心。  
  
3.  单击**树视图图标**，展开**动态访问控制**，然后选择**索赔类型**。  
  
    右键单击**索赔类型**，单击**新建**，然后单击**声明类型**。  
  
    > [!TIP]  
    > 你还可以打开**创建索赔类型：**窗口中的从**任务**窗格。 在**任务**窗格中，单击**新建**，然后单击**声明类型**。  
  
4.  在**源属性**列表、向下滚动列表的属性，然后单击**部门**。 这应该填充**显示名称**字段使用**部门**。 单击**确定**。  
  
5.  在**任务**窗格中，单击**新建**，然后单击**声明类型**。  
  
6.  在**源属性**列表中，向下滚动的属性，列表，然后单击**c**属性（国家/地区名称）。 在**显示名称**字段中，键入**国家/地区**。  
  
7.  在**建议值**部分中，选择**推荐以下值：**，然后单击**添加**。  
  
8.  在**值**和**显示名称**字段，键入**美国**，然后单击**确定**。  
  
9. 重复上述步骤。 在**增值建议**对话框中，键入**JP**中**值**和**显示名称**字段，，然后单击**确定**。  
  
![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
 
    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  
      
 
  
> [!TIP]  
> 可以使用 Active Directory 管理中心中的 Windows PowerShell 历史记录查看器要查找的 Windows PowerShell cmdlet 的 Active Directory 管理中心在执行每个过程。 有关详细信息，请参阅[Windows PowerShell 历史记录查看器](https://technet.microsoft.com/library/hh831702)  
  
下一步是创建资源属性。 在下面的过程中，以使其可用到文件服务器创建将自动添加到资源的全局属性列表域控制器上的资源属性。  
  
#### <a name="to-create-and-enable-pre-created-resource-properties"></a>若要创建并启用预创建的资源属性  
  
1.  在左侧窗格中的 Active Directory 管理中心，单击**树视图**。 展开**动态访问控制**，然后选择**资源属性**。  
  
2.  右键单击**资源属性**，单击**新建**，然后单击**参考资源属性**。  
  
    > [!TIP]  
    > 你还可以选择从资源属性**任务**窗格。 单击**新建**，然后单击**参考资源属性**。  
  
3.  在**选择要共享的索赔类型推荐值列表**，单击**国家/地区**。  
  
4.  在**显示名称**字段中，键入**国家/地区**，然后单击**确定**。  
  
5.  双击**资源属性**列表中，向下滚动到**部门**资源属性。 右键单击，然后依次单击**启用**。 这将使内置**部门**资源属性。  
  
6.  在**资源属性**列表上 Active Directory 管理中心的导航窗格中，你现在将有两个资源启用的属性：  
  
    -   国家/地区  
  
    -   部门  
  
![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  
  
```  
  
下一步是创建定义人可以访问资源的中央访问规则。 在此情况下业务规则是：  
  
-   仅通过财经部门的成员，可以朗读财经文档。  
  
-   财经部门成员都可以访问仅在自己的国家/地区的文档。  
  
-   只有财经管理员可以让写入访问权限。  
  
-   我们可异常 FinanceException 组中的成员。 此组将具有阅读访问权限。  
  
-   管理员帐户和文档所有者将仍然能够的完全访问权限。  
  
或者快速与 Windows Server 2012 构造规则：  
  
定向：Resource.Department 包含财经  
  
使用规则中：  
  
-   允许阅读 User.Country=Resource.Country 和 User.department = Resource.Department  
  
-   允许完全控制 User.MemberOf(FinanceAdmin)  
  
-   允许阅读 User.MemberOf(FinanceException)  
  
#### <a name="to-create-a-central-access-rule"></a>若要创建中心访问规则  
  
1.  在 Active Directory 管理中心左侧窗格中，单击**树视图**、选择**动态访问控制**，然后单击**中央访问规则**。  
  
2.  右键单击**中央访问规则**，单击**新建**，然后单击**中央访问规则**。  
  
3.  在**名称**字段中，键入**财经文档规则**。  
  
4.  在**目标资源**部分中，单击**编辑**，并在**中央访问规则**对话框中，单击**添加条件**。 添加满足以下条件：   
    [**资源**][**部门**][**等于**][**Value**][**财经**]，然后单击**确定**。  
  
5.  在**权限**部分中，选择**为当前权限使用下列权限**，单击**编辑**，并在**的权限高级安全设置**对话框中，单击**添加**。  
  
    > [!NOTE]  
    > **使用以下权限与提出的权限**选项允许你创建该策略临时中。 有关如何执行此操作的详细信息，请参考保持：更改和阶段本主题中的策略部分。  
  
6.  在**权限条目权限**对话框中，单击**选择主体**，类型**验证用户**，，然后单击**确定**。  
  
7.  在**权限条目权限**对话框中，单击**添加条件**，并添加满足以下条件：   
    [**User**][**国家/地区**][**Any of**][**资源**][**国家/地区**]   
     单击**添加条件**。   
     [**And**]   
    单击 [**用户**] [**部门**] [**任一**] [**资源**] [**部门**]。 设置**权限**到**阅读**。  
  
8.  单击**确定**，然后单击**添加**。 单击**选择主体**，类型**FinanceAdmin**，然后单击**确定**。  
  
9. 选择**修改、阅读和执行、读取、写入**权限，然后单击**确定**。  
  
10. 单击**添加**，单击**选择主体**，类型**FinanceException**，然后单击**确定**。 选择要进行的权限**阅读**和**阅读并且执行**。  
  
11. 单击**确定**完成并返回到 Active Directory 管理中心的三倍。  
  
    ![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
    以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
 
    $countryClaimType = Get-ADClaimType 国家/地区  
    $departmentClaimType = Get-ADClaimType 部门  
    $countryResourceProperty = Get-ADResourceProperty 国家/地区  
    $departmentResourceProperty = Get-ADResourceProperty 部门  
    $currentAcl ="O:SYG:SYD:AR(A;;FA;; OW)（一个;FA;;栏）（一个; 0x1200a9;;S-1-5-21-1787166779-1215870801-2157059049-1113)（一个; 0x1301bf;;S-1-5-21-1787166779-1215870801-2157059049-1112)（一个;FA;;SY) (XA; 0x1200a9;;AU;((@USER." + $countryClaimType.Name +"Any_of @RESOURCE。" + $countryResourceProperty.Name +") & & (@USER。" + $departmentClaimType.Name +"Any_of @RESOURCE。" + $departmentResourceProperty.Name +")))"  
    $resourceCondition ="(@RESOURCE。" + $departmentResourceProperty.Name +"包含 {`"Finance`"})"  
    New-ADCentralAccessRule"财务文档规则"CurrentAcl $currentAcl-ResourceCondition $resourceCondition  

  
> [!IMPORTANT]  
> 在 cmdlet 上例中组 FinanceAdmin 安全标识符 (Sid) 和用户取决于每次创建并将会在你的示例中不同。 例如，提供的 SID 值 (S-1-5-21-1787166779-1215870801-2157059049-1113) FinanceAdmins 需要与你将需要创建的部署 FinanceAdmin 组的实际 SID 更换。 你可以使用 Windows PowerShell 以查找 SID 值该组的分配变量，此值，然后使用以下变量。 有关详细信息，请参阅[Windows PowerShell 笔尖：处理 Sid](https://go.microsoft.com/fwlink/?LinkId=253545)。  
  
现在，你应该拥有允许用户从的相同国家/地区和相同部门访问文档中心访问规则。 规则允许 FinanceAdmin 组编辑文档，并允许 FinanceException 组阅读的文档。 此规则面向将其划分为财经的文档。  
  
#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>若要向中心访问策略中添加中心访问规则  
  
1.  在左侧窗格中的 Active Directory 管理中心中，单击**动态访问控制**，然后单击**中央访问策略**。  
  
2.  在**任务**窗格中，单击**新建**，然后单击**中央访问策略**。  
  
3.  在**中央访问策略：**，类型**财经策略**中**名称**框。  
  
4.  在**成员中心访问规则**，单击**添加**。  
  
5.  双击**财经文档规则**添加到**添加以下中心访问规则**列表，然后依次单击**确定**。  
  
6.  单击**确定**才能完成。 现在，你应该拥有名为财经策略中心访问策略。  
  
    ![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
    以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
    ```  
    New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
    -Identity "Finance Policy"   
    -Member "Finance Documents Rule"  
  
    ```  
  
#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>若要使用组策略文件服务器跨应用的中央访问策略  
  
1.  在**开始**屏幕上，在**搜索**框中，键入**组策略管理**。 双击**组策略管理**。  
  
    > [!TIP]  
    > 如果**显示管理工具**禁用设置，则**管理工具**文件夹及其内容将不会显示在**设置**结果。  
  
    > [!TIP]  
    > 在 production 环境中，你应该创建文件服务器组织单元 (OU)，并将你的所有文件服务器都添加到此 OU，你想要将此策略应用。 然后，你可以创建组策略，并将此 OU 添加到该策略。  
  
2.  此步骤中，在你编辑组策略对象在你创建[版本域控制器](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build)部分测试环境包含你创建的中心访问策略中。 在组策略管理编辑器中，导航到和域 (在本例中为 contoso.com) 中选择部门：**组策略管理**，**森林：contoso.com**，**域**，**contoso.com**，**Contoso**，**FileServerOU**。  
  
3.  右键单击**FlexibleAccessGPO**，然后单击**编辑**。  
  
4.  在组策略编辑器中管理窗口中，导航到**计算机配置**，展开**策略**，展开**Windows 设置**，然后单击**安全设置**。  
  
5.  展开**文件系统**，右键单击**中央访问策略**，然后单击**管理中央访问策略**。  
  
6.  在**中央访问策略配置**对话框框中，添加**财经策略**，然后单击**确定**。  
  
7.  向下滚动到**高级审查策略配置**，并将其展开。  
  
8.  展开**审核策略**，然后选择**对象访问**。  
  
9. 双击**审核中心访问策略暂存**。 选择所有三个复选框，然后单击**确定**。 此步骤后，收到审核事件中心访问临时策略相关系统。  
  
10. 双击**审核文件系统属性**。 选中所有三种复选框，然后单击**确定**。  
  
11. 关闭组策略管理编辑器。 现在已内置到组策略的中央访问策略。  
  
对于域域控制器提供索赔或授权设备的数据，域控制器需要进行配置，以支持动态访问控制。  
  
#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>若要启用的索赔支持和 contoso.com 复合身份验证  
  
1.  打开组策略管理，请单击**contoso.com**，然后单击**域控制器**。  
  
2.  右键单击**默认域控制器策略**，然后单击**编辑**。  
  
3.  在组策略编辑器中管理窗口中，双击**计算机配置**，双击**策略**，双击**管理模板**，双击**系统**，然后双击**KDC**。  
  
4.  双击**KDC 支持的索赔，复合身份验证和 Kerberos 程度**。 在**KDC 支持的索赔，复合身份验证和 Kerberos 程度**对话框中，单击**启用**选择**支持**从**选项**下拉列表。 （你需要启用此设置，以使用中心访问策略用户索赔。）  
  
5.  关闭**组策略管理**。  
  
6.  打开命令提示符下，键入`gpupdate /force`。  
  
## <a name="BKMK_1.4"></a>部署中心访问策略  
  
||步骤|示例|  
|-|--------|-----------|  
|3.1|将笔帽分配给相应文件服务器上的共享文件夹中。|将中心访问策略分配到文件服务器的相应的共享文件夹。|  
|3.2|确认访问正确配置。|检查从其他国家/地区和部门用户访问。|  
  
在此步骤，您将分配中心访问策略到文件服务器。 你将登录到已接收你创建了之前的步骤中心访问策略好文件服务器，并指定的共享文件夹的策略。  
  
#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>若要指定到文件服务器中心访问策略  
  
1.  在 HYPER-V 管理器中连接到服务器文件 1。 登录到使用密码使用 contoso\administrator 服务器：**pass@word1**。  
  
2.  打开提升了权限的命令提示符下，键入： **gpupdate /force**。 这将确保组策略更改将你的服务器上的生效。  
  
3.  你还需要恢复 Active Directory 从全球的资源属性。 打开提升了权限的 Windows PowerShell 窗口，然后键入`Update-FSRMClassificationpropertyDefinition`。 单击输入、，然后关闭 Windows PowerShell。  
  
    > [!TIP]  
    > 你还可以通过登录到文件服务器刷新全球资源属性。 若要恢复文件服务器从全球的资源属性，请执行以下  
    >   
    > 1.  登录到文件服务器文件 1 作为 contoso\administrator，使用密码**pass@word1**。  
    > 2.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击**开始**，类型**文件服务器资源管理器**，然后单击**文件服务器资源管理器**。  
    > 3.  在文件服务器资源管理器中，单击**文件分类管理**，右键单击**分类属性**，然后单击**刷新**。  
  
4.  打开 Windows 资源管理器，并在左侧窗格中，单击 d。右键单击驱动器**财经文档**文件夹，然后单击**属性**。  
  
5.  单击**分类**选项卡上，单击**国家/地区**，然后选择**美国**中**值**字段。  
  
6.  单击**部门**，然后依次选择**财经**中**值**字段，然后单击**应用**。  
  
    > [!NOTE]  
    > 请记住，中心访问策略配置的财经部门对于目标文件。 上述步骤标记的国家/地区和商业部的特性与的文件夹中的所有文档。  
  
7.  单击**安全**选项卡，然后单击**高级**。 单击**中央策略**选项卡。  
  
8.  单击**更改**、选择**财经策略**从下拉菜单中，然后单击**应用**。 你可以看到**财经文档规则**策略中列出。 展开项，以查看所有 Active Directory 中创建规则时设置的权限。  
  
9. 单击**确定**返回到 Windows 资源管理器。  
  
在下一步中，你可以确保相应地配置访问。  用户帐户都需要拥有的相应部门特性集（将其设使用 Active Directory 管理中心）。 若要查看新的策略有效结果的最简单方法是使用**有效访问**Windows 资源管理器中的选项卡。 **有效访问**选项卡显示给定的用户帐户的访问权限。  
  
#### <a name="to-examine-the-access-for-various-users"></a>若要检查一段代码对各种用户的访问权限  
  
1.  在 HYPER-V 管理器中连接到服务器文件 1。 使用 contoso\administrator 登录到服务器上。 导航到 D:\ Windows 资源管理器。 右键单击**财经文档**文件夹，，然后单击**属性**。  
  
2.  单击**安全**选项卡上，单击**高级**，然后单击**有效访问**选项卡。  
  
3.  若要检查一段代码为用户的权限，请单击**选择用户**、键入用户的名称，然后单击**查看有效访问**以查看有效的访问权限。 例如：  
  
    -   Myriam Delesalle (MDelesalle) 处于财经部门，应该可以朗读访问该文件夹。  
  
    -   王华 (MReid) FinanceAdmin 组成员，并且应到文件夹中有修改访问权限。  
  
    -   在财经部门; 的不是 Esther Valle (EValle)但是，她是 FinanceException 组成员，并应拥有阅读访问。  
  
    -   Maira Wenzel (MWenzel) 在财经部门并不不是任一成员 FinanceAdmin 或 FinanceException 组。 她应该没有任何访问到的文件夹。  
  
    请注意，名为的最后一个列**访问受到**有效访问窗口中。 此列显示哪些之门会影响此人的权限。 在此情况下，共享和 NTFS 权限允许所有用户完全控制。 但是，中心访问策略限制基于你之前配置规则访问。  
  
## <a name="BKMK_1.5"></a>保持：更改和阶段策略  
  
||||  
|-|-|-|  
|号码|步骤|示例|  
|4.1|将配置客户端提起的索赔设备|将组策略设置以使设备声明|  
|4.2|使设备的索赔。|使设备的国家/地区索赔类型。|  
|4.3|添加到你想要修改现有中心访问规则转移策略。|修改财经文档规则添加转移策略。|  
|4.4|查看转移策略的结果。|请检查 Ester Velle 权限。|  
  
#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>设置组策略设置以启用索赔设备  
  
1.  登录到 DC1，打开组策略管理，单击**contoso.com**，单击**默认域策略**、右键单击，然后选择**编辑**。  
  
2.  在组策略编辑器中管理窗口中，导航到**计算机配置**，**策略**，**管理模板**，**系统**，**Kerberos**。  
  
3.  选择**Kerberos 客户端支持索赔、复合身份验证和 Kerberos 程度**单击**启用**。  
  
#### <a name="to-enable-a-claim-for-devices"></a>若要启用的设备的声明  
  
1.  打开在 Hyper-V 管理器和日志中服务器 DC1 作为 contoso\Administrator，使用密码**pass@word1**。  
  
2.  从**工具**菜单上，打开 Active Directory 管理中心。  
  
3.  单击**树视图**，展开**动态访问控制**，双击**声称类型**，然后双击**国家/地区**声称。  
  
4.  在**这种类型的索赔可以发出下列类**、选择**计算机**复选框。 单击**确定**。   
    同时**用户**和**计算机**现在选中复选框。 现在可以与除了用户的设备使用国家/地区的索赔。  
  
下一步是创建一个临时策略规则。 可以临时策略用于监视器的新策略项效果之前你启用它。 在下面的步骤，将会影响你的共享文件夹创建一个临时策略项和监视器。  
  
#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>若要创建一个临时策略规则，并将其添加到中心访问策略  
  
1.  打开在 Hyper-V 管理器和日志中服务器 DC1 作为 contoso\Administrator，使用密码**pass@word1**。  
  
2.  打开 Active Directory 管理中心。  
  
3.  单击**树视图**，展开**动态访问控制**，然后选择**中央访问规则**。  
  
4.  右键单击**财经文档规则**，然后单击**属性**。  
  
5.  在**提出权限**部分中，选择**启用权限临时配置**复选框、单击**编辑**，然后单击**添加**。 在**权限条目提出权限**窗口中，单击**选择主体**链接，请键入**验证用户**，，然后单击**确定**。  
  
6.  单击**添加条件**链接并添加满足以下条件：   
     [**User**][**国家/地区**][**Any of**][**资源**][**国家/地区**]。  
  
7.  单击**添加条件**再次，并添加满足以下条件：  
    [**And**]   
     [**设备**][**国家/地区**][**Any of**][**资源**][**国家/地区**]  
  
8.  单击**添加条件**再次，并添加满足以下条件。  
    [和]   
     [**User**][**Group**][**的任何成员**][**值**] \ (**FinanceException**)  
  
9. 若要设置 FinanceException，组中，单击**添加项目**在**选择用户、计算机、服务帐户或组**窗口中，键入**FinanceException**。  
  
10. 单击**权限**、选择**完全控制**，然后单击**确定**。  
  
11. 在提出权限窗口的高级安全设置，请选择**FinanceException**单击**删除**。  
  
12. 单击**确定**两次才能完成。  
  
![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  
  
```  
  
> [!NOTE]  
> 在 cmdlet 上例中服务器值反映服务器的测试实验环境中。 Windows PowerShell 历史记录查看器可用于查找 Windows PowerShell cmdlet 的 Active Directory 管理中心在执行每个过程。 有关详细信息，请参阅[Windows PowerShell 历史记录查看器](https://technet.microsoft.com/library/hh831702)  
  
中设置此提出的权限，FinanceException 组成员将能够完整访问文件从其自己的国家/地区时，他们通过与文档的相同国家/地区从设备访问它们。 审核项都可在尝试访问文件的安全日志，当某人财经部门文件服务器。 但是，该策略提升从临时之前，不强制安全设置。  
  
在接下来的过程中，您验证转移策略的结果。 你访问了用户名具有权限根据当前规则共享的文件夹。 Esther Valle (EValle) 是 FinanceException 的成员，并且她当前具有读取权利。 根据我们转移策略 EValle 应该没有任何权利。  
  
#### <a name="to-verify-the-results-of-the-staging-policy"></a>若要验证转移策略结果  
  
1.  作为 contoso\administrator，使用密码上连接到文件服务器文件 1 Hyper-V 管理器和日志中**pass@word1**。  
  
2.  打开 Command Prompt 窗口，然后键入**gpupdate /force**。 这将确保组策略更改将会在服务器上的生效。  
  
3.  在 Hyper-V 管理器中，连接到服务器客户端 1。 关闭当前登录的用户身份登录。 重新启动虚拟机，客户端 1。 然后登录到使用 contoso\EValle 计算机pass@word1。  
  
4.  双击 \\\FILE1\Finance 文档的桌面快捷方式。 EValle 应该仍有权访问这些文件。 切换到文件 1。  
  
5.  打开**事件查看器**来自的快捷方式在桌面上。 展开**Windows 日志**，然后选择**安全**。 打开具有条目**事件 ID 4818**下**中央访问策略临时**任务类别。 你将看到 EValle 已允许访问;但是，根据转移策略，用户将拒绝访问。  
  
## <a name="next-steps"></a>后续步骤  
如果你有如 System Center Operations Manager 中央服务器管理系统，可以还配置监视事件。 这允许管理员监视器中心访问策略的影响之前履行它们。  
  

