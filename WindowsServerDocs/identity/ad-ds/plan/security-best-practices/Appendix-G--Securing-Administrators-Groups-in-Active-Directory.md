---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 附录 G-保护 Active Directory 中的管理员组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cdea04e211b1873ff51c4bc3dc9ff24e746ead69
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408645"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 G：保护 Active Directory 中的管理员组

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>附录 G：保护 Active Directory 中的管理员组  
与企业管理员（EA）和域管理员（DA）组一样，内置管理员（BA）组中的成员身份只应在生成或灾难恢复方案中需要。 只有域的内置管理员帐户例外，管理员组中不应有任何日常用户帐户，前提是该帐户已受保护，如 @no__t 0Appendix D：Active Directory @ no__t 中保护内置管理员帐户。  

默认情况下，管理员默认情况下是其各自域中的大多数 AD DS 对象的所有者。 在生成或灾难恢复方案中，可能需要此组中的成员资格或取得对象所有权的权限。 此外，DAs 和 EAs 通过其在 Administrators 组中的默认成员身份来继承其权限。 不应修改 Active Directory 中特权组的默认组嵌套，并且应按照以下分步说明中所述保护每个域的管理员组。  

对于林中每个域中的 Administrators 组：  

1.  删除 Administrators 组中的所有成员，并在域的内置管理员帐户可能例外的情况下执行此操作，前提是该帐户已受保护，如 @no__t 0Appendix D：Active Directory @ no__t 中保护内置管理员帐户。  

2.  在链接到包含每个域中的成员服务器和工作站的 Ou 的 Gpo 中，应将 BA 组添加到 "**计算机配置 \windows" \ 安全 \ 本地策略 \ 用户权限分配**：  

    -   拒绝从网络访问这台计算机  

    -   拒绝作为批处理作业登录  

    -   拒绝以服务身份登录  

3.  对于林中每个域中的域控制器 OU，应为 Administrators 组授予以下用户权限：  

    -   从网络访问此计算机  

    -   允许本地登录  

    -   允许通过远程桌面服务登录  

4.  如果对 Administrators 组的属性或成员身份进行任何修改，应将审核配置为发送警报。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>从 Administrators 组中删除所有成员的分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **Active Directory 用户和计算机**"。  

2.  若要从 Administrators 组中删除所有成员，请执行以下步骤：  

    1.  双击 "**管理员**" 组，然后单击 "**成员**" 选项卡。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  选择组成员，单击 "**删除**"，单击 **"是**"，然后单击 **"确定"** 。  

3.  重复步骤2，直到删除了 Administrators 组的所有成员。  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Active Directory 中的管理员组的安全分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 "**组策略管理**"。  

2.  在控制台树中，展开 "&lt;Forest @ no__t-1\Domains @ no__t-2 @ no__t-3Domain @ no__t，然后**组策略对象**（其中 &lt;Forest @ no__t-7 是林的名称，&lt;Domain @ no__t-9 是要用于的域的名称。设置组策略）。  

3.  在控制台树中，右键单击**组策略对象**"，然后单击"**新建**"。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  在 "**新建 GPO** " 对话框中，键入 <GPO Name>，然后单击 **"确定"** （其中*gpo 名称*是此 GPO 的名称）。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  在详细信息窗格中，右键单击 **<GPO Name>** ，然后单击 "**编辑**"。  

6.  导航到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略**"，然后单击 "**用户权限分配**"。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  通过执行以下操作，配置用户权限以阻止 Administrators 组的成员访问网络上的成员服务器和工作站：  

    1.  双击 **"拒绝从网络访问此计算机"** ，并选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrators**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

8.  通过执行以下操作，将用户权限配置为阻止 Administrators 组的成员作为批处理作业登录：  

    1.  双击 "**拒绝作为批处理作业登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrators**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

9. 通过执行以下操作，将用户权限配置为阻止 Administrators 组的成员作为服务登录：  

    1.  双击 "**拒绝作为服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrators**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

10. 若要退出**组策略管理编辑器**，请单击 "**文件**"，然后单击 "**退出**"。  

11. 在**组策略管理**中，通过执行以下操作将 GPO 链接到成员服务器和工作站 ou：  

    1.  导航到 &lt;Forest @ no__t-1 > \Domains @ no__t-2 @ no__t-3Domain @ no__t-4 （其中 &lt;Forest @ no__t-6 是林的名称，&lt;Domain @ no__t-8 是要设置组策略的域的名称）。  

    2.  右键单击 GPO 将应用到的 OU，然后单击 "**链接现有 GPO**"。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  选择刚创建的 GPO，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  创建指向包含工作站的所有其他 Ou 的链接。  

    5.  创建指向包含成员服务器的所有其他 Ou 的链接。  

        > [!IMPORTANT]  
        > 如果使用跳转服务器来管理域控制器和 Active Directory，请确保 "跳转服务器" 位于此 Gpo 未链接到的 OU 中。  

        > [!NOTE]  
        > 当你对 Gpo 中的 Administrators 组实施限制时，Windows 除了域的 Administrators 组外，还会将设置应用于计算机的本地 Administrators 组的成员。 因此，在 Administrators 组中实施限制时应谨慎。 尽管在任何可行的情况下，都建议禁止 Administrators 组成员的网络、批处理和服务登录，但不要通过远程桌面服务限制本地登录名或登录名。 阻止这些登录类型会阻止本地管理员组的成员对计算机进行合法的管理。  
        >   
        > 下面的屏幕截图显示除了内置本地或域管理员组以外，还会阻止滥用内置本地和域管理员帐户的配置设置。 请注意，"**拒绝通过远程桌面服务**用户权限登录" 权限不包括 Administrators 组，因为在此设置中包含它也会阻止属于本地计算机的管理员组成员的帐户登录。 如果计算机上的服务被配置为在此部分中所述的任何特权组的上下文中运行，则实现这些设置可能会导致服务和应用程序失败。 因此，与本部分中的所有建议一样，您应该针对您的环境中的适用性全面测试设置。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>向管理员组授予用户权限的分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 "**组策略管理**"。  

2.  在控制台树中，展开 "<Forest> \ 域 @ no__t"，然后**组策略对象**（其中，@no__t 为林的名称，<Domain> 是要设置组策略的域的名称）。  

3.  在控制台树中，右键单击**组策略对象**"，然后单击"**新建**"。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  在 "**新建 GPO** " 对话框中，键入 <GPO Name>，然后单击 **"确定"** （其中 <GPO Name> 是此 GPO 的名称）。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  在详细信息窗格中，右键单击 **<GPO Name>** ，然后单击 "**编辑**"。  

6.  导航到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略**"，然后单击 "**用户权限分配**"。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  通过执行以下操作，将用户权限配置为允许 Administrators 组的成员通过网络访问域控制器：  

    1.  双击 **"从网络访问此计算机"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

8.  通过执行以下操作，将用户权限配置为允许 Administrators 组的成员本地登录：  

    1.  双击 "**允许在本地登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrators**，单击 "检查**名称**"，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

9. 通过执行以下操作，将用户权限配置为允许 Administrators 组的成员通过远程桌面服务登录：  

    1.  双击 "**允许通过远程桌面服务登录"** 并选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrators**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

10. 若要退出**组策略管理编辑器**，请单击 "**文件**"，然后单击 "**退出**"。  

11. 在**组策略管理**中，通过执行以下操作将 GPO 链接到域控制器 OU：  

    1.  导航到 <Forest> 域 @ no__t @ no__t-2 （其中，<Forest> 是林的名称，<Domain> 是要在其中设置组策略的域的名称）。  

    2.  右键单击 "域控制器" OU，然后单击 "**链接现有 GPO**"。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  选择刚创建的 GPO，然后单击 **"确定"** 。  

        ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证 "拒绝从网络访问此计算机" GPO 设置  
在不受 GPO 更改（如 "跳转服务器"）影响的任何成员服务器或工作站上，尝试通过受 GPO 更改影响的网络访问成员服务器或工作站。 若要验证 GPO 设置，请尝试使用**NET USE**命令映射系统驱动器。  

1.  使用作为 Administrators 组成员的帐户在本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 "**命令提示符**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**" 以打开提升的命令提示符。  

4.  当提示批准提升时，单击 **"是"** 。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  在 "**命令提示符**" 窗口中，键入**net use \\ @ no__t @ No__t-4Server name @ no__t-5\c $** ，其中 \<Server name @ no__t-7 是你尝试通过网络访问的成员服务器或工作站的名称。  

6.  以下屏幕截图显示应显示的错误消息。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证 "拒绝作为批处理作业登录" GPO 设置  
在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

###### <a name="create-a-batch-file"></a>创建批处理文件  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入**notepad**，然后单击 "**记事本**"。  

3.  在**记事本**中，键入**dir c：** 。  

4.  单击 "**文件**"，然后单击 "**另存为**"。  

5.  在 "**文件名**" 字段中，键入 **<Filename>** （其中，<Filename> 是新批处理文件的名称）。  

###### <a name="schedule-a-task"></a>计划任务  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**任务计划程序**"，然后单击 "**任务计划程序**"。  

    > [!NOTE]  
    > 在运行 Windows 8 的计算机上的 "搜索" 框中，键入 "计划任务"，然后单击 "计划任务"。  

3.  单击 "**操作**"，然后单击 "**创建任务**"。  

4.  在 "**创建任务**" 对话框中，键入 **<Task Name>** （其中 @no__t 为新任务的名称）。  

5.  单击 "**操作**" 选项卡，然后单击 "**新建**"。  

6.  在 "**操作**" 字段中，选择 "**启动程序**"。  

7.  在 "**程序/脚本**" 字段中，单击 "**浏览**"，找到并选择 "**创建批处理文件**" 部分中创建的批处理文件，然后单击 "**打开**"。  

8.  单击 **“确定”** 。  

9. 单击“常规”选项卡。  

10. 在 "**安全选项**" 字段中，单击 "**更改用户或组**"。  

11. 键入作为 Administrators 组成员的帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

12. 选择 **"运行" （无论用户是否已记录 onor** ）而不**存储密码**。 该任务将仅有权访问本地计算机资源。  

13. 单击 **“确定”** 。  

14. 应显示一个对话框，请求用户帐户凭据以运行任务。  

15. 输入密码后，单击 **"确定"** 。  

16. 应出现如下所示的对话框。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证 "拒绝作为服务登录" GPO 设置  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**" 字段中，选择 "**此帐户**"。  

7.  单击 "**浏览**"，键入作为 Administrators 组成员的帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

8.  在 "**密码**" 和 "**确认密码**" 字段中，键入所选帐户的密码，然后单击 **"确定"** 。  

9. 再单击 **"确定"** 三次。  

10. 右键单击 "**打印后台处理程序**"，然后单击 "**重新启动**"。  

11. 重新启动该服务时，应显示类似于以下内容的对话框。  

    ![安全管理组](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原对打印机后台处理程序服务所做的更改  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**" 字段中，单击 "**本地系统**帐户"，然后单击 **"确定"** 。  
