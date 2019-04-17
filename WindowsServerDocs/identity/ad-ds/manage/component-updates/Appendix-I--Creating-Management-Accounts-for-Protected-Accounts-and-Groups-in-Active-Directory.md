---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: "附录我的 Active Directory 中的受保护的帐户和组创建管理的帐户"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附录 i：创建管理帐户中的 Active Directory 的受保护的帐户和组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>附录 i：创建管理帐户中的 Active Directory 的受保护的帐户和组  

在实现 Active Directory 模型，不依赖于高权限的组成员永久挑战之一是必须机制填充需要临时组成员时的这些组。 某些身份特权的管理解决方案需要的软件的服务的帐户被授予永久如 DA 或系统管理员森林中的每个域中的组成员。 但是，则从技术上讲没有必要的特权身份管理 (PIM) 解决方案，运行此类高权限上下文中的它们的服务。  
  
此附录提供可用于为本地实现或第三方 PIM 解决方案创建帐户，仅受有限权限和可以来严格控制，但可用于需要临时提升时填充特权的组 Active Directory 中的信息。 如果实现 PIM 作为本机解决方案时，这些帐户可用于管理人员通过执行临时组填充中，并且如果你要通过第三方软件实现 PIM，你可能能够适应充当服务帐户这些帐户。  
  
> [!NOTE]  
> 此附录中所述的过程提供管理高权限的组 Active Directory 中的一种方法。 你可以调整适合你的需求，添加更多的限制，这些步骤或忽略某些此处所述的限制。  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>为受保护的帐户和 Active Directory 中的组创建管理帐户  
创建帐户，可用于管理而无需要授予过多的权限管理帐户的权限的组成员资格，它权限包含四个常规活动，述按照的分步说明进行操作：  
  
1.  首先，你应该创建将管理的帐户，一组，因为这些帐户应由一组有限的受信任的用户。 如果你已经没有 OU 结构，可容纳隔离特权和受保护的帐户和系统从常规样本域中，您应该创建一个。 尽管不提供此附录特定的说明进行操作，屏幕截图将显示此类 OU 层次它的一个示例。  
  
2.  创建管理的帐户。 这些帐户应该创建为"常规"用户帐户，并且可以授予以外已经授予默认用户没有用户权利。  
  
3.  使其可用，专门用于为其创建它们，除了控制谁可以启用并使用帐户 （你创建的第一步中的组） 仅管理帐户实现限制。  
  
4.  若要允许管理的帐户，以更改在域中权限的组成员的每个域中 AdminSDHolder 对象上配置权限。  
  
彻底应测试的所有这些过程，并根据需要您的环境生产环境中实现它们之前对其进行修改。 你还应该按预期方式工作了所有设置验证 （某些测试过程中提供了此附录），并且应测试灾难恢复方案中管理的帐户不是可用于填充进行恢复的受保护的组。 有关备份和还原 Active Directory 的详细信息，请参阅[广告 DS 备份和恢复分步指南](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)。  
  
> [!NOTE]  
> 通过实现此附录中所述的步骤，您将创建将能够管理每个域中的所有受保护的组、 不仅最高权限 Active Directory 组如 EAs、 DAs 和营业活动报表会员的帐户。 有关 Active Directory 中的组受保护的详细信息，请参阅[附录 c： 保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>创建适用于受保护组管理的帐户的分步说明  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>创建一组启用和禁用管理的帐户  
管理帐户应拥有自己的密码重置，在每次使用，并完成要求他们的活动时，应禁用。 尽管你还可以考虑实现这些帐户的智能卡登录要求，它是可选的配置并这些说明假定管理的帐户将具有用户名和密码长，复杂配置为最少的控件。 在此步骤中，您将创建重置密码管理帐户以启用和禁用帐户具有权限的组。  
  
若要创建一组启用和禁用管理的帐户，请执行以下步骤：  
  
1.  在 OU 结构进行了外壳管理的帐户中，右键单击你想要创建组，的 OU 单击**新建**单击**组**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  在**新对象-组**对话框中，输入组的名称。 如果您计划使用此组你森林中的所有管理帐户"都激活"，使其跨平台安全组。 如果你有一个域森林或者想要在每个域创建一组，可以创建一个全球安全组。 单击**确定**创建组。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  右键单击你刚创建的组中，单击**属性**，然后单击**对象**选项卡。中的组**对象属性**对话框中，选择**被意外删除保护对象**，这不会仅阻止其他方式授权的用户从删除组中，但还能从除非将其移动到另一个首次取消选择属性。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > 如果你已经配置权限的组父华丽绚烂限制管理于一组有限的用户，你可能不需要执行以下步骤。 在此处提供了，这样即使尚未实现 OU 结构在其中已创建此组有限管理控制权，你可以安全避免修改组未经授权的用户。  
  
4.  单击**成员**选项卡，然后将负责启用管理的帐户或填充团队成员保护组时需要有关添加帐户。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  如果你拥有不已执行此操作，请在**Active Directory 用户和计算机**控制台中，单击**视图**选择**高级功能**。 右键单击你刚创建的组中，单击**属性**，然后单击**安全**选项卡。在**安全**选项卡上，单击**高级**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  在**[群组] 高级安全设置**对话框中，单击**禁用继承**。 看到提示后，单击**转换继承为对此对象明确许可的权限**，单击**确定**返回到组**安全**对话框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  在**安全**选项卡上，删除应不允许访问此组中的组。 例如，如果你不希望通过验证能够阅读该组的名称和常规属性的用户，你可以删除该 ACE。 你还可以删除 Ace，如这些帐户运营商和 windows 以前版本 2000 Server 兼容访问。 你应该但是，保留一组最低对象权限。 保持不变的以下 Ace:  
  
    -   自行  
  
    -   系统  
  
    -   域管理员  
  
    -   企业管理员  
  
    -   管理员  
  
    -   Windows 授权的访问组 （如果适用）  
  
    -   企业域控制器  
  
    尽管这听起来可能很起来不够直观允许最高权限的组 Active Directory 管理此组中，您在实现这些设置的目标是不防止的这些组成员授权的更改。 而是权限的目标是权限的确保场合要求非常高后，将成功授权的更改。 这是由于此原因，更改默认特权组嵌套，权利，并且权限提倡本文档。 通过使默认结构保持不变并清空的目录中最高权限的组成员资格，则可以创建仍能按预期更安全的环境。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > 如果尚未的物体审核策略配置 OU 结构你在哪些位置创建此组中，你应该将配置审核日志更改此组。  
  
8.  你已经完成了用于"查看"组配置管理帐户时需要"时签入"帐户已完成的他们的活动。  
  
##### <a name="creating-the-management-accounts"></a>创建管理的帐户  
你应该创建至少一个用于管理特权安装的 Active Directory 中的组成员的帐户，最好的第二个帐户，以作为备份。 你选择在单个域树林中创建管理帐户并将它们授予所有域管理功能保护组，或者你选择在每个森林中的域中实现管理的帐户，过程是否有效地相同。  
  
> [!NOTE]  
> 本文档中的步骤假定，你尚未实现角色基于访问控件和特权的标识管理 Active Directory。 因此，必须用户的用户的帐户的问题的域的域管理员组成员通过执行一些过程。  
>   
> 当你使用帐户具有 DA 权限时，你可以登录到域控制器执行配置活动。 可以通过不太权限管理工作站到登录的帐户执行都不需要 DA 权限的步骤。 显示界定对话框中的浅蓝色的屏幕截图表示可以执行该域控制器的活动。 在更暗的蓝色中显示对话框中的屏幕截图表示可以在使用帐户，仅受有限权限管理的工作站执行的活动。  
  
若要创建管理的帐户，请执行以下步骤：  
  
1.  登录到的是域 DA 组成员的帐户的域控制器。  
  
2.  启动**Active Directory 用户和计算机**并导航到 OU 会位置创建管理的帐户。  
  
3.  右键单击 OU 和单击**新建**单击**用户**。  
  
4.  在**新对象-用户**对话框中，输入命名你所需的帐户信息，然后单击**下一步**。  
  
5.  登录到的是域 DA 组成员的帐户的域控制器。  
  
6.  启动**Active Directory 用户和计算机**并导航到 OU 会位置创建管理的帐户。  
  
7.  右键单击 OU 和单击**新建**单击**用户**。  
  
8.  在**新对象-用户**对话框中，输入命名你所需的帐户信息，然后单击**下一步**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. 有关用户帐户时，清除提供初始的密码**用户必须更改密码，在下次登录**，选择**用户无法更改密码**和**帐户已禁用**，单击**下一步**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. 验证帐户详细信息是否正确，然后单击**完成**。  
  
11. 右键单击你刚创建的用户对象，然后单击**属性**。  
  
12. 单击**帐户**选项卡。  
  
13. 在**帐户选项**字段中，选择**敏感帐户，不能委派**标志，选择**此帐户支持 Kerberos AES 128 位加密**和/或**此帐户支持 Kerberos AES 256 加密**标记，并单击**确定**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > 因为此帐户，例如其他帐户，必须有限，但功能强大的函数，应仅在安全管理主机上使用该帐户。 您的环境中所有安全管理主机，你应该考虑实现组策略设置**网络安全： 对于 Kerberos 允许配置加密类型**允许仅最安全的加密类型可以实现安全主机。  
    >   
    > 尽管为主机实现更安全的加密类型不能解决凭据盗窃攻击的相应使用和配置安全主机如此。 设置为仅供特权帐户的主机更强大的加密类型只需减少了这两台计算机的总体攻击 surface。  
    >   
    > 有关配置系统和帐户上的加密类型的详细信息，请参阅[Windows 配置中 Kerberos 支持加密类型](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)。  
    >   
    > **注意**这些设置仅在运行 Windows Server 2012、 Windows Server 2008 R2、 Windows 8 或 Windows 7 的计算机上受支持。  
  
14. 在**对象**选项卡上，选择**被意外删除保护对象**。 这将不仅阻止对象删除 （即使通过授权的用户），但会阻止它移到另一个管理单元在广告 DS 分层中，除非具有更改特性权限的用户的首次清除该复选框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. 单击**遥控器**选项卡。  
  
16. 清除**启用遥控器**标志。 它不应必需的支持人员，若要连接到此帐户会话实现修补程序。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > 每个中 Active Directory 对象应有指定的 IT 所有者和指定的业务所有者中所述[规划危害](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)。 如果在跟踪中 Active Directory （而不是外部数据库） 的广告 DS 对象的所有权时，你应该输入此对象属性相应拥有信息。  
    >   
    > 在此情况下，业务者，很可能 IT 部门、 andthere 是还未 IT 所有者商家没有禁止。 的建立对象的所有权一点允许你为时需要更改不会对对象，可能是年从其初始创建识别联系人。  
  
17. 单击**组织**选项卡。  
  
18. 在你的广告 DS 对象标准输入所需的任何信息。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. 单击**拨号中**选项卡。  
  
20. 在**网络访问权限**字段中，选择**拒绝访问**。此帐户应永远不需要通过远程连接。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > 不太可能会使用此帐户登录到您的环境中仅阅读域控制器 (Rodc)。 但是，应情况是否需要的帐户登录到 RODC，你应该帐户添加到拒绝 RODC 密码复制组以使其密码没有缓存 RODC 在。  
    >   
    > 尽管应在每个使用后重置该帐户的密码，应禁用该帐户，实现此设置没有 deleterious 影响该帐户，并且在某些情况下，忘记用重置该帐户的密码，并将其禁用的管理员获取帮助。  
  
21. 单击**隶属于**选项卡。  
  
22. 单击**添加**。  
  
23. 键入**拒绝 RODC 密码复制组**中**选择用户联系人计算机**对话框中，单击**检查名称**。 当组的名称有下划线对象选取器中时，单击**确定**并验证该帐户现下面的屏幕截图中显示的两个组中的成员。 请勿向任何受保护的组添加该帐户。  
  
24. 单击**确定**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. 单击**安全**选项卡，然后单击**高级**。  
  
26. 在**高级安全设置**对话框中，单击**禁用继承**和复制继承直接权限，作为的权限，然后单击**添加**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. 在**[帐户] 权限条目**对话框中，单击**选择主体**并添加了以前的过程中创建的组。 滚动到底部的对话框中，单击**清除所有**删除所有默认权限。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. 滚动到顶部**权限项**对话框。 确保**类型**下拉列表中设置为**允许**，并在**适用于**下拉列表中，选择**只是这个对象**。  
  
29. 在**权限**字段中，选择**阅读所有属性**，**都阅读权限**，并**重置密码**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. 在**属性**字段中，选择**阅读 userAccountControl**和**编写 userAccountControl**。  
  
31. 单击**确定**，**确定**再次**高级安全设置**对话框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > **UserAccountControl**特性控件多个帐户配置选项。 你无法授予权限，更改仅有一些配置选项，当您授予写入特性权限。  
  
32. 在**组或用户名**字段**安全**选项卡上，删除任何应该不允许访问或管理的帐户的组。 如每个人群发和自行计算帐户，请不要删除已配置拒绝 Ace，使用任何组 (时设置该 ACE**用户无法更改密码**标志已启用该帐户创建期间。 也不要删除的该组刚添加、 系统帐户或组例如 EA、 DA、 栏或 Windows 授权的访问权限的组。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. 单击**高级**并验证高级安全设置对话框中类似下面的屏幕截图。  
  
34. 单击**确定**，并**确定**以关闭的帐户属性对话框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. 设置的第一管理的帐户现已完成。 更高版本的过程中，您将测试该帐户。  
  
###### <a name="creating-additional-management-accounts"></a>创建管理其他帐户  
你可以创建管理其他帐户，重复上述步骤、 复制你刚创建的帐户或创建一个创建帐户与你所需的配置设置脚本。 但是请注意，是否复制你刚创建的帐户的自定义的设置和 Acl 许多不复制到新帐户，并且你将需要重复大部分配置步骤。  
  
你可以创建一个组您向其中委派权利填充并 unpopulate 受保护的组，但你将需要该组，你将在其中的帐户安全。 应你的目录中很少授予管理的受保护的组成员的功能的帐户，因为创建个人帐户可能的最简单方法。  
  
无论你选择如何创建一组您向其中位置管理的帐户，你应确保每个帐户受到如上文所述。 你还应该考虑实现类似于，述 GPO 限制[附录 d： 保护内置管理员帐户中 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
###### <a name="auditing-management-accounts"></a>审核管理的帐户  
你应该配置审核至少登录该帐户写入的所有帐户。 这将允许你不仅识别成功的帐户启用和期间授权的使用，但也找出未经授权的用户尝试操控帐户的密码重置。 失败的写入帐户上应捕获你的安全信息和事件监视 (SIEM) 在系统中 （如果适用），并应触发向负责调查潜在危害的人员提供通知的通知。  
  
SIEM 解决方案拍摄来自涉及的安全来源 （如事件日志、 应用程序的数据、 网络流、 反恶意软件产品和入侵检测源） 的事件信息、 整理数据，并尝试使智能视图和预防措施。 有许多商业 SIEM 解决方案，并且许多企业创建专用实现。 安全监视和事件响应功能，也设计和相应实施的 SIEM 显著可以增强。 但是，功能和准确程度而异有很大的解决方案。 SIEMs 超出范围内本文中，但认为包含的特定的事件建议任何 SIEM 实施者。  
  
有关域控制器推荐的审核配置设置的详细信息，请参阅[监视 Active Directory 中是否的危害的迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。 中提供了域控制器特定配置设置[监视 Active Directory 中是否的危害的迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)。  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>启用管理修改受保护的组成员的帐户  
在此过程中，您将配置上的域 AdminSDHolder 对象允许修改受保护的域中的组成员的新创建的管理帐户的权限。 无法通过图形用户界面 (GUI) 执行此过程。  
  
中所述[附录 c： 保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)，SDProp 任务运行时的对象有效"复制"到某个域 AdminSDHolder ACL 受保护的对象。 组受保护和帐户不文件沿用文件其权限从 AdminSDHolder 对象;显式设置其权限相符 AdminSDHolder 对象上。 因此，在修改 AdminSDHolder 对象的权限时，则必须修改它们适用于受保护的对象有定位类型的特性。  
  
在此情况下，你将授予新建的管理帐户以允许他们读取和写入成员特性组对象上。 但是，AdminSDHolder 对象组对象并不图形 ACL 编辑器中不会公开组属性。 出于此原因，你将实现 Dsacls 命令行实用程序通过权限的更改是。 若要授予 （已禁用） 管理修改受保护的组成员的帐户权限，请执行以下步骤：  
  
1.  域控制器，最好是域控制器按 PDC 仿真器 (PDCE) 角色，与已 DA 组成员的域中的用户帐户凭据登录。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  打开提升了权限的命令提示符下，方法是右键单击**权限的命令提示符**单击**以管理员身份运行**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  当批准提升提示，单击**是**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > 有关 Windows 中的海拔和用户帐户控制 (UAC) 的详细信息，请参阅[UAC 进程和交互](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx)TechNet 网站上。  
  
4.  在命令提示符下，键入 （替换领域特有信息） **Dsacls [你的域中 AdminSDHolder 对象识别名] /G [管理帐户 UPN]: RPWP; 成员**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    上一个命令 （这不区分大小写），如下所示的工作原理：  
  
    -   Dsacls 设置，或在 directory 对象将显示 Ace  
  
    -   CN = AdminSDHolder，CN = 系统特区 = TailSpinToys 特区 = msft 标识对象进行修改  
  
    -   /G 指示正在配置授予 ACE  
  
    -   PIM001@tailspintoys.msft是名称 UPN) 将授予 Ace 的安全主要  
  
    -   RPWP 授予阅读属性，然后写属性权限  
  
    -   有关设置权限的成员是个属性） 的名称  
  
    有关详细信息的使用**Dsacls**，不带任何参数在命令提示符下键入 Dsacls。  
  
    如果你已创建的域的多个管理帐户，你应该运行每个帐户 Dsacls 命令。 当你完成上 AdminSDHolder 对象 ACL 配置时，应该强制 SDProp 以运行，或者等到完成其计划的运行。 有关如何强制 SDProp 运行的信息，请参阅"手动运行 SDProp"在[附录 c： 保护帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    SDProp 已经在运行时，可验证，已对 AdminSDHolder 对象所做的更改应用于受保护的域中的组。 你无法验证此通过之前所述，原因 AdminSDHolder 对象中查看 ACL，但你可以进行验证，已通过受保护组上查看 Acl 应用权限。  
  
5.  在**Active Directory 用户和计算机**，确认你已启用**高级功能**。 若要执行此操作，请单击**视图**，找到**域管理员**分组、 右键单击该组和单击**属性**。  
  
6.  单击**安全**选项卡，然后单击**高级**打开**的域管理员高级安全设置**对话框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  选择**管理帐户允许 ACE**单击**编辑**。 验证该帐户已被只授予**读取成员**和**写入成员**权限 DA 组中，然后单击**确定**。  
  
8.  单击**确定**中**高级安全设置**对话框中，单击**确定**来关闭 DA 组属性对话框。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. 你可以重复上述步骤其他受到保护中的组域;权限应相同的所有受保护的组。 现在，你已完成创建和配置管理帐户此域中的受保护的组。  
  
    > [!NOTE]  
    > 有权编写的一组成员 Active Directory 中的任何帐户还可以添加自己的组。 此问题是由设计，并且无法禁用。 出于此原因，你应始终保持管理帐户禁用在不使用时，并应密切监视帐户，如果他们在禁用，当他们使用。  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>验证组和配置设置的帐户  
现在，你已创建并管理的帐户，可以进行修改受保护 （其中包括最高权限的 EA、 DA 和栏组） 的域中的组成员的配置，你应该验证已正确创建帐户和管理他们的组。 验证包含这些常规任务：  
  
1.  测试的组，可以启用和禁用管理的帐户，以验证的成员组可以启用和禁用帐户和重置密码，但是不能管理的帐户执行其他管理的活动。  
  
2.  测试管理的帐户，若要验证可以将他们添加和删除成员受保护的域中的组但无法更改受保护的帐户和组任何其他属性。  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>测试的组这将启用和禁用管理的帐户  
  
1.  若要测试启用管理帐户和重置它的密码，请登录到中创建的组成员的帐户安全管理工作站[附录 i： 创建管理的帐户中 Active Directory 的受保护的帐户和组](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  打开**Active Directory 用户和计算机**、 右键单击管理帐户，然后单击**启用帐户**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  应显示对话框中，请确认启用了该帐户。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  接下来，重置管理的帐户的密码。 若要执行此操作，请再次右键单击该帐户，然后单击**重置密码**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  键入的帐户中的新密码**新密码**和**确认密码**字段，然后单击**确定**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  应该会显示一个对话框中，确认已被帐户的密码重置。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  现在尝试修改管理的帐户的其他属性。 右键单击该帐户，然后单击**属性**，然后单击**遥控器**选项卡。  
  
8.  选择**启用遥控器**单击**应用**。 操作应失败并**拒绝访问**应显示错误消息。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. 单击**帐户**选项卡上的帐户，并尝试更改的帐户名称、 登录的时间或登录工作站。 所有应失败，并且用户不受的选项**userAccountControl**应灰特性，而不适用于修改。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. 尝试将管理组添加到如 DA 组受保护的组。 在你单击时**确定**，应显示一条消息，通知你没有修改组的权限。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. 当要求验证，你无法上管理帐户，除配置任何内容，请执行其他测试**userAccountControl**设置和密码重置。  
  
    > [!NOTE]  
    > **UserAccountControl**特性控件多个帐户配置选项。 你无法授予权限，更改仅有一些配置选项，当您授予写入特性权限。  
  
###### <a name="test-the-management-accounts"></a>测试管理的帐户  
现在，你已启用可以更改的受保护的组成员的一个或多个帐户，你可以在帐户后，从而确保他们可以修改受保护的组成员资格，但无法执行其他修改受保护的帐户和组测试。  
  
1.  第一个管理的帐户登录到的安全管理主机上。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  启动**Active Directory 用户和计算机**并找到**域管理员组**。  
  
3.  右键单击**域管理员**分组，并单击**属性**。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  在**域管理员属性**，单击**成员**选项卡和**单击**添加。 输入获得临时域管理员权限和单击的帐户名称**检查名称**。 当有下划线的帐户名称时，请单击**确定**返回到**成员**选项卡。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  在**成员**的选项卡**域管理员属性**对话框中，单击**应用**。 单击后**应用**帐户就应保持 DA 组的成员，你应该会收到任何错误消息。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  单击**管理者**选项卡**域管理员属性**对话框框中，并验证你不能在任何字段中输入文本，以及所有按钮显示为都灰色。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  单击**常规**选项卡**域管理员属性**对话框框中，并验证你无法修改任何关于该选项卡的信息。  
  
    ![创建管理的帐户](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  根据需要额外的保护组中重复这些步骤。 完成后，登录的组成员的帐户安全管理主机保持你创建启用和禁用管理帐户。 然后，重置你只需进行测试，并禁用帐户管理帐户的密码。 你已完成设置的管理帐户并将负责启用和禁用帐户的组。  
  


