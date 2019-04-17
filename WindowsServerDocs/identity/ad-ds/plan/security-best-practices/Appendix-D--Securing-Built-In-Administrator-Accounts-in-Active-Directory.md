---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: "附录 D-保护 Active Directory 中内置管理员帐户"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 d：保护 Active Directory 中的内置管理员帐户

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 d：保护 Active Directory 中的内置管理员帐户  
在每个 Active Directory 在域中，管理员帐户创建的域的创建过程。 此帐户默认情况下在域中，域管理和管理员组的成员，但森林根域域，则该帐户是否还企业管理员组成员。

使用管理员帐户某个域，应该仅为初始版本活动，甚至，灾难恢复方案保留。 若要确保管理员帐户可用于在可以使用其他帐户不会影响修复，不应更改默认会员的树林中的任何域中的管理员帐户。 相反，你应该安全森林中的每个域中的管理员帐户，在以下部分中所述，并详细的分步说明进行操作，请按照。 

> [!NOTE]
> 本指南，用来禁用该帐户的建议。 作为森林恢复白色纸删除了这将使用默认的管理员帐户。 原因是，这是允许没有全球目录服务器登录仅帐户。


#### <a name="controls-for-built-in-administrator-accounts"></a>内置管理员帐户的控件  
对于你森林中的每个域中内置的管理员帐户，你应配置以下设置：  

-   启用**敏感帐户，不能委派**标志的帐户。  

-   启用**智能卡，才能进行交互登录**标志的帐户。  

-   配置 Gpo 将使用域加入系统上限制的管理员帐户：  

    -   在你创建并链接到工作站和成员服务器华丽绚烂每个域中的一个或多个 Gpo，每个域管理员帐户添加到在以下的用户权限**计算机配置 \ 策略 \windows \ 设置本地 Settings\User 权限分配**:  

        -   拒绝该计算机的访问从网络  

        -   作为批量作业拒绝登录  

        -   作为一项服务拒绝登录  

        -   远程桌面服务通过拒绝登录  

> [!NOTE]  
> 当你添加到此设置的帐户时，你必须指定配置本地管理员帐户或域管理员帐户。 例如，若要添加到这些 NWTRADERS 域的管理员帐户拒绝权限，你必须为 NWTRADERS\Administrator 或通过管理员帐户浏览 NWTRADERS 域键入的帐户。 如果你在这些用户版权设置组策略编辑器中键入"管理员"，你将限制的应用 GPO 每台计算机上本地管理员帐户。
>   
> 我们建议域基于管理员帐户的方式相同限制成员服务器和工作站上的本地管理员帐户。 因此，你应通常添加森林中的每个域的管理员帐户和本地计算机管理员帐户对这些用户版权设置。 下面的屏幕截图显示配置这些用户权限本地管理员帐户并执行不需要用于这些帐户的登录的域的管理员帐户阻止它的一个示例。  


![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   配置 Gpo 限制域控制器上的管理员帐户  
    -   森林中的每个域，默认的域控制器 GPO 或策略链接到应修改 OU 每个域管理员帐户添加到在以下的用户权限域控制器**计算机配置 \ 策略 \windows \ 设置本地 Settings\User 权限分配**:   
        -   拒绝该计算机的访问从网络  

        -   作为批量作业拒绝登录  

        -   作为一项服务拒绝登录  

        -   远程桌面服务通过拒绝登录  

> [!NOTE]  
> 这些设置将确保的域内置的管理员帐户不能用于连接到域控制器，尽管帐户，如果启用，可以在本地登录到域控制器。 由于此帐户应仅启用并且用于灾难恢复方案中，预计将提供，物理访问至少一个域控制器或，可以使用其他帐户具有权限才能远程访问域控制器。  

-   配置审核的管理员帐户  

    有安全的每个域管理员帐户并将其禁用时，应该配置审核监控对帐户的更改。 如果已启用该帐户、 其密码重置，或任何其他修改对该帐户，警报应该会发送给或用户团队负责管理 Active directory，除了在你的组织的事件响应团队。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>分步说明安全 Active Directory 中内置管理员帐户  

1.  在**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  若要防止充分利用委派使用该帐户凭据其他系统上的攻击，请执行以下步骤：  

    1.  右键单击**管理员**帐户，然后单击**属性**。  

    2.  单击**帐户**选项卡。  

    3.  下**用户选项**、 选择**敏感帐户，不能委派**添加标志，述下面的屏幕截图，然后单击**确定**。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要启用**智能卡，才能进行交互登录**标记的帐户，请执行以下步骤：  

    1.  右键单击**管理员**帐户，然后选择**属性**。  

    2.  单击**帐户**选项卡。  

    3.  下**帐户**选项中选择**智能卡，才能进行交互登录**添加标志，述下面的屏幕截图，然后单击**确定**。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>配置 Gpo 限制域级别的管理员帐户  

> [!WARNING]  
> 永远不会将此 GPO 链接域级别，因为它可能会使不可用，即使在灾难恢复方案中内置的管理员帐户。  

1.  在**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(位置<Forest>是森林的名称和<Domain>是你想要创建组策略的域的名称)。  

3.  控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  在**新 GPO**对话框中，键入<GPO Name>，并单击**确定**(其中<GPO Name>是此 GPO 的名称) 下面的屏幕截图中所述。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在详细信息窗格中，右键单击<GPO Name>，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows \ 本地策略**，然后单击**用户权限分配**。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  配置用户的权限，以防止管理员帐户通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝到此计算机的访问，从网络**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式下面的屏幕截图中所述。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  单击**确定**，并**确定**再次。  

8.  配置用户权限若要防止管理员帐户作为批量作业登录通过执行以下操作：  

    1.  双击**拒绝作为批量作业登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式下面的屏幕截图中所述。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  单击**确定**，并**确定**再次。  

9. 配置用户权限若要防止管理员帐户即服务登录通过执行以下操作：  

    1.  双击**拒绝服务作为登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式下面的屏幕截图中所述。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  单击**确定**，并**确定**再次。  

10. 配置用户权限若要防止栏帐户访问成员服务器和通过远程桌面服务工作站通过执行以下操作：  

    1.  双击**拒绝登录通过远程桌面服务**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式下面的屏幕截图中所述。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  单击**确定**，并**确定**再次。  

11. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

12. 在**组策略管理**，链接到成员服务器和工作站华丽绚烂的组策略对象，通过执行以下操作：  

    1.  导航到<Forest>\Domains\\<Domain> (其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

    2.  右键单击 GPO 将应用于和单击的 OU**链接现有的组策略对象**。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  选择你创建 GPO，然后单击**确定**。  

        ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  创建包含工作站所有其他华丽绚烂的链接。  

    5.  创建包含成员服务器所有其他华丽绚烂的链接。  

> [!IMPORTANT]  
> 当你添加到这些设置的管理员帐户时，你指定无论你通过您添加帐户的标签配置的本地管理员帐户或域管理员帐户。 例如，若要添加到这些 TAILSPINTOYS 域的管理员帐户拒绝权利，您会浏览的管理员帐户 TAILSPINTOYS 域中，它将显示为 TAILSPINTOYS\Administrator。 如果在这些用户版权设置组策略编辑器中键入"管理员"，你将限制 GPO 应用于，每台计算机上的本地管理员帐户，如上文所述。  

#### <a name="verification-steps"></a>验证步骤  
下面列出的验证步骤是专用于 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>验证"智能卡登录必须，交互式"帐户选项  

1.  从任何成员服务器或更改受影响的工作站上，尝试登录交互域通过使用内置的域的管理员帐户。 在尝试登录之后, 应该会显示类似于以下对话框。  

![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>验证"帐户已禁用"帐户选项  

1.  从任何成员服务器或更改受影响的工作站上，尝试登录交互域通过使用内置的域的管理员帐户。 在尝试登录之后, 应该会显示类似于以下对话框。  

![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝访问到此计算机的网络"GPO 设置  
从任何成员服务器或不受更改 （例如跳转服务器） 的工作站上，尝试访问成员服务器或工作站通过受更改的网络。 若要验证 GPO 设置，请尝试使用映射系统驱动器**网络使用**命令，请执行以下步骤：  

1.  登录到使用内置的域的管理员帐户的域。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**权限的命令提示符**，右键单击**权限的命令提示符**，然后单击**以管理员身份运行**打开提升了权限的命令提示符窗口。  

4.  当批准提升提示，单击**是**。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在**权限的命令提示符**窗口中，键入**网络使用 \\\<Server Name>\c$**，其中<Server Name>是成员服务器或工作站你尝试访问通过网络的名称。  

6.  下面的屏幕截图显示应该会显示错误消息。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝作为登录批量作业"GPO 设置  

任何成员服务器或更改受影响的工作站上，从本地登录。  

###### <a name="create-a-batch-file"></a>创建批量文件  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在**记事本**，类型**目录 c:**。  

4.  单击**文件**单击**另存为**。  

5.  在**文件名**字段中，键入** <Filename>.bat** (其中<Filename>是批新文件的名称)。  

###### <a name="schedule-a-task"></a>计划任务  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在搜索框中，运行 Windows 8 计算机上键入**计划任务**，并单击**计划任务**。  

3.  在**任务计划程序**，单击**操作**，然后单击**创建任务**。  

4.  在**创建任务**对话框中，键入** <Task Name> ** (其中** <Task Name> **被新任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  下**操作：**、 选择**启动程序**。  

7.  下**程序/脚本：**，单击**浏览**，找到并选择"创建批量文件"部分中，在创建的批量文件，并单击**打开**。  

8.  单击**确定**。  

9. 单击**常规**选项卡。  

10. 下**安全**选项，请单击**更改用户或组**。  

11. 在域级别单击键入栏帐户的名称**检查名称**，然后单击**确定**。  

12. 选择**运行或无法用户是否登录**和**不要将密码存储**。 此任务将只能访问本地计算机资源。  

13. 单击**确定**。  

14. 应该会显示一个对话框中，请求的用户帐户凭据运行任务。  

15. 输入凭据后, 单击**确定**。  

16. 应该会显示类似于以下对话框。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝登录即服务"GPO 设置  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  下**作为登录：**、 选择**此帐户**。  

7.  单击**浏览**、 域级别键入栏帐户的名称、 单击**检查名称**，然后单击**确定**。  

8.  下**密码：**和**确认密码：**、 键入管理员帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序的服务**选择**重启**。  

11. 该服务重新启动后，应该会显示类似于以下对话框。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到打印机的后台服务的更改  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  下**作为登录：**、 选择**本地系统**帐户，然后单击**确定**。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝登录远程桌面服务通过"GPO 设置

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在**计算机**字段中，键入你想要连接到和单击的计算机名称**连接**。 （你可以键入 IP 地址代替计算机名称）。  

4.  看到提示后，提供凭据域级别栏帐户的名称。  

5.  应该会显示类似于以下对话框。  

    ![保护内置的管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
