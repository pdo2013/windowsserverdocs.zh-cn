---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 附录 I-为 Active Directory 中的受保护帐户和组创建管理帐户
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8880f26acd8b32a4ab8a32ede067d158f2d6aed1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369215"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附录 I：为 Active Directory 中的受保护帐户和组创建管理帐户

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在实现不依赖于高特权组中的永久成员身份的 Active Directory 模型时，其中一项难题是，当需要组中的临时成员身份时，必须有一种机制来填充这些组。 某些特权标识管理解决方案要求在林中的每个域的组（如 DA 或管理员）中授予软件的服务帐户永久成员身份。 不过，在技术上，Privileged Identity Management （PIM）解决方案在这种高度特权的上下文中运行其服务并不是必需的。  
  
本附录提供了可用于本机实现的或第三方 PIM 解决方案的信息，用于创建具有有限权限且可得到控制的帐户，但 Active Directory 可以在需要临时提升。 如果要将 PIM 作为本机解决方案来实现，则管理人员可能会使用这些帐户来执行临时组填充，如果你要通过第三方软件实现 PIM，则可以将这些帐户改编为充当服务帐户.  
  
> [!NOTE]  
> 本附录中描述的过程提供了一种方法来管理 Active Directory 中的高特权组。 您可以根据需要修改这些过程，添加其他限制，或者省略此处所述的某些限制。  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>为 Active Directory 中的受保护帐户和组创建管理帐户

创建可用于管理特权组成员身份的帐户，而无需授予管理帐户过多的权限和权限，这四个常规活动在分步说明中进行了介绍。参照  
  
1.  首先，您应创建一个将管理帐户的组，因为这些帐户应由一组有限的受信任的用户进行管理。 如果你还没有 OU 结构可用于从域中的常规填充中分离特权和受保护的帐户和系统，则应该创建一个。 尽管此附录中未提供特定说明，但屏幕截图显示了此类 OU 层次结构的示例。  
  
2.  创建管理帐户。 应将这些帐户创建为 "常规" 用户帐户，并授予除默认情况下已授予给用户的权限以外的任何用户权限。  
  
3.  对管理帐户实施限制，使其仅用于创建它们的专用目的，以及控制谁可以启用和使用帐户（在第一步中创建的组）。  
  
4.  在每个域中的 AdminSDHolder 对象上配置权限，以允许管理帐户更改域中特权组的成员身份。  
  
在生产环境中实施这些过程之前，应全面测试所有这些过程并根据环境需要对其进行修改。 还应验证所有设置是否按预期方式工作（本附录中提供了一些测试过程），并且你应该测试灾难恢复方案，在此方案中，管理帐户不能用于填充受保护的组以进行恢复之. 有关备份和还原 Active Directory 的详细信息，请参阅[AD DS 备份和恢复循序渐进指南](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 通过实施此附录中所述的步骤，你将创建可管理每个域中所有受保护组的成员身份的帐户，而不仅是最高特权 Active Directory 组（如 EAs、DAs 和 BAs）。 有关 Active Directory 中的受保护组的详细信息，请参阅 @no__t 0Appendix C：Active Directory @ no__t 中的受保护帐户和组。  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>为受保护组创建管理帐户的分步说明  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>创建组以启用和禁用管理帐户

管理帐户应在每次使用时重置密码，并应在需要这些帐户的活动完成后将其禁用。 尽管你可能还会考虑为这些帐户实现智能卡登录要求，但它是一个可选配置，并且这些说明假定管理帐户将使用用户名和长的复杂密码（最小密码）进行配置控件. 在此步骤中，你将创建一个有权在管理帐户上重置密码以及启用和禁用帐户的组。  
  
若要创建一个组以启用和禁用管理帐户，请执行以下步骤：  
  
1.  在将存放管理帐户的 OU 结构中，右键单击要在其中创建组的 OU，单击 "**新建**"，然后单击 "**组**"。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在 "**新建对象-组**" 对话框中，输入组的名称。 如果计划使用此组来 "激活" 林中的所有管理帐户，请将其设为通用安全组。 如果你有单域林，或者计划在每个域中创建组，则可以创建一个全局安全组。 单击 “确定”以创建组。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  右键单击刚创建的组，单击“属性”，然后单击“对象”选项卡。在组的 "**对象属性**" 对话框中，选择 "**防止对象被意外删除**"，这不仅会阻止以其他方式授权的用户删除组，而且还会将其移动到另一个 OU （除非首先返回该属性）状态.  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > 如果已在组的父 Ou 上配置了权限，以限制对一组有限的用户进行管理，则可能无需执行以下步骤。 此处提供了这些内容，即使你尚未对创建此组的 OU 结构实现有限的管理控制，你也可以保护组不受未经授权的用户的修改。  
  
4.  单击 "**成员**" 选项卡，并添加团队成员的帐户，这些帐户将负责启用管理帐户或在必要时填充受保护的组。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果尚未执行此操作，请在 " **Active Directory 用户和计算机**" 控制台中，单击 "**查看**"，然后选择 "**高级功能**"。 右键单击刚创建的组，单击 "**属性**"，然后单击 "**安全**" 选项卡。在“安全” 选项卡中，单击“高级”。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在 " **[组] 的高级安全设置**" 对话框中，单击 "**禁用继承**"。 出现提示时，单击 "**将继承权限转换为对此对象的显式权限**"，然后单击 **"确定"** 以返回到组的 "**安全性**" 对话框。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  在 "**安全**" 选项卡上，删除不允许访问此组的组。 例如，如果不想让经过身份验证的用户能够读取组的 "名称" 和 "常规" 属性，则可以删除该 ACE。 你还可以删除 Ace，如用于帐户操作员和 Windows 2000 以前的 Windows Server 兼容访问的 Ace。 但是，您应该保留一组最小的对象权限。 保留以下 Ace 不变：  
  
    -   解压  
  
    -   主板  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Windows 授权访问组（如果适用）  
  
    -   企业域控制器  
  
    尽管在 Active Directory 中允许最高特权组来管理此组，但实现这些设置的目标并不是阻止这些组的成员进行授权更改。 相反，其目的是确保在需要非常高的权限级别时，授权的更改将会成功。 出于此原因，不建议更改默认的特权组嵌套、权限和权限。 通过保持默认结构不变并在目录中清空最高权限组的成员身份，你可以创建仍按预期方式工作的更安全的环境。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果尚未为创建此组的 OU 结构中的对象配置审核策略，则应将审核配置为记录更改此组。  
  
8.  你已完成了组的配置，该配置将用于在需要时 "查看" 管理帐户，并在其活动完成时 "签入" 帐户。  
  
#### <a name="creating-the-management-accounts"></a>创建管理帐户

你应该至少创建一个用于管理 Active Directory 安装中特权组的成员身份的帐户，并最好创建一个用于作为备份的第二个帐户。 无论你选择在林中的单个域中创建管理帐户并为所有域的受保护组授予管理功能，还是选择在林中的每个域中实施管理帐户，这些过程都是实际上是相同的。  
  
> [!NOTE]  
> 本文档中的步骤假设您尚未实现 Active Directory 的基于角色的访问控制和特权标识管理。 因此，某些过程必须由其帐户是所述域的 Domain Admins 组成员的用户执行。  
>   
> 使用具有 DA 权限的帐户时，可以登录到域控制器以执行配置活动。 不需要 DA 权限的步骤可以通过登录到管理工作站的不太特权帐户来执行。 显示以较浅蓝色显示的对话框的屏幕截图表示可在域控制器上执行的活动。 以较深蓝色显示对话框的屏幕截图表示可以在具有有限特权的帐户的管理工作站上执行的活动。  
  
若要创建管理帐户，请执行以下步骤：  
  
1. 使用作为域的 DA 组成员的帐户登录到域控制器。  

2. 启动**Active Directory 的用户和计算机**，然后导航到要在其中创建管理帐户的 OU。  

3. 右键单击 OU，然后单击 "**新建**"，然后单击 "**用户**"。  

4. 在 "**新建对象-用户**" 对话框中，输入所需的帐户命名信息，然后单击 "**下一步**"。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. 为用户帐户提供初始密码，清除 "**用户在下次登录时必须更改密码**"，选择 "**用户不能更改密码**和**帐户已禁用**"，然后单击 "**下一步**"。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. 验证帐户详细信息是否正确，然后单击 "**完成**"。  

7. 右键单击刚创建的用户对象，然后单击 "**属性**"。  

8. 单击 "**帐户**" 选项卡。  

9. 在 "**帐户选项**" 字段中，选择 "**敏感帐户，不能被委派**" 标志，选择 "**此帐户支持 kerberos aes 128 位加密**" 和/或 "**此帐户支持 kerberos aes 256 加密**标志"。然后单击 **"确定"** 。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > 由于此帐户与其他帐户类似，因此，帐户只应在安全的管理主机上使用。 对于环境中的所有安全管理主机，应考虑实现组策略设置 @no__t 0Network 安全性：将 Kerberos @ no__t 允许的加密类型配置为允许安全主机仅允许最安全的加密类型。  
   >
   > 尽管为主机实现更安全的加密类型不能减少凭据被盗攻击，但安全主机的适当使用和配置也是如此。 为仅由特权帐户使用的主机设置更强的加密类型只是减少了计算机的整体攻击面。  
   >
   > 有关在系统和帐户上配置加密类型的详细信息，请参阅 " [Kerberos 支持的加密类型的 Windows 配置](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)"。  
   >
   > 只有运行 Windows Server 2012、Windows Server 2008 R2、Windows 8 或 Windows 7 的计算机才支持这些设置。  
  
10. 在 "**对象**" 选项卡上，选择 "**防止对象被意外删除**"。 这不仅会阻止对象被删除（甚至由授权用户删除），而且还会阻止将其移到 AD DS 层次结构中的不同 OU，除非用户第一次清除复选框，用户有权更改该属性。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. 单击 "**远程控制**" 选项卡。  

12. 清除 "**启用远程控制**" 标志。 如果支持人员连接到此帐户的会话以实现修补程序，则决不需要此方法。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Active Directory 中的每个对象都应具有指定的 IT 所有者和指定的业务所有者，如[规划折衷](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)中所述。 如果正在跟踪 Active Directory 中 AD DS 对象的所有权（而不是外部数据库），则应在此对象的属性中输入适当的所有权信息。  
    >
    > 在这种情况下，业务所有者很可能是 IT 部门，andthere 对企业所有者也不是 IT 所有者。 建立对象所有权的点是允许您在需要对对象进行更改时确定联系人，这可能是由于最初创建的年份。  

13. 单击 "**组织**" 选项卡。  

14. 输入 AD DS 对象标准中所需的任何信息。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. 单击 "**拨入**" 选项卡。  

16. 在 "**网络访问权限**" 字段中，选择 "**拒绝访问**"。此帐户永远不需要通过远程连接进行连接。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > 此帐户不太可能用于登录到您的环境中的只读域控制器（Rodc）。 但是，如果需要使用该帐户登录到 RODC，则应将此帐户添加到 "拒绝的 RODC 密码复制组"，以使其密码不会缓存在 RODC 上。  
    >
    > 尽管应在每次使用后重置帐户的密码，并且应禁用该帐户，但实现此设置并不会对帐户产生产生负面影响，并且可能会在管理员忘记重置帐户的密码并禁用密码。  

17. 单击 **“隶属于”** 选项卡。  

18. 单击**添加**。  

19. 在 "**选择用户、联系人、计算机**" 对话框中，键入 "**拒绝的 RODC 密码复制组**"，然后单击 "**检查名称**"。 在对象选取器中为组的名称加下划线后，单击 **"确定"** ，然后验证该帐户是否为以下屏幕截图中显示的两个组的成员。 不要将该帐户添加到任何受保护的组。  

20. 单击 **“确定”** 。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. 单击 "**安全**" 选项卡，然后单击 "**高级**"。  

22. 在 "**高级安全设置**" 对话框中，单击 "**禁用继承**" 并将继承的权限复制为显式权限，然后单击 "**添加**"。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. 在 " **[帐户] 的权限条目**" 对话框中，单击 "**选择主体**"，并添加在前面的过程中创建的组。 滚动到对话框的底部，然后单击 "**全部清除**" 以删除所有默认权限。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. 滚动到 "**权限条目**" 对话框的顶部。 确保 "**类型**" 下拉列表设置为 "**允许**"，然后在 "**应用于**" 下拉列表中，选择 "**仅此对象**"。  

25. 在 "**权限**" 字段中，选择 "**读取所有属性**"、"**读取权限**" 和 "**重置密码**"。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. 在 "**属性**" 字段中，选择 "**读取 userAccountControl**并**写入 useraccountcontrol**"。  

27. 单击 "**高级安全设置**" 对话框中的 **"确定**"，再次单击 "**确定**"。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > **UserAccountControl**属性控制多个帐户配置选项。 向属性授予写入权限时，不能授予仅更改某些配置选项的权限。  

28. 在 "**安全**" 选项卡的 "**组或用户名**" 字段中，删除不允许访问或管理帐户的所有组。 请勿删除任何已配置 "拒绝" Ace 的组，如 "Everyone" 组和 "自行计算的帐户" （在创建帐户期间启用了 "**用户无法更改密码**" 标志时设置了 ACE。 同时，不要删除刚添加的组、系统帐户或组（如 EA、DA、BA 或 Windows 授权访问组）。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. 单击 "**高级**"，并验证 "高级安全设置" 对话框是否与以下屏幕截图类似。  

30. 单击 **"确定**"，然后再次单击 **"确定"** 以关闭帐户的属性对话框。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. 第一个管理帐户的设置现已完成。 你将在后面的过程中测试该帐户。  

##### <a name="creating-additional-management-accounts"></a>创建其他管理帐户

可以通过重复上述步骤来创建其他管理帐户，方法是复制刚创建的帐户，或者创建一个脚本来创建具有所需配置设置的帐户。 但请注意，如果复制刚刚创建的帐户，则很多自定义设置和 Acl 将不会复制到新帐户，你将需要重复大多数配置步骤。  
  
您可以改为创建一个组，以向其委派填充和 unpopulate 受保护组的权限，但需要保护该组和您在其中放置的帐户。 由于目录中应该有很少的帐户被授予管理受保护组成员身份的能力，因此创建单个帐户可能是最简单的方法。  
  
不管你如何选择创建一个将管理帐户放置到其中的组，都应确保按前面所述保护每个帐户。 还应考虑实现 GPO 限制 @no__t，类似于 0Appendix D：Active Directory @ no__t 中保护内置管理员帐户。  
  
##### <a name="auditing-management-accounts"></a>审核管理帐户

应将审核配置为至少记录所有写入帐户的帐户。 这样，你不仅可以识别成功启用帐户并在授权使用期间重置其密码，还可以确定未经授权的用户操纵帐户的尝试。 应在安全信息和事件监视（SIEM）系统（如果适用）中捕获帐户上的失败写入，并触发警报，向负责调查潜在损害的人员提供通知。  
  
SIEM 解决方案从涉及的安全源（例如，事件日志、应用程序数据、网络流、反恶意软件产品和入侵检测源）获取事件信息，对数据进行整理，并尝试建立智能视图和主动操作. 有很多商业 SIEM 解决方案，许多企业创建专用实现。 设计良好且适当实现的 SIEM 可以显著增强安全监视和事件响应功能。 但解决方案之间的功能和准确性会有所不同。 Siem 超出了本文的范围，但任何 SIEM 实施者都应该考虑到包含的特定事件建议。  
  
有关域控制器的建议审核配置设置的详细信息，请参阅[监视 Active Directory 是否有泄露迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 监视 Active Directory 中提供了特定于域控制器的配置设置[，以应对泄露迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>启用管理帐户以修改受保护组的成员身份

在此过程中，你将配置对域的 AdminSDHolder 对象的权限，以允许新创建的管理帐户修改域中受保护组的成员身份。 不能通过图形用户界面（GUI）执行此过程。  
  
如 0Appendix C @no__t 中所述：Active Directory @ no__t 中受保护的帐户和组-在 SDProp 任务运行时，将在域的 AdminSDHolder 对象上将 ACL "复制" 到受保护的对象。 受保护的组和帐户不会从 AdminSDHolder 对象继承其权限;它们的权限显式设置为匹配 AdminSDHolder 对象上的权限。 因此，修改 AdminSDHolder 对象上的权限时，必须对其进行修改，使其适用于目标为受保护对象的类型。  
  
在这种情况下，你将授予新创建的管理帐户，使其能够读取和写入组对象上的成员属性。 但是，AdminSDHolder 对象不是组对象，并且不会在图形 ACL 编辑器中公开组属性。 出于此原因，你将通过 Dsacls 命令行实用工具实现权限更改。 若要授予（禁用）管理帐户修改受保护组成员身份的权限，请执行以下步骤：  
  
1. 登录到域控制器，最好是持有 PDC 模拟器（PDCE）角色的域控制器，其中包含已成为域中的 "DA" 组成员的用户帐户的凭据。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. 右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**"，以打开提升的命令提示符。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. 当提示批准提升时，单击 **"是"** 。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > 有关 Windows 中的提升和用户帐户控制（UAC）的详细信息，请参阅 TechNet 网站上的[UAC 进程和交互](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)。  
  
4. 在命令提示符下，键入（替换域特定信息） **Dsacls [域中 AdminSDHolder 对象的可分辨名称]/g [管理帐户 UPN]： RPWP; member**。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   前面的命令（不区分大小写）的工作方式如下：  
  
   - Dsacls 设置或显示目录对象上的 Ace  
  
   - CN = AdminSDHolder，CN = System，DC = Azure-tailspintoys，DC = msft 标识要修改的对象  
  
   - /G 指示正在配置 grant ACE  
  
   - PIM001@tailspintoys.msft 是将向其授予 Ace 的安全主体的用户主体名称（UPN）  
  
   - RPWP 授予 "读取属性" 和 "写入属性" 权限  
  
   - Member 是将对其设置权限的属性（属性）的名称  
  
   有关使用**Dsacls**的详细信息，请在命令提示符处键入 Dsacls，不使用任何参数。  
  
   如果已为域创建了多个管理帐户，则应为每个帐户运行 Dsacls 命令。 在 AdminSDHolder 对象上完成 ACL 配置后，应强制 SDProp 运行，或等待其计划的运行完成。 有关强制 SDProp 运行的信息，请参阅 [Appendix C：Active Directory @ no__t 中的受保护帐户和组。  
  
   运行 SDProp 时，你可以验证你对 AdminSDHolder 对象所做的更改是否已应用于域中的受保护组。 由于前面所述的原因，你无法通过查看 AdminSDHolder 对象上的 ACL 来验证这一点，但你可以通过查看受保护组的 Acl 来验证是否已应用这些权限。  
  
5. 在**Active Directory 用户和计算机**"中，验证是否已启用**高级功能**。 为此，请单击 "**查看**"，找到 "**域管理员**" 组，右键单击该组，然后单击 "**属性**"。  
  
6. 单击 "**安全**" 选项卡，然后单击 "**高级**" 以打开 "**域管理员的高级安全设置**" 对话框。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. **为管理帐户选择 "允许 ACE"** ，然后单击 "**编辑**"。 验证该帐户是否已被授予对 DA 组的 "**读取成员**" 和 "**写入成员**" 权限，然后单击 **"确定"** 。  
  
8. 在 "**高级安全设置**" 对话框中单击 **"确定"** ，然后再次单击 **"确定"** 以关闭 DA 组的 "属性" 对话框。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 可以对域中的其他受保护组重复前面的步骤;所有受保护的组的权限都应该相同。 你现在已经完成了在此域中创建和配置受保护组的管理帐户。  
  
    > [!NOTE]  
    > 在 Active Directory 中有权写入组成员身份的任何帐户也可以将其自身添加到组。 此行为是设计的，不能禁用。 出于此原因，在不使用管理帐户时应始终将其禁用，并应在帐户被禁用和使用时对其进行密切监视。  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>正在验证组和帐户配置设置

现在，你已创建并配置了可修改域中受保护组的成员身份（包括最高特权的 EA、DA 和 BA 组）的管理帐户，你应验证这些帐户及其管理组是否已已正确创建。 验证包含以下常规任务：  
  
1.  测试可以启用和禁用管理帐户的组，以验证组的成员是否可以启用和禁用帐户并重置其密码，但不能在管理帐户上执行其他管理活动。  
  
2.  测试管理帐户，验证它们是否可以在域中的受保护组中添加和删除成员，但不能更改受保护帐户和组的任何其他属性。  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>测试将启用和禁用管理帐户的组
  
1.  若要测试启用管理帐户并重置其密码，请使用作为在 [Appendix I 中创建的组的成员的帐户登录到安全管理工作站：为 Active Directory @ no__t 中的受保护帐户和组创建管理帐户。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  打开**Active Directory 用户和计算机**"，右键单击管理帐户，然后单击"**启用帐户**"。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  应显示一个对话框，确认已启用该帐户。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下来，请重置管理帐户的密码。 为此，请再次右键单击该帐户，然后单击 "**重置密码**"。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  在 "**新密码**" 和 "**确认密码**" 字段中为该帐户键入一个新密码，然后单击 **"确定"** 。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  应该会出现一个对话框，确认帐户的密码已重置。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  现在尝试修改管理帐户的其他属性。 右键单击该帐户并单击 "**属性**"，然后单击 "**远程控制**" 选项卡。  
  
8.  选择 "**启用远程控制**"，然后单击 "**应用**"。 操作应失败并且应显示 "**访问被拒绝**" 错误消息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 单击帐户的 "**帐户**" 选项卡，尝试更改帐户的名称、登录时间或登录工作站。 所有这些都应该会失败，并且不由**userAccountControl**属性控制的帐户选项应显示为灰色且不可修改。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 尝试将管理组添加到受保护的组，如 DA 组。 单击 **"确定"** 后，将显示一条消息，通知您无权修改该组。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 根据需要执行其他测试，以验证你是否无法针对管理帐户配置任何内容（ **userAccountControl**设置和密码重置除外）。  
  
    > [!NOTE]  
    > **UserAccountControl**属性控制多个帐户配置选项。 向属性授予写入权限时，不能授予仅更改某些配置选项的权限。  
  
##### <a name="test-the-management-accounts"></a>测试管理帐户

现在您已启用了一个或多个帐户，该帐户可以更改受保护组的成员身份，您可以对这些帐户进行测试，以确保它们能够修改受保护的组成员身份，但不能对受保护的帐户和组执行其他修改。  
  
1.  以第一个管理帐户的身份登录到安全的管理主机。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  启动**Active Directory "用户和计算机**"，然后找到 "**域管理员" 组**。  
  
3.  右键单击 "**域管理员**" 组，然后单击 "**属性**"。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在 "**域管理员" 属性**中，单击 "**成员**" 选项卡，然后**单击**"添加"。 输入将拥有临时域管理员权限的帐户的名称，然后单击 "**检查名称**"。 当帐户的名称带有下划线时，单击 **"确定"** 返回到 "**成员**" 选项卡。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  在 "**域管理员属性**" 对话框的 "**成员**" 选项卡上，单击 "**应用**"。 单击 "**应用**" 后，该帐户应保留 DA 组的成员，并且不会收到错误消息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  单击 "**域管理员属性**" 对话框中的 "**管理者**" 选项卡，验证你不能在任何字段中输入文本并且所有按钮均灰显。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  单击 "**域管理员属性**" 对话框中的 "**常规**" 选项卡，然后验证是否无法修改该选项卡的任何相关信息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  根据需要为其他受保护的组重复这些步骤。 完成后，使用一个帐户登录到安全管理主机，该帐户是创建的组的成员，用于启用和禁用管理帐户。 然后重置刚刚测试的管理帐户的密码并禁用该帐户。 您已经完成了管理帐户和负责启用和禁用帐户的组的设置。  
