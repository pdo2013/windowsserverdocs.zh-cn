---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: "附录 H 的本地管理员帐户和组保护"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 h：保护措施的本地管理员帐户和组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 h：保护措施的本地管理员帐户和组  
所有 Windows 版本的当前是在主要支持，默认情况下，这可以使该帐户 pass 哈希和其他凭据盗窃攻击不可用的本地管理员帐户处于禁用都状态。 但是，在环境中包含的旧操作系统或在其中已启用的本地管理员帐户，这些帐户可用如上文所述跨成员服务器和工作站传播泄露。 每个的本地管理员帐户并组应遵循的分步中所述安全。  

有关保护内置管理员 （栏） 组中的注意事项的详细信息，请参阅[实现最低权限管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。  

#### <a name="controls-for-local-administrator-accounts"></a>控件的本地管理员帐户  
对于你森林中的每个域中的本地管理员帐户，你应配置以下设置：  

-   配置 Gpo 将在已加入域的系统上的域的管理员帐户使用限制  
    -   在你创建并链接到工作站和成员服务器华丽绚烂每个域中的一个或多个 Gpo 的管理员帐户添加到在以下的用户权限**计算机配置 \ 策略 \windows \ 设置本地 Settings\User 权限分配**:  

        -   拒绝该计算机的访问从网络  

        -   作为批量作业拒绝登录  

        -   作为一项服务拒绝登录  

        -   远程桌面服务通过拒绝登录  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>若要在安全的本地管理员组的分步说明  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>配置 Gpo 限制上已加入域的系统管理员帐户  

1.  在**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

3.  控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  在**新 GPO**对话框中，键入** <GPO Name> **，并单击**确定**(其中<GPO Name>是此 GPO 的名称)。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  在详细信息窗格中，右键单击** <GPO Name> **，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows \ 本地策略**，然后单击**用户权限分配**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  配置的用户权限，以防止的本地管理员帐户通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝到此计算机的访问，从网络**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**、 键入的本地管理员帐户的用户名，然后单击**确定**。 将此用户名**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  单击确定。  

        > [!IMPORTANT]  
        > 当你添加到这些设置的管理员帐户时，你指定无论你通过您添加帐户的标签配置的本地管理员帐户或域管理员帐户。 例如，若要添加到这些 TAILSPINTOYS 域的管理员帐户拒绝权利，您会浏览的管理员帐户 TAILSPINTOYS 域中，它将显示为 TAILSPINTOYS\Administrator。 如果你键入**管理员**在这些用户版权设置组策略编辑器中，你将限制 GPO 应用于，每台计算机上的本地管理员帐户，如上文所述。  

8.  配置用户权限若要防止的本地管理员帐户作为批量作业登录通过执行以下操作：  

    1.  双击**拒绝作为批量作业登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**、 键入的本地管理员帐户的用户名，然后单击**确定**。 将此用户名**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  单击**确定**。  

        > [!IMPORTANT]  
        > 当你添加到这些设置的管理员帐户时，你指定无论你通过您添加帐户的标签配置的本地管理员帐户或域管理员帐户。 例如，若要添加到这些 TAILSPINTOYS 域的管理员帐户拒绝权利，您会浏览的管理员帐户 TAILSPINTOYS 域中，它将显示为 TAILSPINTOYS\Administrator。 如果你键入**管理员**在这些用户版权设置组策略编辑器中，你将限制 GPO 应用于，每台计算机上的本地管理员帐户，如上文所述。  

9. 配置用户权限若要防止的本地管理员帐户即服务登录通过执行以下操作：  

    1.  双击**拒绝服务作为登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**、 键入的本地管理员帐户的用户名，然后单击**确定**。 将此用户名**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  单击**确定**。  

        > [!IMPORTANT]  
        > 当你添加到这些设置的管理员帐户时，你指定无论你通过您添加帐户的标签配置的本地管理员帐户或域管理员帐户。 例如，若要添加到这些 TAILSPINTOYS 域的管理员帐户拒绝权利，您会浏览的管理员帐户 TAILSPINTOYS 域中，它将显示为 TAILSPINTOYS\Administrator。 如果你键入**管理员**在这些用户版权设置组策略编辑器中，你将限制 GPO 应用于，每台计算机上的本地管理员帐户，如上文所述。  

10. 配置用户权访问成员服务器和工作站远程桌面服务通过执行以下阻止的本地管理员帐户：  

    1.  双击**拒绝登录通过远程桌面服务**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**、 键入的本地管理员帐户的用户名，然后单击**确定**。 将此用户名**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  单击**确定**。  

        > [!IMPORTANT]  
        > 当你添加到这些设置的管理员帐户时，你指定无论你通过您添加帐户的标签配置的本地管理员帐户或域管理员帐户。 例如，若要添加到这些 TAILSPINTOYS 域的管理员帐户拒绝权利，您会浏览的管理员帐户 TAILSPINTOYS 域中，它将显示为 TAILSPINTOYS\Administrator。 如果你键入**管理员**在这些用户版权设置组策略编辑器中，你将限制 GPO 应用于，每台计算机上的本地管理员帐户，如上文所述。  

11. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

12. 在**组策略管理**，链接到成员服务器和工作站华丽绚烂的组策略对象，通过执行以下操作：  

    1.  导航到<Forest>\Domains\\<Domain> (其中<Forest>是森林的名称和<Domain>是你想要在组策略设置的域的名称)。  

    2.  右键单击 GPO 将应用于和单击的 OU**链接现有的组策略对象**。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  选择你创建 GPO，然后单击**确定**。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  创建包含工作站所有其他华丽绚烂的链接。  

    5.  创建包含成员服务器所有其他华丽绚烂的链接。  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝访问到此计算机的网络"GPO 设置  

从任何成员服务器或不受更改 （例如跳转服务器） 的工作站上，尝试访问成员服务器或工作站通过受更改的网络。 若要验证 GPO 设置，请尝试使用映射系统驱动器**网络使用**命令。  

1.  登录到的任何成员服务器或工作站不受影响的更改。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**权限的命令提示符**，右键单击**权限的命令提示符**，然后单击**以管理员身份运行**打开提升了权限的命令提示符窗口。  

4.  当批准提升提示，单击**是**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  在**权限的命令提示符**窗口中，键入**网络使用 \\\<Server Name>\c$ /user:<Server Name>\Administrator**，其中<Server Name>是成员服务器或工作站在尝试访问通过网络的名称。  

    > [!NOTE]  
    > 本地管理员凭据必须是同一个系统尝试通过网络的访问。  

6.  下面的屏幕截图显示应该会显示错误消息。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝作为登录批量作业"GPO 设置  
任何成员服务器或更改受影响的工作站上，从本地登录。  

###### <a name="create-a-batch-file"></a>创建批量文件  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在**记事本**，类型**目录 c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在**文件名**框中，键入** <Filename>.bat** (其中<Filename>是批新文件的名称)。  

###### <a name="schedule-a-task"></a>计划任务  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入任务计划程序中，单击**任务计划程序**。  

    > [!NOTE]  
    > 在计算机上运行 Windows 8，请在**搜索**框中，键入**计划任务**，然后单击**计划任务**。  

3.  单击**操作**，然后单击**创建任务**。  

4.  在**创建任务**对话框中，键入** <Task Name> ** (其中<Task Name>被新任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在**操作**字段中，单击**启动程序**。  

7.  在**程序/脚本**字段中，单击**浏览**，找到并选择在创建的批量文件**创建批量文件**部分，然后单击**打开**。  

8.  单击**确定**。  

9. 单击**常规**选项卡。  

10. 在**安全选项**字段中，单击**更改用户或组**。  

11. 键入的系统的本地管理员帐户名称、 单击**检查名称**，然后单击**确定**。  

12. 选择**运行或无法用户是否登录**和**不要将密码存储**。 此任务将只能访问本地计算机资源。  

13. 单击**确定**。  

14. 应该会显示一个对话框中，请求的用户帐户凭据运行任务。  

15. 输入凭据后, 单击**确定**。  

16. 应该会显示类似于以下对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝登录即服务"GPO 设置  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  在**作为登录**字段中，单击**此帐户**。  

7.  单击**浏览**键入系统的本地管理员帐户，请单击**检查名称**，然后单击**确定**。  

8.  在**密码**和**确认密码**字段中，键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**单击**重启**。  

11. 该服务重新启动后，应该会显示类似于以下对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到打印机的后台服务的更改  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  在**作为登录**： 字段中，选择**本地 Systemaccount**，然后单击**确定**。  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝登录远程桌面服务通过"GPO 设置  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在**计算机**字段中，键入你想要连接到和单击的计算机名称**连接**。 （你可以键入 IP 地址代替计算机名称）。  

4.  出现提示时，系统的本地提供凭据**管理员**帐户。  

5.  应该会显示类似于以下对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
