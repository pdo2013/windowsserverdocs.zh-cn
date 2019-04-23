---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 附录 G-保护 Active Directory 中的管理员组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882708"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 g:保护 Active Directory 中的管理员组

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 g:保护 Active Directory 中的管理员组  
使用 Enterprise Admins (EA) 和域管理员 (DA) 组情况一样，应仅在生成或灾难恢复方案中需要中内置的管理员 (BA) 组的成员身份。 应该有没有日常用户帐户在域中的内置 Administrator 帐户除外 Administrators 组中如果保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

管理员是，默认情况下，大部分在其各自的域中的 AD DS 对象的所有者。 可能的所有权或取得对象的所有权的功能是必需的生成或灾难恢复方案中需要此组中的成员身份。 此外，DAs 和 EAs 继承其权限和权限按其默认成员身份的管理员组中的数字。 不应修改默认的 Active Directory 中的特权组嵌套的组，并应保护每个域的管理员组，请按照分步说明中所述。  

每个林中的域中的 Administrators 组：  

1.  从管理员组，但可能在域中的内置管理员帐户中删除所有成员提供保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  应将 Gpo 链接到包含成员服务器和工作站中的每个域的 Ou，DA 组添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \ Policies\ 用户权限分配**:  

    -   拒绝从网络访问这台计算机  

    -   拒绝作为批处理作业登录  

    -   拒绝以服务身份登录  

3.  在域控制器的林中每个域中 OU 的管理员组应授予以下用户权限：  

    -   从网络访问此计算机  

    -   允许本地登录  

    -   允许通过远程桌面服务登录  

4.  应配置审核来发送警报，如果对属性或 Administrators 组的成员身份进行任何修改。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>从管理员组中删除所有成员的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  若要从管理员组中删除所有成员，请执行以下步骤：  

    1.  双击**管理员**组，并单击**成员**选项卡。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  选择组的成员，单击**删除**，单击**是**，然后单击**确定**。  

3.  重复步骤 2，直到已删除管理员组的所有成员。  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>保护 Active Directory 中的管理员组的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  在控制台树中，展开&lt;林&gt;\Domains\\&lt;域&gt;，，然后**组策略对象**(其中&lt;林&gt;是在林中的名称和&lt;域&gt;是你想要将组策略设置的域的名称)。  

3.  在控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  在**新的 GPO**对话框中，键入<GPO Name>，然后单击**确定**(其中*GPO 名称*是此 GPO 的名称)。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  在细节窗格中，右键单击**<GPO Name>**，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略**，然后单击**用户权限分配**。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  配置用户权限来防止管理员组的成员通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝从网络访问这台计算机**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  单击**确定**，并**确定**试。  

8.  配置用户权限以防止管理员组的成员作为批处理作业登录通过执行以下操作：  

    1.  双击**拒绝作为批处理作业登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  单击**确定**，并**确定**试。  

9. 配置用户权限以防止管理员组的成员作为服务登录通过执行以下操作：  

    1.  双击**拒绝作为服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  单击**确定**，并**确定**试。  

10. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

11. 在中**组策略管理**，将 GPO 链接到的成员服务器和工作站的 Ou，执行以下操作：  

    1.  导航到&lt;林&gt;> \Domains\\&lt;域&gt;(其中&lt;林&gt;是林的名称和&lt;域&gt;名称域的你想要将组策略设置）。  

    2.  右键单击该 GPO 将应用于和单击的 OU**链接现有 GPO**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  选择刚创建的 GPO，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  创建链接到包含工作站的所有其他 Ou。  

    5.  创建链接到包含成员服务器的所有其他 Ou。  

        > [!IMPORTANT]  
        > 如果跳转服务器用于管理域控制器和 Active Directory，请确保跳转服务器位于 Gpo 未链接到此 OU 中。  

        > [!NOTE]  
        > 在 Gpo 中的管理员组实现限制时，Windows 将设置应用于计算机的本地 Administrators 组的域管理员组的成员。 因此，在管理员组中实现限制时应小心。 尽管 Administrators 组的成员禁止网络、 批处理和登录服务的建议是可行来实现的任何位置，但不限制本地登录或通过远程桌面服务登录。 阻止这些登录类型可能会阻止合法管理的计算机的本地管理员组的成员。  
        >   
        > 下面的屏幕截图显示了配置设置的阻止不恰当使用内置的本地和域管理员帐户，除了不恰当使用的内置本地或域管理员组。 请注意，**拒绝通过远程桌面服务登录**用户权限不包括 Administrators 组中，因为其包括在此设置还会阻止这些成员的本地计算机的帐户登录管理员组。 如果计算机上的服务配置为在本部分中所述的特权任何的组上下文中运行，实现这些设置会导致服务和应用程序失败。 因此，与所有的此部分中的建议，一样，应全面测试适用性的设置您的环境中。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>授予用户权限到管理员组的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  在控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是林的名称和<Domain>是，您希望对域的名称设置组策略）。  

3.  在控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  在中**新的 GPO**对话框中，键入<GPO Name>，然后单击**确定**(其中<GPO Name>是此 GPO 的名称)。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  在细节窗格中，右键单击**<GPO Name>**，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略**，然后单击**用户权限分配**。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  配置用户权限，允许通过网络访问域控制器管理员组的成员，通过执行以下操作：  

    1.  双击**从网络访问这台计算机**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  单击**添加用户或组**然后单击**浏览**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  单击**确定**，并**确定**试。  

8.  配置用户权限，允许 Administrators 组的成员登录本地通过执行以下操作：  

    1.  双击**允许本地登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击复选标记**名称**，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  单击**确定**，并**确定**试。  

9. 配置用户权限，允许 Administrators 组的成员登录通过远程桌面服务通过执行以下操作：  

    1.  双击**允许通过远程桌面服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  单击**确定**，并**确定**试。  

10. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

11. 在中**组策略管理**，将 GPO 链接到域控制器 OU，执行以下操作：  

    1.  导航到<Forest>\Domains\\ <Domain> (其中<Forest>是林的名称和<Domain>是你想要将组策略设置的域的名称)。  

    2.  右键单击域控制器 OU，然后单击**链接现有 GPO**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  选择刚创建的 GPO，然后单击**确定**。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝从网络访问这台计算机"GPO 设置  
从任何成员服务器或不受 GPO 更改 （例如"跳转服务器"） 的工作站，尝试通过 GPO 更改的会影响网络访问的成员服务器或工作站。 若要验证的 GPO 设置，请尝试通过使用系统驱动器映射**NET USE**命令。  

1.  在使用本地管理员组的成员帐户登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在**搜索**框中，键入**命令提示符**，右键单击**命令提示符下**，然后单击**以管理员身份运行**若要打开提升命令提示符。  

4.  当系统提示您批准提升，请单击**是**。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  在中**命令提示符**窗口中，键入**net 使用\\ \\\<服务器名称\>\c$**，其中\<服务器名称\>是成员服务器或要通过网络访问的工作站的名称。  

6.  下面的屏幕截图显示了应出现的错误消息。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝登录作为批处理作业"GPO 设置  
从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

###### <a name="create-a-batch-file"></a>创建一个批处理文件  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在中**记事本**，类型**dir c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在中**文件名**字段中，键入 **<Filename>.bat** (其中<Filename>是新的批处理文件的名称)。  

###### <a name="schedule-a-task"></a>计划的任务  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在搜索框中，运行 Windows 8 的计算机上键入计划任务，然后单击计划任务。  

3.  单击**操作**，然后单击**创建任务**。  

4.  在中**创建任务**对话框中，键入**<Task Name>** (其中<Task Name>是新的任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在中**操作**字段中，选择**启动程序**。  

7.  在中**程序/脚本**字段中，单击**浏览**，找到并选择中创建的批处理文件**创建一个批处理文件**部分，然后单击**打开**.  

8.  单击 **“确定”**。  

9. 单击“常规”选项卡。  

10. 在中**安全选项**字段中，单击**更改用户或组**。  

11. 键入该帐户是 Administrators 组的成员的名称，单击**检查名称**，然后单击**确定**。  

12. 选择**运行是否该用户不是记录的 onor**并**不存储密码**。 任务将仅具有本地计算机资源的访问权限。  

13. 单击 **“确定”**。  

14. 应出现一个对话框，请求的用户帐户凭据运行任务。  

15. 输入密码后，单击**确定**。  

16. 应显示类似于以下的对话框。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝作为登录服务"GPO 设置  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  在中**作为登录**字段中，选择**此帐户**。  

7.  单击**浏览**，键入该帐户是 Administrators 组的成员的名称，单击**检查名称**，然后单击**确定**。  

8.  在中**密码**并**确认密码**字段中，键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**然后单击**重新启动**。  

11. 重新启动该服务时，应显示类似于以下的对话框。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到的打印机后台处理程序服务所做的更改  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  在中**作为登录**字段中，单击**本地系统**帐户，然后单击**确定**。  
