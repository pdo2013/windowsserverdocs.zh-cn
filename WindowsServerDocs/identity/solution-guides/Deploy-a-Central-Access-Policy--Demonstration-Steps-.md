---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: 部署中央访问策略（示范步骤）
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9505772dcb3ec10ff087856ff0d8cb3832b17c6e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445724"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>部署中央访问策略（示范步骤）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在此方案中，将结合使用财务部门安全操作与中心信息安全性来指定对中心访问策略的需求，以便保护存储在文件服务器上的已存档财务信息。 每个国家/地区的已存档的财务信息可以只读形式由来自同一国家/地区的财务人员访问。 中心财务管理员组可以访问来自所有国家/地区的财务信息。  

中心访问策略的部署包括以下阶段：  

|阶段|描述  
|---------|---------------  
|[计划：确定需要策略和部署所需的配置](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.2)|标识所需的策略和部署所需的配置。 
|[实现：配置组件和策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.3)|配置组件和策略。  
|[部署中心访问策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|部署策略。  
|[维护：更改并暂存策略](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.5)|策略更改和分段测试。 

## <a name="BKMK_1.1"></a>设置测试环境  
在开始之前，需要设置实验室以测试该方案。 设置实验室的步骤进行了详细信息中[附录 b:设置测试环境](Appendix-B--Setting-Up-the-Test-Environment.md)。  

## <a name="BKMK_1.2"></a>计划：标识对策略的需求和部署所需的配置。  
本部分提供了一系列高级步骤，可帮助你规划部署。  

||步骤|示例|  
|-|--------|-----------|  
|1.1|业务确定需要采用中心访问策略|为了保护存储在文件服务器的财务信息，将结合使用财务部门安全操作与中心信息安全性来指定对中心访问策略的需求。|  
|1.2|表示访问策略|只有财务部门的成员才可以读取财务文档。 财务部门的成员只应访问自己国家/地区的文档。 应当只有财务管理员才具有写入访问的权限。 允许为 FinanceException 组的成员设置例外。 该组具有读取访问权限。|  
|1.3|Express 在 Windows Server 2012 构造中的访问策略|目标：<br /><br />-Resource.Department 包含财务部门<br /><br />访问规则：<br /><br />-允许读取 User.Country=Resource.Country AND User.department = Resource.Department<br />-允许完全控制 user.memberof （financeadmin）<br /><br />例外：<br /><br />允许读取 memberOf(FinanceException)|  
|1.4|确定策略所需的文件属性|使用以下属性标记文件：<br /><br />-部门<br />-国家/地区|  
|1.5|确定策略所需的声明类型和组|声明类型：<br /><br />-国家/地区<br />-部门<br /><br />用户组：<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|确定应用此策略的服务器|将此策略应用于所有财务文件服务器。|  

## <a name="BKMK_1.3"></a>实现：配置组件和策略  
本部分提供了有关为财务文档部署中心访问策略的示例。  

|否|步骤|示例|  
|------|--------|-----------|  
|2.1|创建声明类型|创建以下声明类型：<br /><br />-部门<br />-国家/地区|  
|2.2|创建资源属性|创建并启用下列资源属性：<br /><br />-部门<br />-国家/地区|  
|2.3|配置中心访问规则|创建财务文档规则，该规则包含前一部分中所确定的策略。|  
|2.4|配置中心访问策略 (CAP)|创建一个称为“财务策略”的 CAP，并向该 CAP 添加财务文档规则。|  
|2.5|将文件服务器作为中心访问策略的目标|将财务策略 CAP 发布到文件服务器。|  
|2.6|为声明、复合身份验证和 Kerberos 保护启用 KDC 支持。|针对 contoso.com 的声明、复合身份验证和 Kerberos 保护启用 KDC 支持。|  

在以下步骤中，可创建两个声明类型：国家/地区和部门。  

#### <a name="to-create-claim-types"></a>创建声明类型  

1. 打开服务器 DC1 中的 HYPER-V 管理器和日志，以 contoso\administrator 的身份使用密码<strong>pass@word1</strong>。  

2. 打开 Active Directory 管理中心。  

3. 单击“树视图”  图标，展开“动态访问控制”  ，然后选择“声明类型”  。  

   右键单击“声明类型”  ，单击“新建”  ，然后单击“声明类型”  。  

   > [!TIP]  
   > 还可以从“任务”  窗格中打开“创建声明类型:”  窗口。 在“任务”  窗格上，单击“新建”  ，然后单击“声明类型”  。  

4. 在“源属性”  列表中，向下滚动列表属性，然后单击“部门”  。 这应该使用“部门”  来填充“显示名称”  。 单击 **“确定”** 。  

5. 在“任务”  窗格中，单击“新建”  ，然后单击“声明类型”  。  

6. 在“源属性”  列表中，向下滚动属性列表，然后单击“c”  属性（国家/地区名称）。 在“显示名称”  字段中，键入**国家/地区**。  

7. 在“建议值”  部分中，选择“建议使用以下值：”  ，然后单击“添加”  。  

8. 在“值”  和“显示名称”  字段中，键入 **US**，然后单击“确定”  。  

9. 重复上述步骤。 在“添加建议值”  对话框中，在“值”  和“显示名称”  字段中，键入 **JP**，然后单击“确定”  。  

![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  


    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  



> [!TIP]  
> 可以使用 Active Directory 管理中心中的 Windows PowerShell 历史记录查看器，来查找 Windows PowerShell cmdlet，其可用于 Active Directory 管理中心中执行的每个过程。 有关详细信息，请参阅 [Windows PowerShell 历史记录查看器](https://technet.microsoft.com/library/hh831702)  

下一步是创建资源属性。 在下列过程中，将创建资源属性，该属性会自动添加到域控制器上的“全局资源属性”列表，以便其可用于文件服务器。  

#### <a name="to-create-and-enable-pre-created-resource-properties"></a>创建并启用预创建的资源属性  

1.  在 Active Directory 管理中心的左窗格中，单击“树视图”  。 展开“动态访问控制”  ，然后选择“资源属性”  。  

2.  右键单击“资源属性”  ，单击“新建”  ，然后单击“引用资源属性”  。  

    > [!TIP]  
    > 还可以从“任务”  窗格中选择资源属性。 单击“新建”  ，然后单击“引用资源属性”  。  

3.  在“选择要共享其建议值的声明类型”  列表中，单击“国家/地区”  。  

4.  在“显示名称”  字段中，键入“国家/地区”  ，然后单击“确定”  。  

5.  双击“资源属性”  列表中，然后向下滚动到“部门”  资源属性。 右键单击，再单击“启用”  。 这将启用内置“部门”  资源属性。  

6.  在 Active Directory 管理中心导航窗格上的“资源属性”  列表中，现在将存在两个已启用的资源属性：  

    -   国家/地区  

    -   部门  

![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  

```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  

```  

下一步是创建中心访问规则，该规则定义了哪些用户可以访问资源。 在此方案中，业务规则是：  

-   只有财务部门的成员才可以读取财务文档。  

-   财务部门的成员只能在自己的国家/地区访问文档。  

-   仅财务主管才具有写入访问权限。  

-   将允许 FinanceException 组的成员的例外。 该组具有读取访问权限。  

-   管理员和文档所有者仍具有完全访问权限。  

或声明与 Windows Server 2012 构造规则：  

目标：Resource.Department 包含财务部门  

访问规则：  

-   允许读取 User.Country=Resource.Country AND User.department = Resource.Department  

-   允许完全控制 User.MemberOf(FinanceAdmin)  

-   允许读取 User.MemberOf(FinanceException)  

#### <a name="to-create-a-central-access-rule"></a>创建中心访问规则  

1. 在 Active Directory 管理中心的左窗格中，单击“树视图”  ，选择“动态访问控制”  ，然后单击“中心访问规则”  。  

2. 右键单击“中心访问规则”  ，单击“新建”  ，然后单击“中心访问规则”  。  

3. 在“名称”  字段中，键入“财务文档规则”  。  

4. 在“目标资源”  部分中，单击“编辑”  ，然后在 **“中心访问规则”** 对话框中，单击“添加一个条件”  。 添加以下条件：   
   [“资源”  ]、[“部门”  ]、[“等于”  ]、[“值”  ]、[“财务”  ]，然后单击“确定”  。  

5. 在“权限”  部分中，选择“使用下列权限作为当前权限”  ，单击“编辑”  ，然后在“权限的高级安全设置”  对话框中，单击“添加”  。  

   > [!NOTE]  
   > 在暂存期间，可使用“使用下列权限作为建议的权限”  选项来创建策略。 有关如何执行此操作的详细信息，请参阅本主题中的“维护：更改并暂存该策略”部分。  

6. 在“权限的权限条目”  对话框中，单击“选择主体”  ，键入“经过身份验证的用户”  ，然后单击“确定”  。  

7. 在“权限的权限条目”  对话框中，单击“添加条件”  ，然后添加以下条件：   
   [**用户**] [**国家/地区**] [**任一**] [**资源**] [**国家/地区**]   
    单击**添加条件**。   
    [**And**]   
   单击 [**用户**] [**部门**] [**任一**] [**资源**] [**部门**]。 将“权限”  设置为“读取”  。  

8. 单击“确定”  ，然后单击“添加”  。 单击“选择主体”  ，键入 **FinanceAdmin**，然后单击“确定”  。  

9. 选择“修改、读取并执行、读取以及写入”  权限，然后单击“确定”  。  

10. 单击“添加”  ，单击“选择主体”  ，键入 **FinanceException**，然后单击“确定”  。 选择“读取”  和“读取并执行”  权限。  

11. 单击“确定”  三次完成操作，然后返回到 Active Directory 管理中心。  

    ![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

    下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  


~~~
$countryClaimType = Get-ADClaimType country  
$departmentClaimType = Get-ADClaimType department  
$countryResourceProperty = Get-ADResourceProperty Country  
$departmentResourceProperty = Get-ADResourceProperty Department  
$currentAcl = "O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;0x1200a9;;;S-1-5-21-1787166779-1215870801-2157059049-1113)(A;;0x1301bf;;;S-1-5-21-1787166779-1215870801-2157059049-1112)(A;;FA;;;SY)(XA;;0x1200a9;;;AU;((@USER." + $countryClaimType.Name + " Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + " Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
$resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + " Contains {`"Finance`"})"  
New-ADCentralAccessRule "Finance Documents Rule" -CurrentAcl $currentAcl -ResourceCondition $resourceCondition  
~~~


> [!IMPORTANT]  
> 在上述 cmdlet 示例中，将在创建时确定 FinanceAdmin 组和用户组的安全标识符 (SID)，并且它们在示例中会有所不同。 例如，需要将所提供的 FinanceAdmins 组的 SID 值 (S-1-5-21-1787166779-1215870801-2157059049-1113) 替换为要在部署中创建的 FinanceAdmin 组的实际 SID 值。 可以使用 Windows PowerShell 来查找此组的 SID 值，将该值分配给一个变量，并此处使用该变量。 有关详细信息，请参阅[Windows PowerShell 提示：使用 Sid](https://go.microsoft.com/fwlink/?LinkId=253545)。  

现在应具有中心访问规则，该规则允许用户在同一国家/地区及同一部门中访问文档。 该规则允许 FinanceAdmin 组编辑文档，并且它还允许 FinanceException 组读取文档。 该规则仅适用于被归类为“财务”的文档。  

#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>向中心访问策略添加中心访问规则  

1. 在 Active Directory 管理中心的左窗格中，单击“动态访问控”  ，然后单击“中心访问策略”  。  

2. 在“任务”  窗格中，单击“新建”  ，然后单击“中心访问策略”  。  

3. 在“创建中心访问策略：”  中，在“名称”  框中，键入 **Finance Policy**。  

4. 在“成员中心访问规则”  中，单击“添加”  。  

5. 双击“财务文档规则”  ，以将其添加到“添加以下中心访问规则列表”  列表，然后单击“确定”  。  

6. 单击“确定”  以完成操作。 现在应具有名为“财务策略”的中心访问策略。  

   ![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

   下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  

   ```  
   New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
   -Identity "Finance Policy"   
   -Member "Finance Documents Rule"  

   ```  

#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>通过使用组策略跨文件服务器应用中心访问策略  

1.  在“开始”  屏幕的“搜索”  框中，键入“组策略管理”  。 双击“组策略管理”  。  

    > [!TIP]  
    > 如果“显示管理工具”  设置处于禁用状态，“管理工具”  文件夹及其内容将不会出现在“设置”  结果中。  

    > [!TIP]  
    > 在生产环境中，应创建要在其中应用此策略的文件服务器组织单位 (OU)，并将所有文件服务器都添加到该 OU。 然后可以创建组策略，并将此 OU 添加到此策略。  

2.  在此步骤中，你将编辑已在测试环境中的[生成域控制器](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build)部分创建的组策略对象，以包含你所创建的中心访问策略。 在组策略管理编辑器中，导航至域（在本例中为 contoso.com）中的组织单位并将其选中：“组策略管理”  、“林: contoso.com”  、“域”  、“contoso.com”  、“Contoso”  、“FileServerOU”  。  

3.  右键单击“FlexibleAccessGPO”  ，然后单击“编辑”  。  

4.  在“组策略管理编辑器”窗口中，导航至“计算机配置”  ，再依次展开“策略”  和“Windows 设置”  ，然后单击“安全设置”  。  

5.  展开“文件系统”  ，右键单击“中心访问策略”  ，然后单击“管理中心访问策略”  。  

6.  在“中心访问策略配置”  对话框中，添加“财务策略”  ，然后单击“确定”  。  

7.  向下滚动到“高级审核策略配置”  ，并将其展开。  

8.  展开“审核策略”  ，并选择“对象访问”  。  

9. 双击“审核中心访问策略暂存”  。 选中所有三个复选框，然后单击“确定”  。 该步骤允许系统收到与中心访问暂存策略相关的审核事件。  

10. 双击“审核文件系统属性”  。 选中所有三个复选框，然后单击“确定”  。  

11. 关闭“组策略管理编辑器”。 现在，你已经将中心访问策略添加到组策略。  

若要提供声明或设备授权数据的域的域控制器，需要将配置为支持动态访问控制域控制器。  

#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>针对 contoso.com 启用对声明和复合身份验证的支持  

1.  打开“组策略管理”，单击“contoso.com”  ，然后单击“域控制器”  。  

2.  右键单击“默认域控制器策略”  ，然后单击“编辑”  。  

3.  在“组策略管理编辑器”窗口中，依次双击“计算机配置”、“策略”、“管理模板”、“系统”和“KDC”      。  

4.  双击“KDC 支持声明、复合身份验证和 Kerberos 保护”  。 在“KDC 支持声明、复合身份验证和 Kerberos 保护”  对话框中，单击“已启用”  ，然后从“选项”  下拉列表中选择“支持”  。 （需要启用此设置以使用中心访问策略中的用户声明。）  

5.  关闭“组策略管理”  。  

6.  打开命令提示符并键入 `gpupdate /force`。  

## <a name="BKMK_1.4"></a>部署中心访问策略  

||步骤|示例|  
|-|--------|-----------|  
|3.1|将 CAP 分配给文件服务器上相应的共享文件夹。|将中心访问策略分配给文件服务器上相应的共享文件夹。|  
|3.2|验证是否已正确配置访问。|检查来自不同国家/地区和部门的用户的访问权限。|  

在此步骤中，你会将中心访问策略分配给文件服务器。 你将登录到接收之前步骤中创建的中心访问策略的文件服务器，并将该策略分配给某个共享文件夹。  

#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>向文件服务器分配中心访问策略  

1. 在 Hyper-V 管理器中，连接到服务器 FILE1。 登录到服务器通过使用 contoso\administrator 和密码： <strong>pass@word1</strong>。  

2. 打开提升的命令提示符并键入： **gpupdate /force**。 这样可以确保组策略的更改将在你的服务器上生效。  

3. 还需要通过 Active Directory 刷新全局资源属性。 打开提升的 Windows PowerShell 窗口并键入 `Update-FSRMClassificationpropertyDefinition`。 单击“ENTER”，然后关闭 Windows PowerShell。  

   > [!TIP]
   > 还可以通过登录到文件服务器来刷新全局资源属性。 若要通过文件服务器刷新全局资源属性，请执行以下操作  
   > 
   > 1. 登录到文件服务器 FILE1，以 contoso\administrator 的身份使用密码<strong>pass@word1</strong>。  
   > 2. 打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击“开始”  ，然后键入“文件服务器资源管理器”  ，最后单击“文件服务器资源管理器”  。  
   > 3. 在文件服务器资源管理器中，单击“文件分类管理”  ，右键单击“分类属性”  ，然后单击“刷新”  。  

4. 打开 Windows 资源管理器，然后在左窗格中单击“驱动器 D”。右键单击“财务文档”  文件夹，然后单击“属性”  。  

5. 单击“分类”  选项卡，单击“国家/地区”  ，然后在“值”  字段中选择“美国”  。  

6. 单击“部门”  ，然后在“值”  字段中选择“财务”  ，最后单击“应用”  。  

   > [!NOTE]  
   > 请记住，已将中心访问策略配置为针对财务部门的文件。 在前面步骤中，已通过国家/地区和部门属性标记文件夹中的所有文档。  

7. 单击“安全”  选项卡，然后单击“高级”  。 单击“中心策略”  选项卡。  

8. 单击“更改”  ，从下拉菜单中选择“财务策略”  ，然后单击“应用”  。 你可以在此策略中查看列出的“财务文档规则”  。 通过展开项，来查看在 Active Directory 中创建规则时所设置的所有权限。  

9. 单击“确定”  返回到 Windows 资源管理器。  

在下一步骤中，请确保访问权限已正确配置。  用户帐户需要设置相应的“部门”属性（使用 Active Directory 管理中心来设置该属性）。 查看新策略有效结果的最简单方法是使用 Windows 资源管理器中的“有效访问”  选项卡。 “有效访问”  选项卡将显示给定用户帐户的访问权限。  

#### <a name="to-examine-the-access-for-various-users"></a>检查各类用户的访问权限  

1.  在 Hyper-V 管理器中，连接到服务器 FILE1。 使用 contoso\administrator 登录到服务器。 导航至 Windows 资源管理器中的 D:\。 右键单击“财务文档”  文件夹，然后单击“属性”  。  

2.  依次单击“安全”  选项卡、“高级”  选项卡和“有效访问”  选项卡。  

3.  若要检查用户权限，请单击**选择一个用户**，键入用户的名称，然后单击**查看有效访问**以查看有效访问权限。 例如：  

    -   Myriam Delesalle (MDelesalle) 是财务部门的成员，因此应具有文件夹的读取访问权限。  

    -   Miles Reid (MReid) 是 FinanceAdmin 组的成员，因此应具有对文件夹的修改访问权限。  

    -   Esther Valle (EValle) 不是财务部门的成员；但是，她是 FinanceException 组的成员，因此应具有读取访问权限。  

    -   Maira Wenzel (MWenzel) 不是财务部门的成员，并且也不是 FinanceAdmin 或 FinanceException 组的成员。 她不应具有对文件夹的任何访问权限。  

    请注意“有效访问”窗口中名为“访问限制”  的最后一列。 此列会告诉你哪些入口会影响到用户的权限。 在此案例中，共享和 NTFS 权限将允许所有用户进行完全控制。 但是，中心访问策略将根据之前配置的规则来限制访问权限。  

## <a name="BKMK_1.5"></a>维护：更改并暂存策略  

||||  
|-|-|-|  
|编号|步骤|示例|  
|4.1|为客户端配置设备声明|设置组策略设置以启用设备声明|  
|4.2|启用设备的声明。|启用设备的国家/地区声明类型。|  
|4.3|将暂存策略添加到要修改的现有中心访问规则。|修改财务文档规则改以添加暂存策略。|  
|4.4|查看暂存策略的结果。|检查 Velle 的权限。|  

#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>设置组策略设置以启用设备的声明  

1.  登录到 DC1，打开组策略管理，单击“contoso.com”  ，再单击“默认域策略”  ，然后右键单击并选择“编辑”  。  

2.  在“组策略管理编辑器”窗口中，依次导航到“计算机配置”  、“策略”  、“管理模板”  、“系统”  、“Kerberos”  。  

3.  选择“对声明、复合身份验证和 Kerberos 保护的 KDC 客户端支持”  ，并单击“启用”  。  

#### <a name="to-enable-a-claim-for-devices"></a>启用设备的声明  

1. 打开服务器 DC1 中的 HYPER-V 管理器和日志，以 contoso\administrator 的身份使用密码<strong>pass@word1</strong>。  

2. 通过“工具”  菜单，打开“Active Directory 管理中心”。  

3. 单击“树视图”  ，展开“动态访问控制”  ，双击“声明类型”  ，然后双击“国家/地区”  声明。  

4. 在“可为以下类发出此类声明”  中，选中“计算机”  复选框。 单击 **“确定”** 。   
   现在应同时选中“用户”  和“计算机”  复选框。 现在，除了可以与用户一起使用外，国家/地区声明还可以与设备一起使用。  

下一步是创建暂存策略规则。 通过使用暂存策略，可在启用新策略项之前监视它的效果。 在以下步骤中，将创建一个暂存策略项，并监视在共享文件夹上的效果。  

#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>创建暂存策略规则并将其添加到中心访问策略  

1. 打开服务器 DC1 中的 HYPER-V 管理器和日志，以 contoso\administrator 的身份使用密码<strong>pass@word1</strong>。  

2. 打开 Active Directory 管理中心。  

3. 单击“树视图”  ，展开“动态访问控制”  ，并选择“中心访问规则”  。  

4. 右键单击“财务文档规则”  ，然后单击“属性”  。  

5. 在“建议的权限”  部分中，选中“启用权限暂存配置”  复选框，再单击“编辑”  ，然后单击“添加”  。 在“建议权限的权限条目”  窗口中，单击“选择主体”  链接，键入“经过身份验证的用户”  ，然后单击“确定”  。  

6. 单击“添加条件”  链接，然后添加以下条件：   
    [“用户”  ]、[“国家/地区”  ]、[“任何”  ]、[“资源”  ]、[“国家/地区”  ]。  

7. 再次单击“添加条件”  ，并添加以下条件：  
   [**And**]   
    [“设备”  ] [“国家/地区”  ] [“任何”  ] [“资源”[  ] [“国家/地区”  ]  

8. 再次单击“添加条件”  ，并添加以下条件：  
   [和]   
    [**用户**] [**组**] [**任何的成员**] [**值**]\(**FinanceException**)  

9. 若要设置 FinanceException 组，请单击“添加项”  ，并在“选择用户、计算机、服务的帐户或组”  窗口中，键入“FinanceException”  。  

10. 单击“权限”  ，选择“完全控制”  ，然后单击“确定”  。  

11. 在“建议权限的高级安全设置”的窗口中，选择“FinanceException”  ，并单击“删除”  。  

12. 单击“确定”  两次以完成操作。  

![解决方案指南](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  

```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  

```  

> [!NOTE]  
> 在上述 cmdlet 示例中，服务器值反映了测试实验室环境中的服务器。 可以使用 Windows PowerShell 历史记录查看器，来查找用于 Active Directory 管理中心中执行的每个过程的 Windows PowerShell cmdlet。 有关详细信息，请参阅 [Windows PowerShell 历史记录查看器](https://technet.microsoft.com/library/hh831702)  

在此建议的权限集中，当 FinanceException 组成员通过自己国家/地区内的设备进行访问时，该组成员对来自同一国家/地区的文件具有完全访问权限。 当财务部门的人员尝试访问文件时，审核项将在文件服务器安全日志中可用。 但是，在策略从暂存推广之前，不会强制执行安全设置。  

在下一过程中，你将验证暂存策略的结果。 通过使用基于当前规则具有权限的用户名，来访问共享文件夹。 Esther Valle (EValle) 是 FinanceException 组的成员，她当前拥有读取权限。 根据暂存策略，EValle 不应具有任何权限。  

#### <a name="to-verify-the-results-of-the-staging-policy"></a>验证暂存策略结果  

1. 连接到 Hyper-v 管理器和日志中的文件服务器 FILE1，以 contoso\administrator 的身份使用密码<strong>pass@word1</strong>。  

2. 打开“命令提示符”窗口并键入 **gpupdate /force**。 这样可以确保组策略的更改将在该服务器上生效。  

3. 在 Hyper-V 管理器中，连接到服务器 CLIENT1。 注销当前登录的用户。 重新启动虚拟机 CLIENT1。 然后登录计算机，通过使用以 contoso\evalle 的身份pass@word1。  

4. 双击桌面快捷方式\\\FILE1\Finance 文档。 EValle 应仍有权访问文件。 重新切换到 FILE1。  

5. 通过桌面上的快捷方式打开“事件查看器”  。 展开“Windows 日志”  ，然后选择“安全”  。 打开与条目**Event ID 4818**下**中心访问策略暂存**任务类别。 你将看到 EValle 已具有访问权限；但是，根据暂存策略，将拒绝授予用户访问权限。  

## <a name="next-steps"></a>后续步骤  
如果存在中心服务器管理系统（如 System Center Operations Manager），则你还可以配置监视事件。 这使管理员能够在强制执行中心访问策略之前监视这些策略的效果。  


