---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: "简介 Active Directory 管理中心增强 (级别 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>简介 Active Directory 管理中心增强 (级别 100)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012 ADAC 包括管理以下功能：

-   [Active Directory 回收站](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [精细的密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Windows PowerShell 查看历史记录](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Active Directory 回收站
意外删除 Active Directory 对象是很常见的用户的 Active Directory 域服务 (广告 DS) 和 Active Directory 轻型目录服务 (广告 LDS)。 在以前版本的 Windows Server、Windows Server 2008 R2、前一个无法恢复意外已删除的 Active Directory 中的对象，但解决方案其缺陷。

在 Windows Server 2008，用户也可以使用 Windows Server 备份功能和**ntdsutil**授权还原命令标记对象作为授权，以确保整个域复制还原的数据。 缺点授权还原解决方案时，它不得不执行中目录服务还原模式 (DSRM)。 在 DSRM，域控制器还原不得不离线状态下，将保留。 因此，它无法为客户端请求提供服务。

在 Windows Server 2003 Active Directory 和 Windows Server 2008 广告 DS，你可以恢复通过逻辑删除恢复已删除 Active Directory 对象。 但是，恢复的对象的链接值属性（例如，用户帐户的组成员），在物理删除和没有恢复已清除的链接值属性。 因此，管理员可能不依赖于逻辑删除恢复为到的对象意外删除旗舰版的解决方案。 有关逻辑删除恢复的详细信息，请参阅[Reanimating Active Directory Tombstone 对象](https://go.microsoft.com/fwlink/?LinkID=125452)。

Active Directory 回收站，开始在 Windows Server 2008 R2、现有的 tombstone 恢复基础结构以此为基础，并增强了你保留和恢复意外已删除 Active Directory 对象的能力。

Active Directory 回收站启用时，将保留所有链接值和链接值的已删除 Active Directory 对象的特性和对象还原到其删除之前，立即所处的相同一致逻辑状态他们整个。 例如，用户已还原的帐户自动重新获取所有组会员身份和相应删除、中和跨域之前，立即的访问权限。 Active Directory 回收站 Bin 适用于广告 DS 和广告 LDS 环境。 Active Directory 回收站的详细说明，请参阅[什么是广告 DS 中的新增功能：Active Directory 回收站](https://technet.microsoft.com/library/dd391916(WS.10).aspx)。

**是什么？** 在 Windows Server 2012、Active Directory 回收站功能已增强的用户，若要管理和还原已删除的对象新图形用户界面。 现在，用户可以直观地找到已删除对象的列表并将其还原到其原始或所需的位置。

如果你打算启用 Active Directory 回收站显示在 Windows Server 2012，请考虑以下各项：

-   默认情况下，Active Directory 回收站处于禁用状态。 若要启用它，必须首先引发广告 DS 或广告 LDS 环境对 Windows Server 2008 R2 或更高版本的森林功能级别。 此反过来要求森林中的所有域控制器或托管的广告 LDS 配置集实例的所有服务器是运行 Windows Server 2008 R2 或更高版本。

-   是不可逆的启用 Active Directory 回收站的过程。 您的环境中启用了 Active Directory 回收站之后，无法将其禁用。

-   若要管理通过用户界面回收站功能，必须安装在 Windows Server 2012 的 Active Directory 管理中心版本。

    > [!NOTE]
    > 你可以使用**服务器管理器**在使用 Active Directory 管理中心正确版本管理通过用户界面回收站中的 Windows Server 2012 计算机上安装远程服务器管理工具 (RSAT)。
    > 
    > 你可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) Windows 上&reg;8 计算机使用 Active Directory 管理中心正确版本管理通过用户界面回收站。

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory 回收站分步
在以下步骤中，您将使用 ADAC 在 Windows Server 2012 执行下列 Active Directory 回收站任务：

-   [第 1 步：提升森林功能级别](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [第 2 步：启用回收站](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [第 3 步：创建测试用户和组单位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [第 4 步：还原已删除的对象](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> 等效权限的企业管理员组中的成员才能执行以下步骤。

### <a name="bkmk_raise_ffl"></a>第 1 步：提升森林功能级别
在此步骤中，您将提升森林功能级别。 你首先必须提升上启用 Active Directory 回收站之前是 Windows Server 2008 R2 至少目标森林功能级别。

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>若要提升功能的级别上目标森林

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  单击左侧的导航窗格中，然后在目标域**任务**窗格中，单击**提升森林功能级别**。 选择至少森林功能级别 Windows Server 2008 R2 或更高版本，然后单击**确定**。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

对于**-身份**参数，指定完整的 DNS 名称。

### <a name="bkmk_enable_recycle_bin"></a>第 2 步：启用回收站
在此步骤，会启用到还原已删除对象广告 DS 中的回收站。

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>若要启用 Active Directory 回收站 ADAC 上目标域中

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在**任务**窗格中，单击**启用回收站…**中**任务**窗格中，单击**确定**警告消息框中，，然后单击**确定**到刷新 ADAC 消息。

4.  若要恢复 ADAC 按 f5 键。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>第 3 步：创建测试用户和组单位
在下面的过程中，您将创建两个测试的用户。 然后，您将创建测试组并添加到组测试用户。 此外，您将创建一个 OU。

##### <a name="to-create-test-users"></a>若要创建测试用户

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在**任务**窗格中，单击**新建**，然后单击**用户**。

    ![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  输入以下信息下的**帐户**，然后单击确定:

    -   全名：test1

    -   用户 SamAccountName 登录：test1

    -   密码：p@ssword1

    -   确认密码：p@ssword1

5.  重复创建第二个的用户，test2 之前的步骤。

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>添加到组的用户，创建一个测试组

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在**任务**窗格中，单击**新建**，然后单击**组**。

4.  输入以下信息下的**组**，然后单击**确定**:

    -   **分组 1 组名称：**

5.  单击**分组 1**，然后在**任务**窗格中，单击**属性**。

6.  单击**成员**，单击**添加**，类型**test1; test2**，然后单击**确定**。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>若要创建单位

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在**任务**窗格中，单击**新建**，然后单击**单位**。

4.  输入以下信息下的**单位**，然后单击**确定**:

    -   **NameOU1**

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>第 4 步：还原已删除的对象
在下面的过程中，您将还原已删除的对象从**删除对象**容器到其原始位置和其他位置。

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>若要还原已删除的到其原始位置的对象

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  选择用户**test1**和**test2**，单击**删除**中**任务**窗格，然后单击**是**确认删除。

    ![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

    以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  导航到**删除对象**容器，选择**test2**和**test1**，然后单击**还原**中**任务**窗格。

5.  若要确认对象已还原到其原始位置，导航到的目标域，然后确认列出的用户帐户。

    > [!NOTE]
    > 如果你导航到**属性**的用户帐户**test1**和**test2**，然后单击**隶属于**，你将看到他们的组成员已还还原。

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>若要还原已删除的不同位置的对象

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  选择用户**test1**和**test2**，单击**删除**中**任务**窗格，然后单击**是**确认删除。

4.  导航到**删除对象**容器，选择**test2**和**test1**，然后单击**还原到**中**任务**窗格。

5.  选择**OU1**，然后单击**确定**。

6.  若要确认对象被还原为**OU1**，导航到的目标域，双击**OU1**，然后确认列出了用户帐户。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>精细的密码策略
Windows Server 2008 操作系统提供组织了一种方法在某个域中定义不同的密码和为不同的用户帐户锁定策略。 在 Windows Server 2008 前 Active Directory 域中，只有一个密码策略和帐户锁定策略可能应用于域中的所有用户。 这些策略默认域策略域中指定。 因此，希望不同的用户的不同密码和帐户锁定设置的组织了以创建一个密码筛选器或部署多个域。 两者都很高的选项。

精细的密码策略用于指定多个密码策略，在一个域中的并应用到不同的用户在某个域中的密码和帐户锁定策略的其他限制。 例如，你可以对其他用户的帐户应用授权帐户更严格的设置和较少严格的设置。 在其他情况下，你可能想要将帐户的密码同步与其他数据来源，用于特殊密码策略应用。 有关 Fine-Grained 密码策略的详细说明，请参阅[广告 DS: Fine-Grained 密码策略](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**是什么？** Windows Server 2012，在精准密码策略管理更简单且更多可视化来进行提供用于广告 DS 管理员可以在 ADAC 管理它们的用户界面。 管理员现在可以查看给定的用户结果策略、查看和排序给定的域中的所有密码策略和视力管理各个密码策略。

如果您计划使用精准密码策略，在 Windows Server 2012，请考虑以下各项：

-   精准密码策略应用仅全球安全组和用户对象（或如果使用它们来代替用户对象 inetOrgPerson 对象）。 默认情况下，只有管理员域组成员可以设置精准密码的策略。 但是，你还可以委派设置这些策略与其他用户的功能。 域功能级别必须是 Windows Server 2008 或更高版本。

-   你必须使用 Active Directory 管理中心的 Windows Server 2012 版本管理通过图形用户界面精准密码策略。

    > [!NOTE]
    > 你可以使用**服务器管理器**在使用 Active Directory 管理中心正确版本管理通过用户界面回收站中的 Windows Server 2012 计算机上安装远程服务器管理工具 (RSAT)。
    > 
    > 你可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) Windows 上&reg;8 计算机使用 Active Directory 管理中心正确版本管理通过用户界面回收站。

### <a name="fine-grained-password-policy-step-by-step"></a>精细的密码策略分步
在以下步骤中，您将使用 ADAC 执行下列精准密码策略任务：

-   [第 1 步：提升域功能级别](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [第 2 步：创建用户测试组中，，单位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [第 3 步：创建新精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [第 4 步：查看结果的一组策略用户](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [第 5 步：编辑精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [第 6 步：删除精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> 等效权限的域管理员组中的成员才能执行以下步骤。

#### <a name="bkmk_raise_dfl"></a>第 1 步：提升域功能级别
在下面的过程中，您将提升域功能级别的目标域，Windows Server 2008 或更高版本。 Windows Server 2008 或更高版本域功能级别需要启用精准密码的策略。

###### <a name="to-raise-the-domain-functional-level"></a>若要提升域功能级别

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  单击左侧的导航窗格中，然后在目标域**任务**窗格中，单击**提升域功能级别**。 选择至少森林功能级别 Windows Server 2008 或更高版本，然后单击**确定**。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>第 2 步：创建用户测试组中，，单位
若要创建测试用户和组需此步骤，请按照位于此处的过程：[步骤 3：创建测试用户和组部门](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)（你不需要创建用于展示精准密码策略 OU）。

#### <a name="bkmk_create_fgpp"></a>第 3 步：创建新精准密码策略
在下面的过程中，您将创建新精准密码策略 ADAC 中使用的 UI。

###### <a name="to-create-a-new-fine-grained-password-policy"></a>若要创建新的精细的密码策略

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在 ADAC 导航窗格中，打开**系统**容器，再单击**密码设置容器**。

4.  在**任务**窗格中，单击**新建**，然后单击**密码设置**。

    填写或编辑内的属性页以创建一个新的字段**密码设置**对象。 **名称**和**优先级**为必填字段。

    ![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  下**直接适用于**，单击**添加**，类型**分组 1**，然后单击**确定**。

    这将密码策略对象关联与全球组测试环境为你创建的成员。

6.  单击**确定**作品的提交。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>第 4 步：查看结果的一组策略用户
在下面的过程中，您将查看赋予精细的密码策略中的组成员的用户的结果密码设置[步骤 3：创建新的精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>若要查看结果的一组策略用户

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  选择用户，**test1**属于组中，**分组 1**你关联精细密码策略内的[步骤 3：创建新的精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

4.  单击**查看结果密码设置**中**任务**窗格。

5.  检查设置密码策略，然后单击**取消**。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>第 5 步：编辑精准密码策略
在下面的过程中，您将编辑中创建的精细的密码策略[步骤 3：创建新的精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>若要编辑精准密码策略

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在 ADAC**导航窗格**，展开**系统**，然后单击**密码设置容器**。

4.  选择在创建精细的密码策略[步骤 3：创建新的精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)单击**属性**中**任务**窗格。

5.  下**强制密码历史**，更改的值**记住的密码的次数**到**30**。

6.  单击**确定**。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>第 6 步：删除精准密码策略

###### <a name="to-delete-a-fine-grained-password-policy"></a>若要删除精准密码策略

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  在 ADAC 导航窗格中，展开**系统**，然后单击**密码设置容器**。

4.  选择在创建精细的密码策略[步骤 3：创建新的精准密码策略](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)在**任务**窗格中单击**属性**。

5.  清除**防止意外删除**复选框，然后单击**确定**。

6.  选择精细的密码策略，在**任务**窗格中单击**删除**。

7.  单击**确定**在确认对话框。

![广告管理中心简介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***

以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Windows PowerShell 查看历史记录
ADAC 是用户界面工具内置在 Windows PowerShell 顶部。  Windows Server 2012，在 IT 管理员可以利用 ADAC 以了解使用 Windows PowerShell 历史记录查看器 Windows PowerShell cmdlet Active Directory。 在用户界面中执行的操作，如 Windows PowerShell 历史记录查看器在向用户显示等效的 Windows PowerShell 命令。 这使管理员创建自动的脚本，减少重复的任务，从而提高 IT 工作效率。  此外，此功能将了解 Active Directory 的 Windows PowerShell 的时间减少，并使其自动化脚本正确中的用户的进一步。

在 Windows Server 2012 使用 Windows PowerShell 历史记录查看器时，请考虑以下：

-   若要使用的 Windows PowerShell 脚本查看器，你必须使用 ADAC Windows Server 2012 版本

    > [!NOTE]
    > 你可以使用**服务器管理器**在使用 Active Directory 管理中心正确版本管理通过用户界面回收站中的 Windows Server 2012 计算机上安装远程服务器管理工具 (RSAT)。
    > 
    > 你可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) Windows 上&reg;8 计算机使用 Active Directory 管理中心正确版本管理通过用户界面回收站。

-   有一些基本的 Windows PowerShell 知识。 例如，你需要了解在 Windows PowerShell 管道的工作方式。 有关在 Windows PowerShell 管道的详细信息，请参阅[管道，并且在 Windows PowerShell 管道](https://technet.microsoft.com/library/ee176927.aspx)。

### <a name="windows-powershell-history-viewer-step-by-step"></a>Windows PowerShell 分步历史记录查看器
在下面的过程中，您将使用 Windows PowerShell 历史记录查看器在 ADAC 构建 Windows PowerShell 脚本。  在开始该过程之前，请移除用户，**test1**组中，从**分组 1**。

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>构建脚本使用 PowerShell 历史记录查看器

1.  右键单击 Windows PowerShell 图标，再单击**以管理员身份运行**，然后键入**dsac.exe**打开 ADAC。

2.  单击**管理**，单击**添加导航节点**选择相应的目标域中**添加导航节点**对话框中，然后单击**确定**。

3.  展开**Windows PowerShell 历史记录**窗格 ADAC 屏幕底部。

4.  选择用户，**test1**。

5.  单击**添加到组…**中**任务**窗格。

6.  导航到**分组 1**单击**确定**对话框中。

7.  导航到**Windows PowerShell 历史记录**窗格中，然后找到刚生成的命令。

8.  复制该命令，并将其粘贴到你所需的编辑器构建你脚本。

    例如，你可以修改命令添加到不同的用户**分组 1**，或添加**test1**到不同的组。

## <a name="see-also"></a>请参阅
[高级的广告 DS 管理使用 Active Directory 管理中心 & #40;级别 200 & #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


