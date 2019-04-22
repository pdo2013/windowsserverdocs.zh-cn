---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc2aaa9f7c7c42b6e94995ff473a580ce560ed93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819998"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题详细介绍了更新的 Active Directory 管理中心及其新的 Active Directory 回收站、细化密码策略，以及更详细的 Windows PowerShell 历史记录查看器，包括体系结构、常见任务示例和疑难解答信息。 有关的介绍，请参阅[Active Directory 管理中心增强功能简介&#40;级别 100&#41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)。  
  
- [Active Directory 管理中心体系结构](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
- [启用和管理 Active Directory 回收站使用 Active Directory 管理中心](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
- [配置和管理使用 Active Directory 管理中心细化密码策略](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
- [使用 Active Directory 管理中心 Windows PowerShell 历史记录查看器](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
- [AD DS 管理工具版疑难解答](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Active Directory 管理中心体系结构  
  
### <a name="active-directory-administrative-center-executables-dlls"></a>Active Directory 管理中心可执行文件 Dll  

未使用新的回收站、FGPP 和历史记录查看器功能更改 Active Directory 管理中心的模块和基础体系结构。  
  
- Microsoft.ActiveDirectory.Management.UI.dll  
- Microsoft.ActiveDirectory.Management.UI.resources.dll  
- Microsoft.ActiveDirectory.Management.dll  
- Microsoft.ActiveDirectory.Management.resources.dll  
- ActiveDirectoryPowerShellResources.dll  
  
新回收站功能的基础 Windows PowerShell 和操作层如下图所示：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>启用和管理 Active Directory 回收站使用 Active Directory 管理中心  
  
### <a name="capabilities"></a>功能  
  
- Windows Server 2012 或更高版本的 Active Directory 管理中心内，可配置和管理 Active Directory 回收站的林中任何域分区。 不再需要使用 Windows PowerShell 或 Ldp.exe 来启用 Active Directory 回收站或还原域分区中的对象。
- Active Directory 管理中心具有高级筛选条件，这使你可以在包含许多有意删除的对象的大型环境中轻松进行定向还原。
  
### <a name="limitations"></a>限制  
  
- 因为 Active Directory 管理中心只能管理域分区，所以它无法从“配置”、“域 DNS”或“林 DNS”分区中还原删除的对象（你无法从“架构”分区中删除对象）。 若要从非域分区中还原对象，请使用 [Restore-ADObject](https://technet.microsoft.com/library/ee617262.aspx)。  

- Active Directory 管理中心无法在单个操作中还原对象的子树。 例如，如果你删除带有嵌套的 OU、用户、组和计算机的 OU，则还原基本 OU 不会还原子对象。  
  
    > [!NOTE]  
    > Active Directory 管理中心批处理还原操作进行"最佳效果"的已删除的对象*中所选内容*以便在还原列表子级之前父项进行排序。 在简单的测试用例中，可能会在单个操作中还原对象的子树。 但极端案例，如包含部分树的某些缺少的已删除的父节点的树的选定内容或错误的情况下，例如，当父还原失败，则可能无法按预期方式工作时跳过子对象。 为此，在还原父对象后，你应该始终通过独立操作还原对象的子树。  
  
Active Directory Recycle 回收站需要 Windows Server 2008 R2 林功能级别，并且必须是 Enterprise Admins 组的成员。 一旦启用，则不能禁用 Active Directory 回收站。 Active Directory 回收站将增大林中每个域控制器上的 Active Directory 数据库 (NTDS.DIT) 大小。 随着时间的推移，回收站使用的磁盘空间将继续增大，因为它保留对象及其所有属性数据。  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心启用 Active Directory 回收站

若要启用 Active Directory 回收站，请打开 **Active Directory 管理中心** ，然后在导航窗格中单击你的林的名称。 从**任务**窗格中，单击“启用回收站”。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Active Directory 管理中心显示“启用回收站确认”  对话框。 此对话框警告你启用回收站操作是不可逆的。 单击“确定”以启用 Active Directory 回收站。 Active Directory 管理中心将显示另一个对话框，以提醒你在所有域控制器都复制配置更改之后，Active Directory 回收站才能实现完整功能。  
  
> [!IMPORTANT]  
> 在以下情况下，用于启用 Active Directory 回收站的选项不可用：  
>
> - 林功能级别低于 Windows Server 2008 R2  
> - 该选项已启用  

等效 Active Directory Windows PowerShell cmdlet 为：  

```powershell
Enable-ADOptionalFeature  
```

有关使用 Windows PowerShell 启用 Active Directory 回收站的详细信息，请参阅 [Active Directory 回收站循序渐进指南](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#active-directory-recycle-bin-step-by-step)。  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心管理 Active Directory 回收站

本部分使用名为 **corp.contoso.com**的现有域的示例。 此域将用户组织到名为“UserAccounts” 的父 OU 中。 “UserAccounts”  OU 包含三个按部门命名的子 OU，每个 OU 又进一步包含 OU、用户和组。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>存储和筛选

Active Directory 回收站可保留在林中删除的所有对象。 它将根据 **msDS deletedObjectLifetime** 属性保存这些对象，默认情况下，该属性将设置为与林的 **tombstoneLifetime** 属性相匹配。 在使用 Windows Server 2003 SP1 或更高版本创建的任何林中，默认情况下， **tombstoneLifetime** 的值设置为 180 天。 在从 Windows 2000 升级或随 Windows Server 2003（没有 Service Pack）一起安装的任何林中，未设置默认 tombstoneLifetime 属性，因此 Windows 将使用内部默认的 60 天。 所有内容都可配置。你可以使用 Active Directory 管理中心还原从林的域分区中删除的任何对象。 你必须继续使用 cmdlet **Restore-adobject** 还原其他分区（例如“配置”）中删除的对象。启用 Active Directory 回收站可使**已删除对象**容器在 Active Directory 管理中心的每个域分区下可见。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
**已删除对象**容器向你显示该域分区中的所有可还原对象。 早于 **msDS-deletedObjectLifetime** 的已删除对象称为已回收对象。 Active Directory 管理中心不会显示已回收对象，并且你无法使用 Active Directory 管理中心还原这些对象。  
  
有关回收站的体系结构和处理规则的更深入说明，请参阅[AD 回收站：了解、 实现、 最佳做法和故障排除](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx)。  
  
Active Directory 管理中心人为地将从容器返回的默认对象数量限制为 20,000 个对象。 通过依次单击“管理”菜单和“管理列表选项”，可以将此上限增加到 100,000 个对象。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>还原  
  
##### <a name="filtering"></a>筛选

Active Directory 管理中心将提供强大的条件和筛选选项，你应该先熟悉它们，然后才能在实际还原中进行使用。 域会有意地在其生存期内删除许多对象。由于已删除对象的生存期可能为 180 天，因此你不能在发生意外时简单地还原所有对象。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
使用基本和高级“筛选器”  菜单以仅列出相关对象，而不是编写复杂的 LDAP 筛选器并将 UTC 值转换为日期和时间。 如果你知道删除日期、对象名称或任何其他关键数据，你可以在筛选时利用这一优势。 通过单击搜索框右侧的 V 形图标切换高级筛选器选项。  
  
与任何其他搜索一样，还原操作支持所有标准筛选器条件选项。 对于内置筛选器，用于还原对象的重要选项通常如下：  
  
- *ANR (模糊名称解析-未列在菜单上，但在键入时将使用 * * * 筛选器 * * * 框)*  
- 给定日期之间的最后修改时间  
- 对象是用户/inetorgperson/计算机/组/组织单位  
- 名称  
- 删除时间  
- 最后一个已知的父对象  
- 在任务栏的搜索框中键入  
- 描述  
- 城市  
- 国家/地区  
- 部门  
- 员工 ID  
- 名字  
- 职务  
- 姓氏  
- SAMaccountname  
- 州/省  
- 电话号码  
- UPN  
- ZIP/邮政编码  

你可以添加多个条件。 例如，可以找到在 2012 年 9 月 24 日从伊利诺斯州芝加哥市删除与管理器的职务的所有用户对象。
  
你还可以在评估要恢复哪些对象时添加、修改或重新排序列标题，以提供更多详细信息。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
有关模糊名称解析的详细信息，请参阅 [ANR 属性](https://msdn.microsoft.com/library/ms675092(VS.85).aspx)。  
  
##### <a name="single-object"></a>单个对象

还原已删除对象一直都是单个操作。  Active Directory 管理中心使该操作更容易。 若要还原已删除对象（例如单个用户），请执行以下操作：  
  
1. 在 Active Directory 管理中心的导航窗格中单击域名。  
2. 在管理列表中，双击“已删除对象”。  
3. 右键单击该对象，然后单击“还原” ，或者从“任务”  窗格中单击“还原”  。  
  
该对象将还原到其原始位置。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
单击**还原到...** 更改还原位置。 如果已删除的对象的父容器同样已被删除，但不是想还原父，这非常有用。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>多个对等对象

你可以还原多个对等级别对象，例如 OU 中的所有用户。 按住 CTRL 键并单击你想要还原的一个或多个已删除的对象。 从“任务”窗格中单击“还原”  。 还可以通过按住 CTRL 键和 A 键选择显示的所有对象，或者使用 SHIFT 键和单击来选择某个范围内的对象。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>多个父对象和子对象

了解多个父子还原的还原过程很重要，因为 Active Directory 管理中心无法通过单个操作还原已删除对象的嵌套树。  
  
1. 还原树中最顶层的已删除对象。  
2. 还原该父对象的直属子对象。  
3. 还原这些父对象的直属子对象。  
4. 根据需要重复操作，直至还原所有对象。  
  
在还原父对象之前，无法还原其子对象。 尝试此还原将返回以下错误：  
  
“无法执行该操作，因为未实例化或删除该对象的父对象。”  
  
**最后一个已知的父对象**属性显示每个对象的父关系。 当你在还原父对象后刷新 Active Directory 管理中心时， **最后一个已知的父对象** 属性将从已删除位置更改为已还原位置。 因此，可以在父对象的位置不再显示已删除的对象容器的可分辨的名称时还原该子对象。  
  
请考虑管理员意外删除包含子 OU 和用户的销售 OU 的情况。  
  
首先，观察的值**最后一个已知的父**的所有已删除的用户的属性以及它读取 **OU = Sales\0ADEL:*< guid + 已删除的对象容器可分辨名称 > * * *:  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
筛选模糊名称“销售”，以返回你随后将还原的已删除 OU：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
刷新 Active Directory 管理中心内，若要查看已删除的用户对象的最后一个已知的父对象属性更改为已还原销售 OU，可分辨名称：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
筛选所有“销售”用户。 按住 CTRL 键和 A 键以选择所有已删除的“销售”用户。 单击“还原”  ，以将对象从“已删除对象”  容器移动到销售 OU，并使其组成员身份和属性保持不变。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
如果“销售”  OU 包含了它自己的子 OU，则你需要先还原子 OU，然后再还原其子项，依此类推。  
  
若要通过指定已删除的父容器还原所有嵌套的已删除的对象，请参阅[附录 b:还原多个已删除 Active Directory 对象 （示例脚本）](https://technet.microsoft.com/library/dd379504(WS.10).aspx)。  
  
用于还原已删除对象的 Active Directory Windows PowerShell cmdlet 为：  

```powershell
Restore-adobject  
```

**Restore-ADObject** cmdlet 功能未在 Windows Server 2008 R2 和 Windows Server 2012 之间发生更改。  
  
##### <a name="server-side-filtering"></a>服务器端筛选

随着时间的推移，大中型企业中的已删除对象容器可能会累计超过 20,000（甚至 100,000）个对象，并且很难显示所有对象。 由于 Active Directory 管理中心中的筛选器机制依赖于客户端筛选，因此它无法显示这些其他的对象。 若要解决此限制，请使用以下步骤执行服务器端搜索：  
  
1. 右键单击“已删除对象”容器并单击“在此节点下搜索”。  
2. 单击 V 形图标以显示“+添加条件”菜单，选择并添加“给定日期之间的最后修改时间”。 最后修改时间（**whenChanged** 属性）近似于删除时间；在大多数环境中，它们是相同的。 此查询执行服务器端搜索。  
3. 通过在结果中使用进一步显示筛选、排序等来找到要还原的已删除对象，然后以正常方式还原它们。  
  
## <a name="BKMK_FGPP"></a>配置和管理使用 Active Directory 管理中心细化密码策略  
  
### <a name="configuring-fine-grained-password-policies"></a>配置细化密码策略

Active Directory 管理中心使你能够创建和管理细化密码策略 (FGPP) 对象。 Windows Server 2008 引入了 FGPP 功能，但 Windows Server 2012 具有它的第一个图形管理界面。 你可在域级别上应用细化密码策略，它能够替代 Windows Server 2003 所需的单个域密码。 通过创建具有不同设置的不同 FGPP，单个用户或组可在域中获取不同的密码策略。  
  
有关细化密码策略的信息，请参阅 [AD DS 细化密码和帐户锁定策略分步指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
在“导航”窗格中，依次单击“树视图”、你的域、“系统” 和“密码设置容器” ，然后在“任务”窗格中，单击“新建”  和“密码设置” 。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>管理细化密码策略

创建新 FGPP 或编辑现有 FGPP 将启动“密码设置”编辑器。 你可从此处配置所有所需的密码策略，就像在 Windows Server 2008 或 Windows Server 2008 R2 中执行该操作一样，区别仅在于现在使用专用于该目的的编辑器。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
填写所有必需（红色星号）字段和任何可选字段，然后单击“添加”  以设置将接收此策略的用户或组。 对于这些指定的安全主体，FGPP 将替代默认域策略设置。 在上图中，仅将非常严格的策略用于内置管理员帐户以防止泄露。 该策略太复杂，标准用户很难符合此策略，但它非常适合仅由 IT 专业人员使用的高风险帐户。  
  
你还可设置优先级以及该策略将应用到给定域中的哪些用户和组。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
用于细化密码策略的 Active Directory Windows PowerShell cmdlet 是：  
  
```powershell
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
```

细化密码策略 cmdlet 功能未在 Windows Server 2008 R2 和 Windows Server 2012 之间发生更改。 为了方便起见，下图说明了 cmdlet 的相关联参数：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Active Directory 管理中心还允许你为特定用户查找生成的已应用 FGPP 组。 右键单击任何用户，然后单击**查看生成的密码设置...** 以打开*密码设置*应用于该用户通过隐式或显式分配的页：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
检查任何用户或组的“属性”  将显示“直接关联的密码设置” ，它们是显式分配的 FGPP：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
隐式 FGPP 分配; 不会显示为此，必须使用**查看生成的密码设置...** 选项。  
  
## <a name="BKMK_HistoryViewer"></a>使用 Active Directory 管理中心 Windows PowerShell 历史记录查看器

Windows PowerShell 是 Windows 管理的未来。 通过利用任务自动化框架上层的分层图形工具，可以一致并高效地管理最复杂的分布式系统。 你需要了解 Windows PowerShell 的工作原理，才能发挥你的全部潜能并最大程度利用你在计算方面的投入。  
  
Active Directory 管理中心现在提供了它运行的所有 Windows PowerShell cmdlet 及其参数和值的完整历史记录。 你可以在其他位置复制 cmdlet 历史记录，以供研究或者修改并重复使用。 你可以创建“任务”备注，以帮助隔离 Active Directory 管理中心命令在 Windows PowerShell 中生成的内容。 还可以筛选该历史记录，以查找兴趣点。  
  
Active Directory 管理中心 Windows PowerShell 历史记录查看器的目的是供你了解整个实践经验。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
单击 V 形图标（箭头），以显示 Windows PowerShell 历史记录查看器。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
然后，创建用户或修改组成员身份。 历史记录查看器持续通过一个折叠视图进行更新，该视图显示 Active Directory 管理中心使用指定参数运行的每个 cmdlet。  
  
展开感兴趣的任何行项，以查看向 cmdlet 的参数提供的所有值：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
单击“启动任务”菜单以创建手动表示法，然后使用 Active Directory 管理中心来创建、修改或删除对象。 键入之前正在执行的操作。  完成所做的更改时，请选择“结束任务” 。 任务备注将执行的所有操作汇集到折叠备注中，你可使用它来更好地了解情况。  
  
例如，若要查看用于更改用户密码并从组中删除该用户的 Windows PowerShell 命令：  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
选择“全部显示”复选框还会显示仅用于检索数据的 Get-* 动词 Windows PowerShell cmdlet。  
  
![高级的 AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
历史记录查看器显示由 Active Directory 管理中心运行的文本命令，你可能注意到似乎不必运行某些 cmdlet。 例如，你可以使用以下对象创建新用户：  

```powershell
new-aduser
```

并且无需使用：  

```powershell
set-adaccountpassword  
enable-adaccount  
set-aduser  
```

Active Directory 管理中心的设计需要尽量少用代码和模块化。 因此，它将最小程度地执行每个函数，然后使用 cmdlet 将它们链接在一起，而不是使用用于创建新用户的函数集和用于修改现有用户的其他集。 当你学习 Active Directory Windows PowerShell 时，请记住这一点。 你还可以将其用作学习技术，你将在其中发现使用 Windows PowerShell 完成单个任务是如此简单。  
  
## <a name="BKMK_Tshoot"></a>AD DS 管理工具版疑难解答  
  
### <a name="introduction-to-troubleshooting"></a>疑难解答简介

由于 Active Directory 管理中心在现有客户环境中相对较新且使用经验较少，因此它的疑难解答选项有限。  
  
### <a name="troubleshooting-options"></a>疑难解答选项  
  
#### <a name="logging-options"></a>日志记录选项

Active Directory 管理中心现在包含内置日志记录，跟踪配置文件的一部分。 在 dsac.exe 所在的相同文件夹中创建/修改以下文件：  
  
**dsac.exe.config**
  
创建以下内容：  
  
```xml
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

**DsacLogLevel** 的详细级别为**无**、**错误**、**警告**、**信息**和**详细**。 输出文件名可进行配置，并将写入 dsac.exe 所在的相同文件夹。 该输出可以告诉你有关以下方面的更多信息：如何运行 ADAC、它联系了哪些域控制器、已执行哪些 Windows PowerShell 命令、响应了哪些内容以及进一步的详细信息的。  

例如，在使用“信息”级别时（这会返回除跟踪级别详细级别之外的所有结果），将发生以下事件：  
  
- DSAC.exe 启动  
- 日志记录启动  
- 域控制器已请求返回初始域信息  

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
   ```

- 域控制器 DC1 已从域 Corp 返回  
- PS AD 虚拟驱动器已加载  

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
   ```
  
- 获取域根 DSE 信息  

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
   ```

- 获取域 AD 回收站信息  

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- 获取 AD 林  

   ```  
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- 获取有关支持的加密类型、FGPP、某些用户信息的架构信息  

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

- 获取要向单击了域标头的管理员显示的域对象的所有相关信息。  

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

设置详细级别还将显示每个函数的 .NET 堆栈，但这些堆栈未包含足够的数据，因此除了用于解决 Dsac.exe 的访问冲突或崩溃，它们不是非常有用。 此问题的两个可能原因如下：
  
- ADWS 服务未在任何可访问的域控制器上运行。
- 从运行 Active Directory 管理中心的计算机到 ADWS 服务阻止网络通信。

> [!IMPORTANT]  
> 还存在一个称为 [Active Directory 管理网关](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852)的带外版本的服务，该服务在 Windows Server 2008 SP2 和 Windows Server 2003 SP2 上运行。
>

在没有可用 Active Directory Web 服务实例时显示的错误如下：  
  
|错误|操作|
| --- | --- |  
|“无法连接到任何域。 请在连接可用时刷新或重试”|在 Active Directory 管理中心应用程序启动时显示|
|"找不到中的可用服务器*<NetBIOS domain name>* 运行 Active Directory Web 服务 (ADWS) 的域"|当尝试在 Active Directory 管理中心应用程序中选择域节点时显示|
  
若要解决此问题，请使用以下步骤：  
  
1. 验证 Active Directory Web 服务服务是否在域中至少一个域控制器（最好是林中的所有域控制器）上启动。 还要确保它设置为在所有域控制器上自动启动。
2. 从运行 Active Directory 管理中心的计算机中，验证是否可以通过运行以下 NLTest.exe 命令找到运行 ADWS 的服务器：  

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   如果即使正在运行 ADWS 服务这些测试仍然失败，则该问题与名称解析或 LDAP 相关，而不与 ADWS 或 Active Directory 管理中心相关。 但是，如果 ADWS 未在任何域控制器上运行，则此测试失败并带有错误“1355 0x54B ERROR_NO_SUCH_DOMAIN”，因此请在得出任何结论之前进行复核。  
  
3. 在 NLTest 返回的域控制器上，使用该命令转储侦听的端口列表：  

   ```
   Netstat -anob > ports.txt  
   ```

   检查 ports.txt 文件，并验证 ADWS 服务正在端口 9389 上进行侦听。 例如：  

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  

   TCP    [::]:9389       [::]:0       LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  
   ```

   如果正在侦听，验证 Windows 防火墙规则，并确保它们允许 9389 TCP 入站。 默认情况下，域控制器启用防火墙规则“Active Directory Web 服务 (TCP-in)”。 如果不侦听，再次验证该服务在此服务器上运行，并重新启动它。 验证端口 9389 上不存在正在侦听的任何其他过程。  
  
4. 在运行 Active Directory 管理中心的计算机上和 NLTEST 返回的域控制器上安装 NetMon 或其他网络捕获实用程序。 从两台计算机中收集同时进行的网络捕获 - 你在这些计算机中启动 Active Directory 管理中心并可以在停止捕获之前看到错误。 验证客户端能够在端口 TCP 9389 上与域控制器进行发送和接收操作。 如果数据包已发送但不能到达，或者可以到达并且域控制器可进行回复，但它们永远不会到达客户端，则可能是那个将数据包放在该端口的网络上的计算机之间存在防火墙。 此防火墙可能是软件或硬件，并且可能是第三方端点保护（防病毒）软件的一部分。  
  
## <a name="see-also"></a>请参阅

[AD 回收站、 细化密码策略，PowerShell 历史记录](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
