---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 附录 D-确保在 Active Directory 中的内置 Administrator 帐户的安全
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 51e503f55ee0fca1f1a53339de555fd213c69296
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870678"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 D：确保在 Active Directory 中的内置 Administrator 帐户的安全

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 D：确保在 Active Directory 中的内置 Administrator 帐户的安全  
在 Active Directory 中的每个域，管理员帐户被创建为域创建的过程。 此帐户是默认情况下在域中 Domain Admins 和 Administrators 组的成员，并且如果域是目录林根域，帐户也是 Enterprise Admins 组的成员。

应仅对初始生成活动和灾难恢复方案可能被保留域的管理员帐户的使用。 若要确保管理员帐户可用于影响修复，可以使用任何其他帐户，不应更改默认成员身份的林中任何域中的管理员帐户。 相反下, 一节中所述，应保护林中每个域中的管理员帐户安全，并遵循的分步说明中详细介绍。 

> [!NOTE]
> 本指南用于建议禁用帐户。 这已作为林恢复白皮书删除使用的默认管理员帐户。 原因是，这是唯一的帐户，而无需为全局编录服务器的登录。


#### <a name="controls-for-built-in-administrator-accounts"></a>内置管理员帐户的控件  
在您的林中每个域中的内置管理员帐户，应配置下列设置：  

-   启用**敏感帐户，不能被委派**帐户上的标志。  

-   启用**智能卡是交互式登录必须**帐户上的标志。  

-   配置 Gpo 以限制在已加入域的系统上的管理员帐户的使用：  

    -   在创建并链接到 Ou 中的每个域的工作站和成员服务器的一个或多个 Gpo，将每个域的管理员帐户添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \策略 \ 用户权限分配**:  

        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

> [!NOTE]  
> 当将帐户添加到此设置时，必须指定配置本地管理员帐户或域管理员帐户。 例如，若要添加到这些 NWTRADERS 域管理员帐户拒绝权限，必须为 NWTRADERS\Administrator 或浏览到管理员帐户为 NWTRADERS 域键入的帐户。 如果这些用户权限设置在组策略对象编辑器中键入"Administrator"，将限制将 GPO 应用于每台计算机上的本地管理员帐户。
>   
> 我们建议将作为基于域的管理员帐户相同的方式限制成员服务器和工作站上的本地管理员帐户。 因此，您应通常添加在林中每个域的管理员帐户和本地计算机的管理员帐户对这些用户权限设置。 以下屏幕截图显示了配置这些用户权限以阻止本地管理员帐户和执行不需要为这些帐户的登录域的管理员帐户的示例。  


![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   配置 Gpo 以限制域控制器上的管理员帐户  
    -   在林中每个域中，默认域控制器 GPO 或策略链接到 OU 应修改每个域的管理员帐户添加到中的以下用户权限的域控制器**计算机配置 \ 策略 \windows设置 \ 安全设置 \ 本地策略 \ 用户权限分配**:   
        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

> [!NOTE]  
> 这些设置将确保域的内置管理员帐户不能使用连接到域控制器，尽管该帐户，如果启用，可以在本地登录到域控制器。 因为此帐户应仅启用和使用在灾难恢复方案中，我们已预见到的物理访问至少一个域控制器将在可用，也可以是其他帐户有权远程访问域控制器使用。  

-   配置管理员帐户的审核  

    已保护的每个域的管理员帐户并将其禁用，应配置审核，以对帐户的更改的监视器。 如果启用了帐户、 重置其密码，或任何其他修改都会对该帐户，则警报应发送到用户或团队负责管理的 Active Directory 中，除了在组织中的事件响应团队。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>保护 Active Directory 中的内置 Administrator 帐户的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  若要阻止攻击利用委派，以便在其他系统上使用该帐户的凭据，请执行以下步骤：  

    1.  右键单击**管理员**帐户并单击**属性**。  

    2.  单击**帐户**选项卡。  

    3.  下**帐户选项**，选择**敏感帐户，不能被委派**标志，如以下屏幕截图中所示，然后单击**确定**。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要启用**智能卡是交互式登录必须**帐户上的标志，请执行以下步骤：  

    1.  右键单击**管理员**帐户并选择**属性**。  

    2.  单击**帐户**选项卡。  

    3.  下**帐户**选项中，选择**智能卡是交互式登录必须**标志，如以下屏幕截图中所示，然后单击**确定**。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>配置 Gpo 以限制在域级别的管理员帐户  

> [!WARNING]  
> 因为这可以使内置 Administrator 帐户不可用，即使在灾难恢复方案中，永远不应在域级别链接此 GPO。  

1.  在中**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  在控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是林的名称和<Domain>是，您希望对域的名称创建的组策略）。  

3.  在控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  在**新的 GPO**对话框中，键入<GPO Name>，然后单击**确定**(其中<GPO Name>是此 GPO 的名称) 下面的屏幕截图中所示。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在细节窗格中，右键单击<GPO Name>，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略**，然后单击**用户权限分配**。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  配置用户权限来防止管理员帐户通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝从网络访问这台计算机**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式如以下屏幕截图中所示。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  单击**确定**，并**确定**试。  

8.  配置用户权限以防止管理员帐户作为批处理作业登录通过执行以下操作：  

    1.  双击**拒绝作为批处理作业登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式如以下屏幕截图中所示。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  单击**确定**，并**确定**试。  

9. 配置用户权限以防止管理员帐户作为服务登录通过执行以下操作：  

    1.  双击**拒绝作为服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式如以下屏幕截图中所示。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  单击**确定**，并**确定**试。  

10. 配置用户权限来防止 BA 帐户访问成员服务器和工作站通过远程桌面服务通过执行以下操作：  

    1.  双击**拒绝通过远程桌面服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**管理员**，单击**检查名称**，然后单击**确定**。 验证该帐户是否显示在<DomainName>\Username 格式如以下屏幕截图中所示。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  单击**确定**，并**确定**试。  

11. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

12. 在中**组策略管理**，将 GPO 链接到的成员服务器和工作站的 Ou，执行以下操作：  

    1.  导航到<Forest>\Domains\\ <Domain> (其中<Forest>是林的名称和<Domain>是你想要将组策略设置的域的名称)。  

    2.  右键单击该 GPO 将应用于和单击的 OU**链接现有 GPO**。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  选择你创建的 GPO，然后单击**确定**。  

        ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  创建链接到包含工作站的所有其他 Ou。  

    5.  创建链接到包含成员服务器的所有其他 Ou。  

> [!IMPORTANT]  
> 将管理员帐户添加到这些设置，当您指定是否要配置的本地管理员帐户或域管理员帐户的帐户的标签。 例如，若要添加到这些 TAILSPINTOYS 域管理员帐户拒绝权限，您应浏览到 TAILSPINTOYS 域的管理员帐户中，这将显示为 TAILSPINTOYS\Administrator。 如果这些用户权限设置在组策略对象编辑器中键入"Administrator"，则将限制 GPO 应用于每台计算机上的本地管理员帐户，如前面所述。  

#### <a name="verification-steps"></a>验证步骤  
此处所述的验证步骤是特定于 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>验证"时进行交互式登录需要智能卡"帐户选项  

1.  从任何成员服务器或工作站的 GPO 更改影响，尝试以交互方式登录到域使用的域的内置管理员帐户。 后尝试登录时，应显示类似于以下的对话框。  

![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>验证"已禁用帐户"帐户选项  

1.  从任何成员服务器或工作站的 GPO 更改影响，尝试以交互方式登录到域使用的域的内置管理员帐户。 后尝试登录时，应显示类似于以下的对话框。  

![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝从网络访问这台计算机"GPO 设置  
从任何成员服务器或不受 GPO 更改 （例如跳转服务器） 的工作站，尝试通过 GPO 更改的会影响网络访问的成员服务器或工作站。 若要验证的 GPO 设置，请尝试通过使用系统驱动器映射**NET USE**命令，请执行以下步骤：  

1.  登录到使用域的内置管理员帐户的域。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在**搜索**框中，键入**命令提示符**，右键单击**命令提示符下**，然后单击**以管理员身份运行**若要打开提升命令提示符。  

4.  当系统提示您批准提升，请单击**是**。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在中**命令提示符**窗口中，键入**net 使用\\ \\\<服务器名称\>\c$**，其中\<服务器名称\>是成员服务器或工作站尝试通过网络访问的名称。  

6.  以下屏幕截图显示了应出现的错误消息。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝登录作为批处理作业"GPO 设置  

从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

###### <a name="create-a-batch-file"></a>创建一个批处理文件  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在中**记事本**，类型**dir c:**。  

4.  单击**文件**然后单击**另存为**。  

5.  在中**文件名**字段中，键入 **<Filename>.bat** (其中<Filename>是新的批处理文件的名称)。  

###### <a name="schedule-a-task"></a>计划的任务  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在搜索框中，在运行 Windows 8 的计算机上键入**计划任务**，然后单击**计划任务**。  

3.  上**任务计划程序**，单击**操作**，然后单击**创建任务**。  

4.  在中**创建任务**对话框中，键入**<Task Name>** (其中**<Task Name>** 是新的任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  下**操作：**，选择**启动程序**。  

7.  下**程序/脚本：**，单击**浏览**，找到并选择在"创建批处理文件"部分中，创建的批处理文件，并单击**打开**。  

8.  单击 **“确定”**。  

9. 单击“常规”选项卡。  

10. 下**安全**选项，请单击**更改用户或组**。  

11. 键入 BA 帐户的名称的域级别，单击**检查名称**，然后单击**确定**。  

12. 选择**运行或不用户是否登录**并**不存储密码**。 任务将仅具有本地计算机资源的访问权限。  

13. 单击 **“确定”**。  

14. 应出现一个对话框，请求的用户帐户凭据运行任务。  

15. 输入凭据后，单击**确定**。  

16. 应显示类似于以下的对话框。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝作为登录服务"GPO 设置  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  下**作为登录：**，选择**此帐户**。  

7.  单击**浏览**，键入在域级别 BA 帐户的名称，单击**检查名称**，然后单击**确定**。  

8.  下**密码：** 并**确认密码：**，键入管理员帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**后台打印程序服务**，然后选择**重新启动**。  

11. 重新启动该服务时，应显示类似于以下的对话框。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到的打印机后台处理程序服务所做的更改  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  下**作为登录：**，选择**本地系统**帐户，然后单击**确定**。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝通过登录远程桌面服务"GPO 设置

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在中**计算机**字段中，键入你想要连接到，然后单击的计算机的名称**Connect**。 （也可以键入而不是计算机名称的 IP 地址）。  

4.  出现提示时，在域级别的 BA 帐户的名称提供凭据。  

5.  应显示类似于以下的对话框。  

    ![确保内置管理员帐户的安全](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
