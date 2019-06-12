---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: 附录 I-创建受保护的帐户和 Active Directory 中的组的管理帐户
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e90c2c075ba2dc2b63e9a18c9eba192116265b90
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443523"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附录 i:为受保护的帐户和组在 Active Directory 中的创建管理帐户

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

实现不依赖于高特权组中的永久成员资格的 Active Directory 模型所面临的挑战之一是必须是一种机制来填充这些组，需要临时成员资格的组中时。 某些特权的标识管理解决方案都需要的软件的服务帐户授予如 DA 或林中的每个域中的管理员组中的永久成员资格。 但是，它从技术上讲不是必要的 Privileged Identity Management (PIM) 解决方案在这种高特权上下文中运行其服务。  
  
本附录提供了信息，可用于针对本机实现或第三方 PIM 解决方案创建的具有有限的特权和可以进行严格控制，但是可以用来填充 Active Directory 中的特权的组帐户时临时提升是必需的。 如果要实现 PIM 作为本机解决方案，这些帐户可用于通过管理人员执行临时组填充，并且如果要通过第三方软件实现 PIM，您可能能够适应这些帐户充当服务帐户。  
  
> [!NOTE]  
> 在本附录中所述的过程提供在 Active Directory 中的高特权组的管理的一种方法。 可以调整这些过程，以满足您的需要，添加其他限制，也可以忽略某些此处所述的限制。  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>为受保护的帐户和组在 Active Directory 中的创建管理帐户

创建帐户可用于管理特权组的成员身份，无需过多的权限和权限授予管理帐户包含的分步说明中所述的四个常规活动的后续操作：  
  
1.  首先，应创建的组，将管理帐户，因为这些帐户应由一组有限的受信任的用户。 如果你还没有可容纳隔开到特权和受保护帐户和域中的总体系统的 OU 结构，则应创建一个。 尽管本附录中未提供具体说明，但屏幕截图显示此类的 OU 层次结构的示例。  
  
2.  创建管理帐户。 这些帐户应创建为"regular"的用户帐户并不授予任何用户权限之外，默认情况下已授予给用户。  
  
3.  实现使其可仅为专门用途为其创建了它们，除了控制谁可以启用和使用的帐户 （第一步中创建的组） 的管理帐户的限制。  
  
4.  若要允许管理帐户的域中的特权组成员身份更改每个域中的 AdminSDHolder 对象上配置权限。  
  
应彻底测试所有这些过程并根据需要在生产环境中实施建议之前为您的环境修改这些。 您还应该验证所有设置按预期方式都工作 （某些测试过程中提供了本附录），并且应测试灾难恢复方案，在其中管理帐户不是可供使用，以填充为恢复的受保护的组目的。 有关备份和还原 Active Directory 的详细信息，请参阅[AD DS 备份和恢复循序渐进指南](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 通过实现在本附录中所述的步骤，您将创建将能够管理所有受保护的组中的每个域，不仅的最高特权 Active Directory 组如 EAs、 DAs 和 BAs 的成员身份的帐户。 有关 Active Directory 中的受保护组的详细信息，请参阅[附录 c:受保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>为受保护组创建管理帐户的分步说明  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>创建组来启用和禁用的管理帐户

管理帐户应具有在每次使用时重置其密码，并要求他们的活动均已完成时，应禁用。 尽管您还可以考虑实现这些帐户的智能卡登录要求，但它是一项可选配置和这些说明假定，将使用用户名和作为最小值的长复杂密码来配置管理帐户控件。 在此步骤中，将创建具有重置密码的管理帐户，还可以启用和禁用帐户的权限的组。  
  
若要创建组来启用和禁用的管理帐户，请执行以下步骤：  
  
1.  在 OU 结构中为其中容纳管理帐户，右键单击你想要创建的组的 OU 单击**新建**然后单击**组**。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在中**新建对象-组**对话框框中，输入组的名称。 如果您计划使用此组来"激活"在您的林中的所有管理帐户，使其通用安全组。 如果你具有的单域林，或如果您计划在每个域中创建一个组，可以创建一个全局安全组。 单击  “确定”以创建组。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  右键单击刚创建的组，单击“属性”  ，然后单击“对象”  选项卡。在组的**对象属性**对话框中，选择**防止对象被意外删除**，这不会仅阻止以其他方式授权的用户删除组，还可以从将其移到另一个 OU 除非该属性第一个被取消选择。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > 如果已配置组的父 Ou，使其限制为一组有限的用户的管理权限，可能不需要执行以下步骤。 在此处提供了，这样即使尚未实现对已在其中创建此组的 OU 结构的有限管理控制，可以保护组修改针对未经授权的用户。  
  
4.  单击**成员**选项卡，然后将负责启用的管理帐户或填充团队成员受保护的必要时组添加帐户。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果你具有尚未这样做，在**Active Directory 用户和计算机**控制台中，单击**视图**，然后选择**高级功能**。 右键单击刚刚创建的组，单击**属性**，然后单击**安全**选项卡。在“安全”  选项卡中，单击“高级”  。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在中 **[组] 的高级安全设置**对话框中，单击**禁用继承**。 出现提示时，单击**继承权限对此对象的显式权限转换为**，然后单击**确定**若要返回到的组**安全**对话框。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  上**安全**选项卡上，删除不应允许访问此组的组。 例如，如果不希望进行身份验证的用户能够读取组的名称和常规属性，可以删除该 ACE。 此外可以删除 Ace，例如针对帐户运算符和 pre-Windows 2000 Server 兼容的访问。 但是，您应该则就地保留最小对象权限集。 将以下 Ace 保留不变：  
  
    -   SELF  
  
    -   系统  
  
    -   Domain Admins  
  
    -   Enterprise Admins  
  
    -   Administrators  
  
    -   Windows Authorization Access Group （如果适用）  
  
    -   企业域控制器  
  
    尽管它可能看起来有悖常理允许的最高特权的组在 Active Directory 来管理此组中，你在执行这些设置的目标是不以防止这些组的成员进行授权的更改。 相反，目标是权限的确保时有某种情况下需要极高，授权的更改将会成功。 正是出于此原因，更改默认特权组嵌套，权限，并且权限不建议使用在本文档。 通过使默认结构保持不变并清空目录中的最高特权组的成员身份，你可以创建更安全的环境，仍可以按照预期方式。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果尚未在其中创建此组的 OU 结构中配置的对象的审核策略，则应配置审核来记录更改此组。  
  
8.  已完成将用于"查看"组的配置管理帐户时需要"签入"帐户时其活动已完成。  
  
#### <a name="creating-the-management-accounts"></a>创建管理帐户

应创建至少一个将用于管理 Active Directory 安装中的特权组的成员身份的帐户，最好要用作备份的第二个帐户。 是选择要在林中的单个域中创建管理帐户并授予其管理功能的所有域的受保护组，或选择在每个林中的域中实现的管理帐户，这些过程是否有效地相同。  
  
> [!NOTE]  
> 本文档中的步骤假定您已经不尚未实现基于角色的访问控制和 Active Directory 的 privileged 的 identity management。 因此，必须由其帐户是问题的域的 Domain Admins 组的成员的用户执行某些过程。  
>   
> 时使用的是帐户使用 DA 权限，你可以登录到要执行的配置活动的域控制器。 可以通过在登录到管理工作站的较低权限帐户执行不需要 DA 权限的步骤。 以较浅蓝色显示界定的对话框的屏幕截图表示可以在域控制器执行的活动。 在更深蓝色显示对话框的屏幕截图表示可以使用具有有限特权的帐户的管理工作站执行的活动。  
  
若要创建的管理帐户，请执行以下步骤：  
  
1. 登录到域控制器是域的数据组的成员的帐户。  

2. 启动**Active Directory 用户和计算机**并导航到要在创建管理帐户的 OU。  

3. 右键单击 OU，然后单击**新建**然后单击**用户**。  

4. 在中**新建对象-用户**对话框中，输入所需命名信息的帐户，单击**下一步**。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. 清除为该用户帐户，提供初始密码**用户必须更改密码，下次登录时须**，选择**用户不能更改密码**并**帐户已禁用**，和单击**下一步**。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. 验证帐户详细信息是否正确，然后单击**完成**。  

7. 右键单击刚创建的用户对象，然后单击**属性**。  

8. 单击**帐户**选项卡。  

9. 在中**帐户选项**字段中，选择**敏感帐户，不能被委派**标志中，选择**此帐户支持 Kerberos AES 128 位加密**和/或**此帐户支持 Kerberos AES 256 加密**标志，并单击**确定**。  

   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > 由于此帐户，类似于其他帐户，将具有有限的但功能强大的函数，该帐户应仅使用安全管理主机上。 对于您的环境中所有安全管理主机，则应考虑实现组策略设置**网络安全：配置 Kerberos 允许的加密类型**以允许仅是最安全的加密类型可以在实现安全的主机。  
   >
   > 尽管对于主机实现更安全的加密类型不能解决凭据被盗攻击，相应的使用和配置安全主机执行。 对于仅由特权帐户使用的主机设置更强的加密类型只是可减少计算机的总体攻击面。  
   >
   > 有关在系统和帐户上配置的加密类型的详细信息，请参阅[Kerberos 支持的加密类型的 Windows 配置](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)。  
   >
   > 仅在运行 Windows Server 2012、 Windows Server 2008 R2、 Windows 8 或 Windows 7 的计算机上支持这些设置。  
  
10. 上**对象**选项卡上，选择**防止对象被意外删除**。 这不会仅阻止对象被删除 （即使由授权用户），但将其阻止被移到另一个管理单元中 AD DS 层次结构，除非由具有权限才能更改该属性的用户先清除该复选框。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. 单击**遥控器**选项卡。  

12. 清除**启用远程控制**标志。 它不应是必需的支持人员以连接到此帐户的会话进行修补。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > 在 Active Directory 中的每个对象应具有指定的 IT 所有者和指定的业务所有者中所述[规划泄露](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)。 如果要跟踪的 Active Directory 中 （而不是外部数据库） 的 AD DS 对象的所有权，则应在此对象的属性中输入相应所有权信息。  
    >
    > 在这种情况下，业务所有者很有可能是 IT 部门，andthere 是对业务所有者也正在 IT 所有者没有禁止。 建立对象的所有权的点是可用于标识联系人时需要对对象，可能是年从其初始创建更改。  

13. 单击**组织**选项卡。  

14. 在 AD DS 对象标准中输入所需的任何信息。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. 单击**拨入**选项卡。  

16. 在中**网络访问权限**字段中，选择**拒绝访问**。此帐户应永远不需要通过远程连接进行连接。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > 不太可能此帐户将用于登录到你的环境中的只读域控制器 (Rodc) 上。 但是，应情况曾经需要用于登录的帐户到 RODC，你应将此帐户添加到拒绝的 RODC 密码复制组，以便不在 RODC 上缓存其密码。  
    >
    > 虽然每次使用后应重置帐户的密码，并且此帐户应被禁用，实现此设置不具有对该帐户的负面影响和管理员忘记重置的帐户的情况下，它可能会帮助密码，将其禁用。  

17. 单击 **“隶属于”** 选项卡。  

18. 单击**添加**。  

19. 类型**Denied RODC Password Replication Group**中**选择用户、 联系人、 计算机**对话框中，单击**检查名称**。 在对象选取器中带下划线的组的名称，单击**确定**和验证该帐户现在是否显示以下屏幕截图中的两个组的成员。 不向任何受保护组添加帐户。  

20. 单击 **“确定”** 。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. 单击**安全**选项卡，单击**高级**。  

22. 在中**高级安全设置**对话框中，单击**禁用继承**并将复制为显式权限是继承的权限，然后单击**添加**。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. 在中 **[Account] 的权限条目**对话框中，单击**选择主体**并添加前面步骤中创建的组。 滚动到底部的对话框中，单击**清除所有**要删除所有默认权限。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. 滚动到顶部**权限条目**对话框。 絋粄**类型**下拉列表设置为**允许**，然后在**适用于**下拉列表中，选择**仅此对象**。  

25. 在中**权限**字段中，选择**读取所有属性**，**都读取权限**，以及**重置密码**。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. 在中**属性**字段中，选择**读取 userAccountControl**并**编写 userAccountControl**。  

27. 单击**确定**，**确定**中再次**高级安全设置**对话框。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > **UserAccountControl**属性控制多个帐户配置选项。 不能授予权限以授予对属性的写入权限时，只有某些配置选项的更改。  

28. 在中**组或用户名**字段**安全**选项卡上，删除任何不应允许访问或管理帐户的组。 不要删除任何已配置了拒绝 Ace 的组，例如 Everyone 组和自助计算帐户 (时设置该 ACE**用户不能更改密码**帐户的创建过程中启用标志。 也不要删除刚添加的组、 SYSTEM 帐户或组如 EA、 DA、 BA 或 Windows Authorization Access Group。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. 单击**高级**并验证高级安全设置对话框中看起来类似于以下屏幕截图。  

30. 单击**确定**，并**确定**以关闭该帐户的属性对话框。  

    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. 第一个管理帐户的安装程序现已完成。 在更高版本的过程中，将测试帐户。  

##### <a name="creating-additional-management-accounts"></a>创建附加的管理帐户

重复前面的步骤、 复制刚刚创建的帐户或创建脚本以使用你所需的配置设置创建帐户，可以创建附加的管理帐户。 但请注意，是否复制刚刚创建的帐户，许多自定义的设置和 Acl 不会复制到新的帐户和将具有重复的大多数配置步骤。  
  
您可以改为创建的组你向其委派权限，以填充和 unpopulate 的受保护的组，但您将需要保护的组并将其放入其中的帐户。 应在目录中的很少帐户授予管理受保护组的成员身份的能力，因为创建个人帐户可能是最简单的方法。  
  
无论您选择如何创建放置管理帐户的组，应确保每个帐户进行保护，如前面所述。 此外应考虑实现类似于中所述的 GPO 限制[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
##### <a name="auditing-management-accounts"></a>审核管理帐户

你应配置进行最小值、 记录所有写入到的帐户的帐户的审核。 这样，不仅可以确定成功启用的帐户和重置其密码期间授权的使用，但还标识未经授权的用户尝试操作帐户。 该帐户上的失败的写入应捕获安全信息和事件监视 (SIEM) 系统中 （如果适用），并应触发提供向其他人员负责调查潜在威胁的通知的警报。  
  
SIEM 解决方案所涉及的安全源 （例如，事件日志、 应用程序数据、 网络流、 反恶意软件产品和入侵检测源） 中获取事件的信息，整理数据，并尝试进行智能视图和主动操作. 有许多商业 SIEM 解决方案中，并且许多企业创建私有实现。 安全监视和事件响应功能，可以显著提高设计良好，适当地实现 SIEM。 但是，功能和准确性有很大差异解决方案之间。 Siem 之间存在不在本文中，范围，但任何 SIEM 实施者应被视为包含的特定事件建议。  
  
有关建议的审核的域控制器的配置设置的详细信息，请参阅[泄漏符号的监视 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 域控制器特定的配置设置中提供了[泄漏符号的监视 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>启用要修改受保护组的成员身份的管理帐户

在此过程中，将为允许新创建的管理帐户以修改域中的受保护组的成员身份的域的 AdminSDHolder 对象上配置权限。 不能通过图形用户界面 (GUI) 执行此过程。  
  
如中所述[附录 c:受保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)，SDProp 任务运行时上域的 AdminSDHolder 对象将有效地"复制"到 ACL 受保护的对象。 受保护的组和帐户不继承其权限从 AdminSDHolder 对象;其权限显式设置为与 AdminSDHolder 对象相匹配。 因此，当修改 AdminSDHolder 对象上的权限，则必须修改它们的属性适合于您的目标的受保护对象的类型的。  
  
在这种情况下，您将会向新创建的管理帐户以允许它们以供阅读和组对象上的写入成员属性。 但是，AdminSDHolder 对象不是组对象和图形的 ACL 编辑器中未公开属性进行分组。 正是出于此原因，您将实现通过 Dsacls 命令行实用程序的权限更改。 若要授予 （已禁用） 管理帐户权限来修改受保护组的成员身份，请执行以下步骤：  
  
1. 登录到域控制器，最好是包含具有已 DA 组的成员在域中的用户帐户的凭据的 PDC 模拟器 (PDCE) 角色的域控制器上。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. 通过右键单击打开提升的命令提示符**命令提示符**然后单击**以管理员身份运行**。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. 当系统提示您批准提升，请单击**是**。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > 有关在 Windows 中的提升和用户帐户控制 (UAC) 的详细信息，请参阅[UAC 进程和交互](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)TechNet 网站上。  
  
4. 在命令提示符下键入 （替换为你的特定于域的信息） **Dsacls [你的域中的 AdminSDHolder 对象的可分辨名称] [管理帐户 UPN] /G: RPWP; 成员**。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   前一命令 （这是不区分大小写） 的工作方式如下：  
  
   - Dsacls 设置或显示目录对象上的 Ace  
  
   - CN = AdminSDHolder，CN = System，DC = TailSpinToys，DC = msft 标识要修改的对象  
  
   - /G 指示正在配置授予 ACE  
  
   - PIM001@tailspintoys.msft 是用户将向其授予 Ace 的安全主体的主体名称 (UPN)  
  
   - RPWP 授予读取属性和写入属性权限  
  
   - 成员是属性 （属性） 的名称上设置的权限  
  
   有关使用的详细信息**Dsacls**，不带任何参数在命令提示符下键入 Dsacls。  
  
   如果已创建的域的多个管理帐户，应运行的每个帐户的 Dsacls 命令。 完成的 AdminSDHolder 对象上的 ACL 配置后，应强制 SDProp 运行，或等待，直到其计划的运行完成。 有关如何强制 SDProp 运行信息，请参阅"手动运行 SDProp"中[附录 c:受保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
   SDProp 开始运行后，可以验证具有已对 AdminSDHolder 对象所做的更改应用到域中的受保护组。 您不能通过 AdminSDHolder 对象上的 ACL 查看前面所述，出于对此进行验证，但您可以验证已将权限应用通过查看受保护组的 Acl。  
  
5. 在中**Active Directory 用户和计算机**，验证是否已启用**高级功能**。 若要执行此操作，请单击**视图**，找到**Domain Admins**组中，右键单击组，然后单击**属性**。  
  
6. 单击**安全**选项卡，单击**高级**以打开**域管理员的高级安全设置**对话框。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. 选择**管理帐户的允许 ACE**然后单击**编辑**。 验证该帐户授予仅**读取成员**并**写入成员**DA 组，然后单击权限**确定**。  
  
8. 单击**确定**中**高级安全设置**对话框中，然后单击**确定**以关闭 DA 组的属性对话框。  
  
   ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 可以为域; 中其他受保护组重复前面的步骤权限应为相同的受保护的所有组。 现在已完成创建和配置此域中的受保护组的管理帐户。  
  
    > [!NOTE]  
    > 任何有权在 Active Directory 中编写的组成员身份的帐户还可以添加本身到组。 此行为是设计使然，并且不能禁用。 出于此原因，应始终保留在不使用时, 禁用的管理帐户和它们处于禁用状态以及在使用时应密切监视帐户。  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>验证组和帐户的配置设置

现在，创建并配置可以修改域 （其中包括最高特权的 EA、 DA 和 BA 组） 中的受保护组的成员身份的管理帐户后，你应验证的帐户和其管理组已正确创建。 验证包含这些常规任务：  
  
1.  测试可以启用和禁用验证的成员的组可以启用和禁用帐户和重置其密码，但不能执行其他管理活动上的管理帐户的管理帐户的组。  
  
2.  测试要验证可以添加和删除成员到受保护域中的组，但不能更改受保护的帐户和组的任何其他属性的管理帐户。  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>测试将启用和禁用的管理帐户的组
  
1.  若要测试启用了管理帐户并重置其密码，请登录到工作站的安全管理是组的成员的帐户中创建[附录 i:创建管理帐户的受保护帐户和组在 Active Directory 中](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  打开**Active Directory 用户和计算机**，右键单击管理帐户，然后单击**启用帐户**。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  应显示一个对话框，确认已启用该帐户。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下来，管理帐户的密码重置。 为此，请再次右键单击该帐户，然后单击**重置密码**。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  键入用于中的帐户的新密码**新密码**并**确认密码**字段，然后单击**确定**。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  应显示一个对话框，并确认帐户的密码已重置。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  现在尝试修改的管理帐户的其他属性。 右键单击该帐户，然后单击**属性**，然后单击**远程控制**选项卡。  
  
8.  选择**启用远程控制**然后单击**应用**。 操作应失败和一个**拒绝访问**应显示错误消息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 单击**帐户**选项卡上的帐户并尝试将更改该帐户的名称、 登录时间或登录的工作站。 所有应失败，并且帐户不受控制的选项**userAccountControl**属性应该灰显并且不可用进行修改。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 尝试向如 DA 组的受保护组添加管理组。 当您单击**确定**，应显示一条消息，通知您没有修改组的权限。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 执行其他测试，因为验证，您不能在上的管理帐户上配置的任何内容所需**userAccountControl**设置和重置密码。  
  
    > [!NOTE]  
    > **UserAccountControl**属性控制多个帐户配置选项。 不能授予权限以授予对属性的写入权限时，只有某些配置选项的更改。  
  
##### <a name="test-the-management-accounts"></a>测试管理帐户

现在，已启用一个或多个可以更改受保护组的成员身份的帐户，可以测试以确保它们可以修改受保护的组成员身份，但不能对受保护的帐户和组执行其他修改的帐户。  
  
1.  第一个管理帐户登录到安全管理主机上。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  启动**Active Directory 用户和计算机**并找到**Domain Admins 组**。  
  
3.  右键单击**Domain Admins**组，并单击**属性**。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在中**Domain Admins 属性**，单击**成员**选项卡并**单击**添加。 输入该帐户会提供临时域管理员权限和单击名称**检查名称**。 当带下划线的帐户的名称时，单击**确定**回到**成员**选项卡。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  上**成员**选项卡上的**Domain Admins 属性**对话框中，单击**应用**。 单击后**应用**，该帐户应保留 DA 组的成员，您应收到没有错误消息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  单击**管理者**选项卡**Domain Admins 属性**对话框框，然后验证不能在任何字段中输入文本，并且所有按钮呈都灰显。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  单击**常规**选项卡**Domain Admins 属性**对话框框，然后验证你是否无法修改任何有关该选项卡的信息。  
  
    ![创建管理帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  根据需要受保护的其他组重复这些步骤。 完成后，登录到该帐户是组的成员具有的安全管理主机您创建的用于启用和禁用的管理帐户。 然后只需进行测试并禁用该帐户的管理帐户的密码重置。 已完成安装程序的管理帐户和将负责启用和禁用帐户的组。  
