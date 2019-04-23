---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 附录 H-保护本地管理员帐户和组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 71eea3f623968172076708dbea34d5bbf4a07684
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858688"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 h:保护本地管理员帐户和组

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 h:保护本地管理员帐户和组  
在所有版本的当前处于主流支持的 Windows，默认情况下，这使得该帐户无法用于传递哈希和其他凭据盗窃攻击禁用本地管理员帐户。 但是，在环境中包含的旧操作系统或在其中启用本地管理员帐户，这些帐户可以使用如前面所述在成员服务器和工作站之间传播泄漏。 每个本地管理员帐户和组应遵循的分步说明中所述进行保护。  

有关保护内置管理员 (BA) 组中的注意事项的详细信息，请参阅[实现最小特权的管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。  

#### <a name="controls-for-local-administrator-accounts"></a>控件的本地管理员帐户  
在您的林中每个域中的本地管理员帐户，应配置下列设置：  

-   配置 Gpo 以限制在已加入域的系统上的域的管理员帐户的使用  
    -   在创建并链接到 Ou 中的每个域的工作站和成员服务器的一个或多个 Gpo，将管理员帐户添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \ Policies\用户权限分配**:  

        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>保护本地管理员组的分步说明  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>配置 Gpo 以限制在已加入域的系统上的管理员帐户  

1.  在中**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  在控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是林的名称和<Domain>是，您希望对域的名称设置组策略）。  

3.  在控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  在中**新的 GPO**对话框中，键入**<GPO Name>**，然后单击**确定**(其中<GPO Name>是此 GPO 的名称)。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  在细节窗格中，右键单击**<GPO Name>**，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略**，然后单击**用户权限分配**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  配置用户权限来防止本地管理员帐户通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝从网络访问这台计算机**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，键入本地 Administrator 帐户的用户名，然后单击**确定**。 此用户名将是**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  单击“确定”。  

        > [!IMPORTANT]  
        > 将管理员帐户添加到这些设置，当您指定是否要配置的本地管理员帐户或域管理员帐户的帐户的标签。 例如，若要添加到这些 TAILSPINTOYS 域管理员帐户拒绝权限，您应浏览到 TAILSPINTOYS 域的管理员帐户中，这将显示为 TAILSPINTOYS\Administrator。 如果键入**管理员**在这些用户权限设置组策略对象编辑器中，您将限制 GPO 应用于每台计算机上的本地管理员帐户，如前面所述。  

8.  配置用户权限以防止本地管理员帐户作为批处理作业登录通过执行以下操作：  

    1.  双击**拒绝作为批处理作业登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，键入本地 Administrator 帐户的用户名，然后单击**确定**。 此用户名将是**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  单击 **“确定”**。  

        > [!IMPORTANT]  
        > 将管理员帐户添加到这些设置，您可以指定是否要配置本地 Administrator 帐户或域管理员帐户的帐户的标签。 例如，若要添加到这些 TAILSPINTOYS 域管理员帐户拒绝权限，您应浏览到 TAILSPINTOYS 域的管理员帐户中，这将显示为 TAILSPINTOYS\Administrator。 如果键入**管理员**在这些用户权限设置组策略对象编辑器中，您将限制 GPO 应用于每台计算机上的本地管理员帐户，如前面所述。  

9. 配置用户权限以防止本地管理员帐户作为服务登录通过执行以下操作：  

    1.  双击**拒绝作为服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，键入本地 Administrator 帐户的用户名，然后单击**确定**。 此用户名将是**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  单击 **“确定”**。  

        > [!IMPORTANT]  
        > 将管理员帐户添加到这些设置，您可以指定是否要配置本地 Administrator 帐户或域管理员帐户的帐户的标签。 例如，若要添加到这些 TAILSPINTOYS 域管理员帐户拒绝权限，您应浏览到 TAILSPINTOYS 域的管理员帐户中，这将显示为 TAILSPINTOYS\Administrator。 如果键入**管理员**在这些用户权限设置组策略对象编辑器中，您将限制 GPO 应用于每台计算机上的本地管理员帐户，如前面所述。  

10. 配置用户权限来防止本地管理员帐户访问成员服务器和工作站通过远程桌面服务通过执行以下操作：  

    1.  双击**拒绝通过远程桌面服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，键入本地 Administrator 帐户的用户名，然后单击**确定**。 此用户名将是**管理员**，安装 Windows 时的默认行为。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  单击 **“确定”**。  

        > [!IMPORTANT]  
        > 将管理员帐户添加到这些设置，您可以指定是否要配置本地 Administrator 帐户或域管理员帐户的帐户的标签。 例如，若要添加到这些 TAILSPINTOYS 域管理员帐户拒绝权限，您应浏览到 TAILSPINTOYS 域的管理员帐户中，这将显示为 TAILSPINTOYS\Administrator。 如果键入**管理员**在这些用户权限设置组策略对象编辑器中，您将限制 GPO 应用于每台计算机上的本地管理员帐户，如前面所述。  

11. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

12. 在中**组策略管理**，将 GPO 链接到的成员服务器和工作站的 Ou，执行以下操作：  

    1.  导航到<Forest>\Domains\\ <Domain> (其中<Forest>是林的名称和<Domain>是你想要将组策略设置的域的名称)。  

    2.  右键单击该 GPO 将应用于和单击的 OU**链接现有 GPO**。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  选择你创建的 GPO，然后单击**确定**。  

        ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  创建链接到包含工作站的所有其他 Ou。  

    5.  创建链接到包含成员服务器的所有其他 Ou。  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝从网络访问这台计算机"GPO 设置  

从任何成员服务器或不受 GPO 更改 （例如跳转服务器） 的工作站，尝试通过 GPO 更改的会影响网络访问的成员服务器或工作站。 若要验证的 GPO 设置，请尝试通过使用系统驱动器映射**NET USE**命令。  

1.  在本地登录到任何成员服务器或工作站的不受 GPO 更改。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在**搜索**框中，键入**命令提示符**，右键单击**命令提示符下**，然后单击**以管理员身份运行**若要打开提升命令提示符。  

4.  当系统提示您批准提升，请单击**是**。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  在中**命令提示符**窗口中，键入**net 使用\\ \\ <Server Name>\c$ /user:<Server Name>\Administrator**，其中<Server Name>是成员名称服务器或工作站尝试通过网络访问。  

    > [!NOTE]  
    > 要在网络上访问同一个系统必须为本地管理员凭据。  

6.  以下屏幕截图显示了应出现的错误消息。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝登录作为批处理作业"GPO 设置  
从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

###### <a name="create-a-batch-file"></a>创建一个批处理文件  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在中**记事本**，类型**dir c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在中**文件名**框中，键入 **<Filename>.bat** (其中<Filename>是新的批处理文件的名称)。  

###### <a name="schedule-a-task"></a>计划的任务  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入任务计划程序，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在计算机上运行 Windows 8 中，在**搜索**框中，键入**计划任务**，然后单击**计划任务**。  

3.  单击**操作**，然后单击**创建任务**。  

4.  在中**创建任务**对话框中，键入**<Task Name>** (其中<Task Name>是新的任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在中**操作**字段中，单击**启动程序**。  

7.  在中**程序/脚本**字段中，单击**浏览**，找到并选择中创建的批处理文件**创建一个批处理文件**部分，然后单击**打开**.  

8.  单击 **“确定”**。  

9. 单击“常规”选项卡。  

10. 在中**安全选项**字段中，单击**更改用户或组**。  

11. 类型系统的本地管理员帐户的名称，请单击**检查名称**，然后单击**确定**。  

12. 选择**运行或不用户是否登录**并**不存储密码**。 任务将仅具有本地计算机资源的访问权限。  

13. 单击 **“确定”**。  

14. 应出现一个对话框，请求的用户帐户凭据运行任务。  

15. 输入凭据后，单击**确定**。  

16. 应显示类似于以下的对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝作为登录服务"GPO 设置  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  在中**作为登录**字段中，单击**此帐户**。  

7.  单击**浏览**，键入系统的本地管理员帐户，单击**检查名称**，然后单击**确定**。  

8.  在中**密码**并**确认密码**字段中，键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**然后单击**重新启动**。  

11. 重新启动该服务时，应显示类似于以下的对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到的打印机后台处理程序服务所做的更改  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  在中**作为登录**： 字段中，选择**本地 Systemaccount**，然后单击**确定**。  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝通过登录远程桌面服务"GPO 设置  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在中**计算机**字段中，键入你想要连接到，然后单击的计算机的名称**Connect**。 （也可以键入而不是计算机名称的 IP 地址）。  

4.  出现提示时，提供凭据，为系统的本地**管理员**帐户。  

5.  应显示类似于以下的对话框。  

    ![安全的本地管理员帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
