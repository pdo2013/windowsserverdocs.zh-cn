---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: "附录 G-保护管理员组 Active Directory 中"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cf89f7bf31ce848de384778b0213d0ddc822158
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 g：保护中 Active Directory 的管理员组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 g：保护中 Active Directory 的管理员组  
一样与企业管理员 (EA) 和域管理员 (DA) 组这种情况，应仅在版本或灾难恢复方案中所需内置管理员 （栏） 组中的成员。 应该会有任何日常用户帐户管理员组除外内置管理员帐户中的域中，如果安全中所述[附录 d： 保护内置管理员帐户中 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

管理员是，默认情况下，大多数广告 DS 对象在各自域的所有者。 此组中的会员可能会要求所有权或拍摄对象的所有权的功能是必需的版本或灾难恢复方案中。 此外，DAs 和 EAs 文件沿用文件大量其权限和依靠其默认会员管理员组中的权限。 不应修改默认组嵌套特权组 Active Directory 中，并应安全每个域管理员一组，述按照的分步说明进行操作。  

森林中的每个域中管理员组：  

1.  提供保护中所述，从管理员组，但内置的管理员帐户域，可能不删除的所有成员[附录 d： 保护内置管理员帐户中 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  应 Gpo 链接到华丽绚烂包含成员服务器和工作站每个域中的，在添加 DA 组在以下的用户权限**计算机配置 \ 策略 \windows \ 本地 Policies\ 用户权限分配**:  

    -   拒绝该计算机的访问从网络  

    -   作为批量作业拒绝登录  

    -   作为一项服务拒绝登录  

3.  域控制器森林中的每个域中，在管理员组应授予以下的用户权限：  

    -   访问网络从这台计算机  

    -   允许本地登录  

    -   允许远程桌面服务通过登录  

4.  应配置审核发送通知，如果属性或管理员组的会员进行任何更改。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>从管理员组中删除的所有成员的分步说明  

1.  在**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  若要删除的所有成员从管理员组中，执行以下步骤：  

    1.  双击**管理员**分组，并单击**成员**选项卡。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  选择组成员的上，单击**删除**，单击**是**，然后单击**确定**。  

3.  直到管理员组中的所有成员已都删除，请重复第 2 步。  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>安全管理员组 Active Directory 中的分步说明  

1.  在**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

3.  控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  在**新 GPO**对话框中，键入<GPO Name>，并单击**确定**(其中*组策略对象名称*是此 GPO 的名称)。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  在详细信息窗格中，右键单击** <GPO Name> **，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows \ 本地策略**，然后单击**用户权限分配**。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  配置的用户权限，以防止管理员组成员通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝到此计算机的访问，从网络**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  单击**确定**，并**确定**再次。  

8.  配置用户权限若要防止管理员组成员的作为批量作业登录通过执行以下操作：  

    1.  双击**拒绝作为批量作业登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  单击**确定**，并**确定**再次。  

9. 配置用户权限若要防止管理员组成员的作为一项服务登录通过执行以下操作：  

    1.  双击**拒绝服务作为登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  单击**确定**，并**确定**再次。  

10. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

11. 在**组策略管理**，链接到成员服务器和工作站华丽绚烂的组策略对象，通过执行以下操作：  

    1.  导航到<Forest>\Domains\\<Domain> (其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

    2.  右键单击 GPO 将应用于和单击的 OU**链接现有的组策略对象**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  选择刚刚创建的 GPO，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  创建包含工作站所有其他华丽绚烂的链接。  

    5.  创建包含成员服务器所有其他华丽绚烂的链接。  

        > [!IMPORTANT]  
        > 如果用于管理域控制器和 Active Directory 跳转服务器，请确保跳转服务器位于的 OU Gpo 未为此链接。  

        > [!NOTE]  
        > 中 Gpo 管理员组上实现限制时，Windows 将应用到计算机的本地管理员组，除了域管理员组成员的设置。 因此，在管理员组中实现限制时应小心。 登录本地或远程桌面服务通过登录，尽管管理员组成员禁止网络、 批量，并登录服务时，建议只要很难实现，不会限制。 阻止这些登录类型可以阻止合法的本地管理员组成员计算机管理。  
        >   
        > 下面的屏幕截图显示配置设置阻止不恰当使用内置的本地并域管理员帐户，除了不恰当使用的内置本地或域管理员组。 请注意，**拒绝登录通过远程桌面服务**用户右不包括管理员组中，因为其包括在此设置会阻止这些本地计算机管理员组成员的帐户登录。 如果计算机上的服务配置为在任何特权组在此部分中所述的环境中运行，实现这些设置会导致失败的应用程序和服务。 因此，所有此部分中的建议，您应该全面测试适用性设置您的环境中。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>为管理员组的用户授予权限的分步说明  

1.  在**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

3.  控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  在**新 GPO**对话框中，键入<GPO Name>，并单击**确定**(其中<GPO Name>是此 GPO 的名称)。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  在详细信息窗格中，右键单击** <GPO Name> **，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows \ 本地策略**，然后单击**用户权限分配**。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  配置用户权限，以便到访问域控制器管理员组成员通过网络通过执行以下操作：  

    1.  双击**到此计算机的网络的访问权限**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  单击**添加的用户或组**单击**浏览**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  单击**确定**，并**确定**再次。  

8.  配置用户权限，以允许管理员组成员登录本地通过执行以下操作：  

    1.  双击**允许本地登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击检查**名称**，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  单击**确定**，并**确定**再次。  

9. 配置用户权允许管理员组成员通过登录远程桌面服务通过执行以下操作：  

    1.  双击**允许远程桌面服务通过登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  单击**确定**，并**确定**再次。  

10. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

11. 在**组策略管理**，链接到域控制器的组策略对象，通过执行以下操作：  

    1.  导航到<Forest>\Domains\\<Domain> (其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

    2.  右键单击，然后单击 OU 域控制器**链接现有的组策略对象**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  选择刚刚创建的 GPO，然后单击**确定**。  

        ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝访问到此计算机的网络"GPO 设置  
从任何成员服务器或不受更改 （例如"跳转服务器"） 的工作站上，尝试访问成员服务器或工作站通过受更改的网络。 若要验证 GPO 设置，请尝试使用映射系统驱动器**网络使用**命令。  

1.  在使用本地组成员的管理员帐户登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**权限的命令提示符**，右键单击**权限的命令提示符**，然后单击**以管理员身份运行**打开提升了权限的命令提示符窗口。  

4.  当批准提升提示，单击**是**。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  在**权限的命令提示符**窗口中，键入**网络使用 \\\<Server Name>\c$**，其中<Server Name>是成员服务器或工作站在尝试访问通过网络的名称。  

6.  下面的屏幕截图显示应该会显示错误消息。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝作为登录批量作业"GPO 设置  
任何成员服务器或更改受影响的工作站上，从本地登录。  

###### <a name="create-a-batch-file"></a>创建批量文件  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在**记事本**，类型**目录 c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在**文件名**字段中，键入** <Filename>.bat** (其中<Filename>是批新文件的名称)。  

###### <a name="schedule-a-task"></a>计划任务  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在搜索框中，运行 Windows 8 计算机上中，键入计划任务中，然后单击计划任务。  

3.  单击**操作**，然后单击**创建任务**。  

4.  在**创建任务**对话框中，键入** <Task Name> ** (其中<Task Name>被新任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在**操作**字段中，选择**启动程序**。  

7.  在**程序/脚本**字段中，单击**浏览**，找到并选择在创建的批量文件**创建批量文件**部分，然后单击**打开**。  

8.  单击**确定**。  

9. 单击**常规**选项卡。  

10. 在**安全选项**字段中，单击**更改用户或组**。  

11. 键入是管理员组成员的帐户名称、 单击**检查名称**，然后单击**确定**。  

12. 选择**运行该用户是否已登录的 onor 不**和**不要将密码存储**。 此任务将只能访问本地计算机资源。  

13. 单击**确定**。  

14. 应该会显示一个对话框中，请求的用户帐户凭据运行任务。  

15. 输入密码后, 单击**确定**。  

16. 应该会显示类似于以下对话框。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝登录即服务"GPO 设置  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  在**作为登录**字段中，选择**此帐户**。  

7.  单击**浏览**、 键入是管理员组成员的帐户名称、 单击**检查名称**，然后单击**确定**。  

8.  在**密码**和**确认密码**字段中，键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**单击**重启**。  

11. 该服务重新启动后，应该会显示类似于以下对话框。  

    ![安全管理员组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到打印机的后台服务的更改  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  在**作为登录**字段中，单击**本地系统**帐户，然后单击**确定**。  
