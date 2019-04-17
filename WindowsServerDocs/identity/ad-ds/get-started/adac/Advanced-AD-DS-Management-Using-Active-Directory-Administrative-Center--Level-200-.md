---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: "高级广告 DS 管理使用 Active Directory 管理中心 (级别 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>高级广告 DS 管理使用 Active Directory 管理中心 (级别 200)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍了在更多详细信息，包括示例执行常见任务的体系结构和疑难解答信息的更新的 Active Directory 管理中心与新 Active Directory 回收站、 Fine-grained 密码策略和 Windows PowerShell 历史记录查看器。 有关说明，请参阅[Active Directory 管理中心增强功能和 #40; 简介级别 100 & #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Active Directory 管理中心建筑](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [启用和管理 Active Directory 回收站使用 Active Directory 管理中心](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [配置和管理精准密码策略使用 Active Directory 管理中心](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [使用 Active Directory 管理中心 Windows PowerShell 历史记录查看器](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [广告 DS 管理疑难解答](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Active Directory 管理中心建筑  
  
### <a name="adprep-executables-dlls"></a>ADPrep 可执行文件 Dll  
与新回收站、 FGPP，并查看器历史记录的功能，未更改模块和的 Active Directory 管理中心基础的体系结构。  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
示的基础 Windows PowerShell 和一层的新的回收站功能的操作：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>启用和管理 Active Directory 回收站使用 Active Directory 管理中心  
  
### <a name="capabilities"></a>功能  
  
-   Windows Server 2012 Active Directory 管理中心可以配置和管理 Active Directory 回收站的森林中的任何域分区。 不再需要使用 Windows PowerShell 或 Ldp.exe 启用 Active Directory 回收站或还原域分区中的对象。  
  
-   具有高级 Active Directory 管理中心，筛选条件，在较大的环境中与许多有意已删除的对象简化有针对性的还原。  
  
### <a name="limitations"></a>限制  
  
-   Active Directory 管理中心只能管理域分区，因为它无法从配置、 域 DNS 或森林 DNS 分区 （不能删除架构分区从对象） 中还原已删除的对象。 若要恢复分区非域对象，使用[还原 ADObject](https://technet.microsoft.com/library/ee617262.aspx)。  
  
-   Active Directory 管理中心无法还原子树中一项操作的对象。 例如，如果你删除 OU 与嵌套华丽绚烂、 用户、 组和计算机，还原基本 OU 不还原孩子对象。  
  
    > [!NOTE]  
    > 有点已删除的对象 Active Directory 管理中心批还原操作的功能"最大努力"*在所选内容*以便家长之前还原列表中的孩子有序。 简单测试用例，在的对象子树木可能在一项操作还原。 但角的情况下，选择包含部分树与某些已删除的家长节点缺少树-例如或错误的情况下，如跳过孩子对象当父还原失败，可能无法按预期工作。 出于此原因，你应始终的对象子树单独的操作为后还原还原家长对象。  
  
Active Directory 回收站需要 Windows Server 2008 R2 森林功能级别，并且你必须是管理员企业组的成员。 启用后，你不能禁用 Active Directory 回收站。 Active Directory 回收站增大 Active Directory 数据库 (NTDS。DIT) 森林中的每个域控制器上。 增加随着时间的推移，因为它保留对象和他们的所有特性数据将继续由回收站中的磁盘空间。  
  
有关详细信息，请参阅[Active Directory 回收站 Bin 分步指南](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx)和[评估 （的 Active Directory 回收站） 的硬件要求](https://technet.microsoft.com/library/cc753439(WS.10).aspx)。  
  
你可以*低*森林和域功能级别从 Windows Server 2012 对 Windows Server 2008 R2，甚至后启用 Active Directory 回收站。 这就需要使用**组 ADForestMode**和**组 ADDomainMode** Active Directory cmdlet。  
  
例如：  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
你无法使用 Active Directory 管理中心进行此项更改，它仅引发功能级别。  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>启用 Active Directory 回收站使用 Active Directory 管理中心  
若要启用 Active Directory 回收站，请打开**Active Directory 管理中心**单击你林导航窗格中的名称。 从**任务**窗格中，单击**启用回收站**。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
显示 Active Directory 管理中心**启用回收站 Bin 确认**对话框。 此对话框发出警告的启用了回收站中的是不可逆。 单击**确定**启用 Active Directory 回收站。 Active Directory 管理中心显示另一个来提醒你，Active Directory 回收站不能正常工作直到所有域控制器都复制配置更改的对话。  
  
> [!IMPORTANT]  
> 若要启用 Active Directory 回收站的选项将不可用如果：  
>   
> -   森林功能级别小于 Windows Server 2008 R2  
> -   已启用  
  
等效 Active Directory 的 Windows PowerShell cmdlet 仍然是：  
  
```  
Enable-ADOptionalFeature  
```  
  
有关如何使用 Windows PowerShell 以使 Active Directory 回收站的详细信息，请参阅[Active Directory 回收站 Bin 分步指南](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx)。  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>管理 Active Directory 回收站使用 Active Directory 管理中心  
此部分中使用的示例中的现有域名为**corp.contoso.com**。此域可以用户整理到 OU 名为家长**用户帐户**。 **用户帐户**OU 包含三个孩子华丽绚烂部门，由名为每个包含更多华丽绚烂、 用户和组。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>存储和筛选  
Active Directory 回收站保留所有对象树林中删除。 这节省根据这些对象**msDS deletedObjectLifetime**特性，它默认设置为匹配**tombstoneLifetime**的森林属性。 在任何森林创建使用 Windows Server 2003 SP1 或更高版本的值**tombstoneLifetime**默认设置为 180 天。 任何森林从 Windows 2000 升级或安装与 Windows Server 2003 (任何 service pack) 中并非设置了默认 tombstoneLifetime 属性，并 Windows 因此使用 60 天的内部默认。 所有这些方式是配置。你可以使用 Active Directory 管理中心还原从域分区的树林中删除任何物体。 你必须继续使用 cmdlet**还原 ADObject**到还原已删除从其他分区，如 Configuration.Enabling 使 Active Directory 回收站的对象**删除对象**容器下 Active Directory 管理中心在每个域分区可见。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
**删除对象**容器向你显示所有可还原的对象该域分区中。 删除对象早于**msDS deletedObjectLifetime**称为回收对象。 Active Directory 管理中心回收的对象未显示，并且你无法还原使用 Active Directory 管理中心这些对象。  
  
回收站的体系结构并处理规则的更深入的说明，请参阅[广告回收站： 了解、 实现，最佳做法，以及疑难解答](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx)。  
  
Active Directory 管理中心人为地限制对象从容器返回到 20000 对象的默认数。 您可以通过单击提升高达 100000 对象此限制**管理**菜单，然后**管理列表选项**。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>还原  
  
##### <a name="filtering"></a>筛选  
Active Directory 管理中心提供了强大的条件以及你应熟悉之前，你需要在现实世界还原中使用这些的筛选选项。 域有意删除其生命周期内的多的对象。使用 180 天可能已删除的对象生命周期内，你不能只需恢复所有对象发生意外事故发生时。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
而不是编写复杂 LDAP 筛选器，而且 UTC 值转换日期和时间，使用普通和高级**筛选器**菜单以列出相关的对象。 如果你知道删除天的对象名称或任何其他重要数据，为您的优势时使用筛选。 通过单击 v 形符号右侧的搜索框中，切换高级的筛选选项。  
  
还原操作支持所有常用的筛选器条件选项，任何其他搜索相同。 内置的筛选器的重要还原对象的通常是：  
  
-   *ANR (明确的名字分辨率-未列在菜单上，但当在你键入时的用途 *** 筛选器 *** 框)*  
  
-   上次之间修改给定的日期  
  
-   对象是用户/inetorgperson/计算机月组/组织设备  
  
-   名称  
  
-   当已删除  
  
-   最后的已知的父  
  
-   键入  
  
-   描述  
  
-   城市  
  
-   国家/地区国家/地区  
  
-   部门  
  
-   员工 ID  
  
-   名字  
  
-   职务  
  
-   姓氏  
  
-   SAMaccountname  
  
-   省/自治区  
  
-   电话号码  
  
-   UPN  
  
-   邮政编码  
  
你可以添加多个条件。 例如，你可以找到上 9 月 24 删除的所有用户对象<sup>日</sup>2012 伊利诺斯芝加哥，从使用职务管理器。  
  
你还可以添加、 修改或重新排序列标题来计算恢复的对象时提供详细信息。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
有关明确的名字分辨率的详细信息，请参阅[ANR 属性](https://msdn.microsoft.com/library/ms675092(VS.85).aspx)。  
  
##### <a name="single-object"></a>单个对象  
还原已删除的对象一直是一次操作。  Active Directory 管理中心简化该操作。 若要还原已删除的对象，如单个用户：  
  
1.  单击以导航窗格中的 Active Directory 管理中心域名。  
  
2.  双击**删除对象**管理列表中。  
  
3.  右键单击对象，然后单击**还原**，或者单击**还原**从**任务**窗格。  
  
对象将还原到其原始位置。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
单击**还原到 …**更改还原位置。 如果已删除的对象父容器也已删除，但你不想还原家长，这非常有用。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>多个等对象  
你可以还原如 OU 中的所有用户的多个等级别对象。 按 CTRL 键，然后单击你想要还原的一个或多个已删除的对象。 单击**还原**从任务窗格。 你还可以选择所有显示的对象通过按 CTRL 和按键，或使用 SHIFT 和单击的对象一系列。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>多个家长和孩子对象  
若要了解 parent 多-子还原恢复过程，因为 Active Directory 管理中心无法还原已删除的一项操作的对象嵌套的树至关重要。  
  
1.  还原树中最上面的已删除的对象。  
  
2.  还原该家长对象的即时孩子。  
  
3.  还原这些父对象即时孩子。  
  
4.  根据需要重复直到所有对象都还原。  
  
你无法恢复与其父之前还原孩子对象。 尝试此还原返回如下：  
  
**因为非实例或删除的对象父无法执行此操作。**  
  
**最后的已知父**特性显示每个对象的家长关系。 **最后的已知父**特性从已删除的位置还原位置时更改还原家长后刷新 Active Directory 管理中心。 因此，你可以还原该孩子对象时，某个父对象位置不再显示已删除的对象容器辨别的名称。  
  
请考虑管理员意外删除销售 OU，其中包含孩子华丽绚烂和用户的方案。  
  
首先，观察值**最后的已知父**已删除的所有用户的特性以及它如何读取* *OU = Sales\0ADEL:*< guid + 已删除的对象容器区分名称 >** *:  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
快速筛选出不明确的名字销售返回已删除的 OU，然后还原：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
刷新 Active Directory 管理中心以查看已删除的用户对象的最后一个已知父特性更改还原的销售组织单位辨别名为：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
快速筛选出所有销售用户。 按住 CTRL 和键来选择删除的销售用户。 单击**还原**移动从对象**删除对象**容器到销售 OU 其组会员身份和特性保持不变。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
如果**销售**OU 包含自己的孩子华丽绚烂，然后你会还原首先之前还原孩子，孩子华丽绚烂等等。  
  
若要通过指定已删除的家长容器还原已删除的所有嵌套的物体，请参阅[附录 b： 还原多个已删除 Active Directory 对象 （示例脚本）](https://technet.microsoft.com/library/dd379504(WS.10).aspx)。  
  
还原已删除的对象 Active Directory 的 Windows PowerShell cmdlet 是：  
  
```  
Restore-adobject  
  
```  
  
**还原 ADObject**之间 Windows Server 2008 R2 和 Windows Server 2012 未更改 cmdlet 功能。  
  
##### <a name="server-side-filtering"></a>服务器端筛选  
很做的可能段时间后，删除对象容器将累积字形 （或更 100000） 的对象中等和大型企业中有显示所有对象困难。 客户端筛选依赖中 Active Directory 管理中心的筛选器机制，因为它不能显示这些其他对象。 要避开此限制，请使用下面的步骤执行服务器端搜索：  
  
1.  右键单击**删除对象**容器和单击**搜索下此节点**。  
  
2.  单击 v 形图标来公开**+ 添加条件**菜单上，选择并添加**上次修改给定的日期间**。 最后一修改的时间 ( **whenChanged**特性) 是接近的情况下删除;在大多数环境中，它们都是相同。 此查询执行服务器端搜索。  
  
3.  找到使用进一步显示筛选排序、 在结果中，依此类推来还原已删除的对象并通常还原它们。  
  
## <a name="BKMK_FGPP"></a>配置和管理精准密码策略使用 Active Directory 管理中心  
  
### <a name="configuring-fine-grained-password-policies"></a>配置精准密码策略  
Active Directory 管理中心可以让你创建和管理 Fine-Grained 密码策略 (FGPP) 的对象。 Windows Server 2008 引入了 FGPP 功能，但 Windows Server 2012 的第一个图形管理接口对它。 适用于域级别 Fine-Grained 密码策略，并允许重由 Windows Server 2003 所需的单个域密码。 通过创建使用不同的设置的不同 FGPP，单个用户或组获取不同密码策略域中。  
  
有关 Fine-Grained 密码策略的信息，请参阅[广告 DS Fine-Grained 密码和帐户锁定策略 Step-by-Step 指南（Windows Server 2008 R2）](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
在导航窗格中，单击树视图中，您的域、**系统**，单击**密码设置容器**，然后在任务窗格中，单击**新建**和**密码设置**。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>管理精准密码策略  
创建新 FGPP 或编辑现有调出**密码设置**编辑器。 在这里，你将配置所有所需的密码策略，因为必须在 Windows Server 2008 或 Windows Server 2008 R2、 仅现在与专门编辑器。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
填写必 （红色星号） 字段和任何可选字段，然后单击**添加**设置的用户或组接收此策略。 FGPP 替代默认这些指定的安全主体的域策略设置。 在上图中，非常严格策略仅适用于内置管理员帐户，以避免损害。 该策略太复杂的标准用户遵守，但完美高风险仅供 IT 专业人员使用帐户。  
  
你还可以设置优先级和的用户和组策略应用内给定的域。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Fine-Grained 密码策略 Active Directory 的 Windows PowerShell cmdlet 是：  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
Windows Server 2008 R2 和 Windows Server 2012 之间未更改精细密码策略 cmdlet 功能。 为方便起见下, 图说明用于 cmdlet 相关的参数：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Active Directory 管理中心还可以找到的特定用户的应用 FGPP 结果集。 右键单击任何用户，单击**查看结果密码设置 …**打开*密码设置*适用于该用户通过隐式或明确分配的页面：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
检查**属性**的任何用户或组节目**直接关联密码设置**，这是明确指定的 FGPPs:  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
隐式 FGPP 分配不会显示在此处;为此，你必须使用**查看结果密码设置 …**选项。  
  
## <a name="BKMK_HistoryViewer"></a>使用 Active Directory 管理中心 Windows PowerShell 历史记录查看器  
在未来的 Windows 管理为 Windows PowerShell。 在顶部任务自动化框架分层图形的工具，管理最复杂的分布式系统变得一致高效。 你需要了解 Windows PowerShell 才能访问你的全部潜在和最大限度地计算投资的工作方式。  
  
Active Directory 管理中心现在提供了它运行的 Windows PowerShell cmdlet 其参数和值的完整历史记录。 你可以将复制到其他位置适用于研究或修改和重新使用 cmdlet 历史记录。 你可以创建任务笔记协助隔离哪些你 Active Directory 管理中心命令会导致 Windows PowerShell。 你也可以筛选历史记录找到旅游点标识。  
  
Active Directory 管理中心 Windows PowerShell 历史记录查看器的目的在于，以便于您了解通过可行的体验。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
单击 v 形符号 （箭头） 来显示 Windows PowerShell 历史记录查看器。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
然后，创建一个用户或修改的组成员。 使用 Active Directory 管理中心运行指定的参数每个 cmdlet 折叠视图不断更新历史记录查看器。  
  
展开的兴趣以查看所有值提供给 cmdlet 参数任何行项：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
单击**启动任务**菜单创建手动表示法，然后使用 Active Directory 管理中心以创建、 修改或删除的对象。 键入你所执行的操作。  当完成后所做的更改，请选择**结束任务**。 向可折叠便笺更好地了解可以使用你的所有这些操作执行的任务注意组。  
  
例如，若要查看用于更改了用户密码，并从一组删除他的 Windows PowerShell 命令：  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
选择显示所有复选框还将显示获取-* 仅检索了数据的 Windows PowerShell cmdlet 的动词。  
  
![高级的广告 DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
查看历史记录显示文本命令运行 Active Directory 管理中心，你可能注意到了一些 cmdlet 出现不必要地运行。 例如，可以创建新的用户：  
  
```  
new-aduser   
```  
  
并不需要使用：  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
最小的代码的使用情况和模块化，所需 Active Directory 管理中心的设计。 因此，而不是一套创建新的用户的功能和修改现有的用户的另一组中，它最小不每个功能，然后将它们链接 cmdlet 与。 当您将学习 Active Directory 的 Windows PowerShell 时，请记住这一点。 你还可以使用该作为学习技术，你看到如何只是你可以使用 Windows PowerShell 完成了一项任务。  
  
## <a name="BKMK_Tshoot"></a>广告 DS 管理疑难解答  
  
### <a name="introduction-to-troubleshooting"></a>故障排除简介  
由于其相对新功能和现有客户环境中使用缺乏，Active Directory 管理中心有限故障排除的选项。  
  
### <a name="troubleshooting-options"></a>故障排除的选项  
  
#### <a name="logging-options"></a>登录选项  
Active Directory 管理中心现在包含内置 Windows Server 2012，在作为登录跟踪配置文件的一部分。 创建月修改 dsac.exe 相同的文件夹中的以下文件：  
  
**dsac.exe.config**  
  
创建以下内容：  
  
```  
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>   
 <trace autoflush="false" indentsize="4">   
  <listeners>   
   <add name="myListener"   
    type="System.Diagnostics.TextWriterTraceListener"   
    initializeData="dsac.trace.log" />   
   <remove name="Default" />   
  </listeners>   
 </trace>   
</system.diagnostics>  
  
```  
  
详细级别，以便进行**DsacLogLevel**是**无**，**错误**，**警告**，**信息**，并**详细**。 输出文件名是配置，并将写入 dsac.exe 相同的文件夹。 输出可以告诉你更多有关如何 ADAC 正在运行的域控制器它联系，执行何种 Windows PowerShell 命令、 响应已以及更多详细信息。  
  
例如，在使用信息级别时，这返回除跟踪级别详细级别的所有结果：  
  
-   DSAC.exe 启动  
  
-   日志记录启动  
  
-   请求退货初始域信息域控制器  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   从域 Corp 返回的域控制器 DC1  
  
-   广告的虚拟驱动器加载电源  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   获取域根 DSE 信息  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   获取域广告回收站 bin 信息  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   获取广告森林  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   获取受支持的加密类型，FGPP，某些用户信息架构的信息  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
    -Properties:lDAPDisplayName  
    -ResultPageSize:"100"  
    -ResultSetSize:$null  
    -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
    -SearchScope:"OneLevel"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
    [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   获取有关要向单击域头的管理员显示的域的所有信息。  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -IncludeDeletedObjects:$false  
    -LDAPFilter:"(objectClass=*)"  
    -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
    -ResultPageSize:"100"  
    -ResultSetSize:"20201"  
    -SearchBase:"DC=corp,DC=contoso,DC=com"  
    -SearchScope:"Base"  
    -Server:"dc1.corp.contoso.com"  
  
    ```  
  
设置的详细级别还将显示为每个功能，.NET 堆栈，但它们不包括足以除时疑难解答遭受可访问冲突或系统崩溃 Dsac.exe 尤其有用的数据。  
  
> [!IMPORTANT]  
> 另外，还有称为的服务的带版本[Active Directory 管理网关](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852)，其中运行 Windows Server 2008 SP2 和 Windows Server 2003 SP2。  
  
此问题的两个可能的原因是：  
  
-   在任何访问域控制器上未运行 ADWS 服务。  
  
-   从计算机运行 Active Directory 管理中心网络通信阻止 ADWS 服务  
  
将是无 Active Directory Web 服务实例可用时显示的错误：  
  
|||  
|-|-|  
|错误|操作|  
|"无法连接到任何域。 恢复或再试一次连接时可用"|显示在开始 Active Directory 管理中心应用程序|  
|"找不到中可用的服务器* <NetBIOS domain name> *域运行 Active Directory Web 服务 (ADWS)"|当尝试选择域节点 Active Directory 管理中心应用程序中显示|  
  
若要解决此问题，请按照以下步骤：  
  
1.  验证 Active Directory Web 服务服务启动至少一个和域控制器的域中 (最好的所有域控制器树林中)。 请确保将它设置为所有域控制器上的自动启动。  
  
2.  从计算机上运行 Active Directory 管理中心，验证你可以找到服务器运行 ADWS 通过运行这些 NLTest.exe 命令：  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    这些测试失败，即使 ADWS 服务正在运行，该问题是与名称分辨率 LDAP 和不 ADWS 或 Active Directory 管理中心。 此测试失败，错误"1355年 0x54B ERROR_NO_SUCH_DOMAIN"如果 ADWS 上未运行任何域控制器尽管，因此仔细检查达到任何结论之前。  
  
3.  返回 NLTest 域控制器，转储命令聆听端口列表：  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    检查 ports.txt 文件和验证 ADWS 服务侦听 9389 端口。 示例：  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    如果倾听，验证 Windows 防火墙规则，并确保它们允许 9389 TCP 站。 默认情况下，域控制器启用了防火墙规则"Active Directory Web 服务 （TCP 以）"。 如果不倾听，再次验证在此服务器上运行的服务，并重新启动它。 验证任何其他进程已经正在侦听 9389 端口。  
  
4.  安装网络监视器或其他网络捕获实用程序和返回 NLTEST 域控制器上运行 Active Directory 管理中心的计算机。 收集同时网络捕获从你开始 Active Directory 管理中心并停止捕获前看到此错误，这两台计算机。 验证可以从端口 TCP 9389 上的域控制器到发送和接收客户端。 如果数据包才会发送，但从未到达，或到达和域控制器回复，但它们永远不会联系客户端，则可能是之间放该端口数据包网络上计算机的防火墙。 此防火墙软件或硬件，可能并可能是第三方端点保护 （防病毒） 软件的一部分。  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>/ 可能已知问题以及支持的方案  
使用 Active Directory 管理中心仅可能问题是无法连接到 Active Directory Web 服务 (ADWS) Windows Server 2012 或 Windows Server 2008 R2 域控制器上运行。  
  
## <a name="see-also"></a>请参阅  
[Active Directory 管理中心增强功能和 #40; 简介级别 100 & #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

