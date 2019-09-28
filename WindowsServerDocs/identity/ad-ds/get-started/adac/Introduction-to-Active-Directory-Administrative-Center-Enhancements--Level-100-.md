---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduction to Active Directory Administrative Center Enhancements (Level 100)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a3bd82feb3a0caf827091bd0cb10edf991921b3c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390625"
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction to Active Directory Administrative Center Enhancements (Level 100)

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server 中的 Active Directory 管理中心包含以下各项的管理功能：

- [Active Directory 回收站](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)
- [细化密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)
- [Windows PowerShell 历史记录查看器](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Active Directory 回收站

Active Directory 域服务 (AD DS) 和 Active Directory 轻型目录服务 (AD LDS) 的用户会经常遇到意外删除 Active Directory 对象的情况。 在以前版本的 Windows Server 中，在 Windows Server 2008 R2 之前，可以在 Active Directory 中恢复意外删除的对象，但这些解决方案有其缺点。

在 Windows Server 2008 中，可以使用 Windows Server Backup 功能和 **ntdsutil** 授权还原命令，将对象标记为授权，以确保在整个域中复制了还原的数据。 授权还原解决方案的缺点在于，它必须在目录服务还原模式 (DSRM) 下才能执行。 在 DSRM 过程中，要还原的域控制器必须保持脱机状态。 因此，它无法处理客户端请求。

在 Windows Server 2003 Active Directory 和 Windows Server 2008 AD DS 中，可以通过逻辑删除恢复来恢复删除的 Active Directory 对象。 不过，恢复对象的已物理删除的链接值属性（例如，用户帐户的组成员身份）和已清除的未链接值属性不会恢复。 因此，管理员无法依靠逻辑删除恢复作为意外删除对象的最终解决方案。 有关逻辑删除恢复的详细信息，请参阅 [恢复 Active Directory 逻辑删除对象](https://go.microsoft.com/fwlink/?LinkID=125452)。

从 Windows Server 2008 R2 开始，Active Directory 回收站构建在现有逻辑删除恢复基础结构上，增强了保留和恢复意外删除的 Active Directory 对象的功能。

启用 Active Directory 回收站后，会保留已删除 Active Directory 对象的所有链接值属性和未链接值属性，并将整个对象还原到与被删除前一致的逻辑状态。 例如，还原的用户帐户会自动重新获得被删除前所具有的域中或跨域的所有组成员身份和相应的访问权限。 Active Directory 回收站同时适用于 AD DS 和 AD LDS 环境。 有关 Active Directory 回收站的详细说明，请参阅 AD DS 中 [What's New：Active Directory 回收站 @ no__t-0。

**新增功能** 在 Windows Server 2012 和更高版本中，使用新的图形用户界面增强了 Active Directory 回收站功能，使用户能够管理和还原已删除的对象。 用户现在可以直观地查找删除的对象列表，并将其还原到原始或期望的位置。

如果计划在 Windows Server 中启用 Active Directory 回收站，请注意以下事项：

- 默认情况下，禁用 Active Directory 回收站。 若要启用此功能，必须首先将 AD DS 或 AD LDS 环境的林功能级别提升到 Windows Server 2008 R2 或更高版本。 这反过来要求林中的所有域控制器或托管 AD LDS 配置集实例的所有服务器都运行 Windows Server 2008 R2 或更高版本。
- 启用 Active Directory 回收站的过程无法撤消。 在环境中启用 Active Directory 回收站之后，就无法再禁用。
- 若要通过用户界面管理回收站功能，必须在 Windows Server 2012 中安装 Active Directory 管理中心版本。

    > [!NOTE]
    > 你可以使用**服务器管理器**安装远程服务器管理工具（RSAT），以便使用正确版本的 Active Directory 管理中心通过用户界面管理回收站。
    >
    > 有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory 回收站的分步设置

在以下步骤中，你将使用 ADAC 来执行 Windows Server 2012 中的以下 Active Directory 回收站任务：

- [步骤 1：提升林功能级别 @ no__t-0
- [步骤 2：启用回收站 @ no__t-0
- [步骤 3：创建测试用户、组和组织单位 @ no__t-0
- [步骤 4：还原已删除对象 @ no__t

> [!NOTE]
> 需要 Enterprise Admins 组的成员身份或同等权限才能执行以下步骤。

### <a name="bkmk_raise_ffl"></a>步骤1：提升林功能级别

在此步骤中，将提升林功能级别。 在启用 Active Directory 回收站之前，必须先将目标林的功能级别至少提升为 Windows Server 2008 R2。

#### <a name="to-raise-the-functional-level-on-the-target-forest"></a>提升目标林的功能级别

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在左导航窗格和 **“任务”** 窗格中单击目标域，再单击 **“提升林功能级别”** 。 选择至少为 Windows Server 2008 R2 或更高版本的林功能级别，然后单击 **"确定"** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

对于 **-Identity**参数，请指定完全限定的 DNS 域名。

### <a name="bkmk_enable_recycle_bin"></a>步骤2：启用回收站

在此步骤中，将启用回收站来还原 AD DS 中删除的对象。

#### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>在目标域的 ADAC 中启用 Active Directory 回收站

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在 **“任务”** 窗格中，单击 **“任务”** 窗格中的 **“启用回收站...”** ，单击警告消息框中的 **“确定”** ，再单击 **“确定”** 以刷新 ADAC 消息。

4. 按 F5 刷新 ADAC。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>步骤3：创建测试用户、组和组织单位

在以下步骤中，将创建两个测试用户。 然后,将创建测试组并将测试用户添加到该组。 此外，还将创建 OU。

#### <a name="to-create-test-users"></a>创建测试用户

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在 **“任务”** 窗格中，单击 **“新建”** ，然后单击 **“用户”** 。

    ![AD 管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4. 在 **“帐户”** 下输入以下信息，再单击“确定”：

   - 全名：test1
   - 用户 SamAccountName 登录：test1
   - 密码： p@ssword1
   - 确认密码： p@ssword1

5. 重复上述步骤以创建第二个用户 test2。

#### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>创建测试组并将用户添加到该组

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。
2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。
3. 在 **“任务”** 窗格中，单击 **“新建”** ，再单击 **“组”** 。
4. 在 **“组”** 下输入以下信息，再单击 **“确定”** ：

    -   **组名称： group1**

5. 单击**group1**，然后在 **“任务”** 窗格下，单击 **“属性”** 。
6. 单击 **“成员”** ，单击 **“添加”** ，键入 **test1;test2**，再单击 **“确定”** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Add-ADGroupMember -Identity group1 -Member test1
```

#### <a name="to-create-an-organizational-unit"></a>创建组织单位

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。
2. 单击 "**管理**"，单击 "**添加导航节点**"，在 "**添加导航节点**" 对话框中选择适当的目标域，然后单击 "确定"。
3. 在 **“任务”** 窗格中，单击 **“新建”** ，再单击 **“组织单位”** 。
4. 在 **“组织单位”** 下输入以下信息，再单击 **“确定”** ：

   - **NameOU1**

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"
```

### <a name="bkmk_restore_del_obj"></a>步骤4：还原删除的对象

在以下过程中，将删除的对象从 **“Deleted Objects”** 容器还原到原始位置和其他位置。

#### <a name="to-restore-deleted-objects-to-their-original-location"></a>将删除的对象还原到原始位置

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 选择用户 **test1** 和 **test2**，单击 **“任务”** 窗格中的 **“删除”** ，再单击 **“是”** 确认删除。

    ![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

    下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

    ```powershell
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4. 导航到 **“Deleted Objects”** 容器，选择 **test2** 和 **test1** ，再单击 **“任务”** 窗格中的 **“还原”** 。

5. 若要确认对象已还原到原始位置，请导航到目标域并确认用户帐户已列出。

    > [!NOTE]
    > 如果导航到用户帐户 **test1** 和 **test2** 的 **“属性”** ，再单击 **“成员”** ，将看到该组成员身份也已还原。

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

#### <a name="to-restore-deleted-objects-to-a-different-location"></a>将删除的对象还原到其他位置

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 选择用户 **test1** 和 **test2**，单击 **“任务”** 窗格中的 **“删除”** ，再单击 **“是”** 确认删除。

4. 导航到 **“Deleted Objects”** 容器，选择 **test2** 和 **test1**，再单击 **“任务”** 窗格中的 **“还原为”** 。

5. 选择 **OU1**，再单击 **“确定”** 。

6. 若要确认对象已还原到 **OU1**，请导航到目标域，双击 **OU1**，再确认用户帐户已列出。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>细化密码策略

Windows Server 2008 操作系统将向组织提供一种为域中不同用户集定义不同密码和帐户锁定策略的方法。 在 Windows Server 2008 之前的 Active Directory 域中，只能对域中的所有用户应用一种密码策略和帐户锁定策略。 这些策略已在域的“Default Domain Policy”中指定。 因此，如果组织希望为不同用户集使用不同密码和帐户锁定设置，就必须创建密码筛选器或部署多个域。 选择这两种方法的代价都很高。

你可以使用细化密码策略，在单个域中指定多个密码策略，并对域中不同的用户集应用不同的密码和帐户锁定策略限制。 例如，可以将较严格的设置应用于有权限的帐户，而将较不严格的设置应用其他用户的帐户。 在其他情况下，可能需要针对密码与其他数据源同步的帐户应用特殊密码策略。 有关细化密码策略的详细说明，请参阅 [AD DS：细化密码策略 @ no__t-0

**新增功能**

在 Windows Server 2012 和更高版本中，通过提供用户界面，使 AD DS 管理员可以在 ADAC 中对其进行管理，从而简化了细化密码策略管理。 管理员现在可以查看给定用户的结果策略，查看和排序指定域内的所有密码策略，以及直观地管理单个密码策略。

如果你计划使用 Windows Server 2012 中的细化密码策略，请考虑以下事项：

- 细化密码策略仅适用于全局安全组和用户对象（或 inetOrgPerson 对象，如果它们使用的是而不是用户对象）。 默认情况下，只有 Domain Admins 组的成员可以设置细化密码策略。 然而，也可以将设置这些策略的能力委派给其他用户。 域功能级别必须属于 Windows Server 2008 或更高版本。

- 必须使用 Windows Server 2012 或更高版本的 Active Directory 管理中心通过图形用户界面管理细化密码策略。

    > [!NOTE]
    > 你可以使用**服务器管理器**安装远程服务器管理工具（RSAT），以便使用正确版本的 Active Directory 管理中心通过用户界面管理回收站。
    >
    > 有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="fine-grained-password-policy-step-by-step"></a>细化密码策略的分步设置

在以下步骤中，将使用 ADAC 执行以下细化密码策略任务：

- [步骤 1：提升域功能级别 @ no__t-0
- [步骤 2：创建测试用户、组和组织单位 @ no__t-0
- [步骤 3：创建新的细化密码策略 @ no__t-0
- [步骤 4：查看用户 @ no__t 的策略的结果集
- [步骤 5：编辑细化密码策略 @ no__t-0
- [步骤 6：删除细化密码策略 @ no__t-0

> [!NOTE]
> 需要 Domain Admins 组的成员身份或同等权限才能执行以下步骤。

#### <a name="bkmk_raise_dfl"></a>步骤1：提升域功能级别

在以下过程中，你会将目标域的域功能级别提升到 Windows Server 2008 或更高版本。 启用细化密码策略需要 Windows Server 2008 或更高版本的域功能级别。

##### <a name="to-raise-the-domain-functional-level"></a>提升域功能级别的步骤

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在左导航窗格和 **“任务”** 窗格中单击目标域，再单击 **“提升域功能级别”** 。 选择至少为 Windows Server 2008 或更高版本的林功能级别，然后单击 **"确定"** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>步骤2：创建测试用户、组和组织单位

若要创建此步骤所需的测试用户和组，请按照此处的步骤操作：[步骤 3：创建测试用户、组和组织单位 @ no__t （无需创建 OU 来演示细化密码策略）。

#### <a name="bkmk_create_fgpp"></a>步骤3：创建新细化密码策略

在以下步骤中，将在 ADAC 中使用 UI 创建新细化密码策略。

##### <a name="to-create-a-new-fine-grained-password-policy"></a>创建新细化密码策略

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在 ADAC 导航窗格中，打开**System**条目，再单击**Password Settings Container**。

4. 在 **“任务”** 窗格中，单击 **“新建”** ，再单击 **“密码设置”** 。

    填写或编辑属性页中的字段，以新建 **“密码设置”** 对象。 要求 **“名称”** 和 **“优先”** 字段。

    ![AD 管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5. 在 "**直接应用**于" 下，单击 "**添加**"，键入**Group1**，然后单击 **"确定"** 。

    这样可将“密码策略”对象与为测试环境创建的全局组成员相关联。

6. 单击 **“确定”** 提交创建。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>步骤4：查看生成的用户策略集

在下面的过程中，你将查看作为你向其分配了细化密码策略的组成员的用户的结果密码设置 [Step 3：创建新的细化密码策略 @ no__t。

##### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>查看生成的用户策略集

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 选择属于**组的用户（属于** **组），** 将细化密码策略与 [Step 3 中的关联。创建新的细化密码策略 @ no__t。

4. 单击 **“任务”** 窗格中的 **“查看 Resultant 密码设置”** 。

5. 检查密码设置策略，再单击 **“取消”** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>步骤5：编辑细化密码策略

在以下过程中，你将编辑在 [Step 3 中创建的细化密码策略：创建新的细化密码策略 @ no__t-0

##### <a name="to-edit-a-fine-grained-password-policy"></a>编辑细化密码策略

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在 ADAC“导航窗格”中，展开“System”，再单击“Password Settings Container”。

4. 选择在 [Step 3 中创建的细化密码策略：创建新的细化密码策略 @ no__t，并单击 "**任务**" 窗格中的 "**属性**"。

5. 在 **“强制密码历史”** 下，将 **“记住密码的次数”** 更改为 **30**。

6. 单击 **“确定”** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>步骤6：删除细化密码策略

##### <a name="to-delete-a-fine-grained-password-policy"></a>删除细化密码策略

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 在 ADAC 导航窗格中，展开 **System** ，再单击 **Password Settings Container**。

4. 选择在 [Step 3 中创建的细化密码策略：创建新的细化密码策略 @ no__t，然后在 "**任务**" 窗格中单击 "**属性**"。

5. 清除 **“防止意外删除”** 复选框并单击 **“确认”** 。

6. 选择细化密码策略，并在 **“任务”** 窗格中单击 **“删除”** 。

7. 单击确认对话框中的 **“确定”** 。

![Intro 到 AD 管理中心](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***

下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。

```powershell
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Windows PowerShell 历史记录查看器

ADAC 是构建在 Windows PowerShell 上的用户界面工具。 在 Windows Server 2012 及更高版本中，IT 管理员可以利用 ADAC，通过使用 Windows PowerShell 历史记录查看器了解适用于 Active Directory cmdlet 的 Windows PowerShell。 在用户界面中执行操作时，会在 Windows PowerShell 历史记录查看器中向用户显示相对应的 Windows PowerShell 命令。 这样，管理员可以创建自动脚本并减少重复任务，从而提高 IT 工作效率。 此外，此功能减少了了解适用于 Active Directory 的 Windows PowerShell 的时间，并使用户能够更有把握地确保其自动化脚本的正确性。

在 Windows Server 2012 或更高版本中使用 Windows PowerShell 历史记录查看器时，请考虑以下事项：

- 若要使用 Windows PowerShell 脚本查看器，必须使用 Windows Server 2012 或更高版本的 ADAC

    > [!NOTE]
    > 你可以使用**服务器管理器**安装远程服务器管理工具（RSAT），以便使用正确版本的 Active Directory 管理中心通过用户界面管理回收站。
    >
    > 有关安装 RSAT 的信息，请参阅文章[远程服务器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

- 对 Windows PowerShell 有一个基本的了解。 例如，需要了解 Windows PowerShell 中管道系统的工作原理。 有关 Windows PowerShell 中管道系统的详细信息，请参阅 [Windows PowerShell 中的管道系统和管道](https://technet.microsoft.com/library/ee176927.aspx)。

### <a name="windows-powershell-history-viewer-step-by-step"></a>Windows PowerShell 历史记录查看器的分步设置

在以下步骤中，将使用 ADAC 中的 Windows PowerShell 历史记录查看器构建 Windows PowerShell 脚本。  开始此步骤之前，请先从组 **group1** 中删除用户 **test1**。

#### <a name="to-construct-a-script-using-powershell-history-viewer"></a>使用 PowerShell 历史记录查看器构建脚本

1. 右键单击 Windows PowerShell 图标，单击 "以**管理员身份运行**"，然后键入**DSAC.EXE**以打开 ADAC。

2. 单击 **“管理”** ，单击 **“添加导航节点”** ，在 **“添加导航节点”** 对话框中选择相应目标域，再单击 **“确定”** 。

3. 展开 ADAC 屏幕底部的 **“Windows PowerShell 历史记录”** 窗格。

4. 选择用户 **test1**。

5. 单击 "**任务**" 窗格中的 "**添加到组 ...** "。

6. 导航到 **group1**，再单击对话框中的 **“确定”** 。

7. 导航到 **“Windows PowerShell 历史记录”** 窗格并找到刚生成的命令。

8. 复制命令并将其粘贴到所需编辑器构造脚本。

    例如，可以修改命令，将其他用户添加到 **group1**，或将 **test1** 添加到其他组。

## <a name="see-also"></a>请参阅

[使用 Active Directory 管理中心&#40;级别200的高级 AD DS 管理&#41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)
