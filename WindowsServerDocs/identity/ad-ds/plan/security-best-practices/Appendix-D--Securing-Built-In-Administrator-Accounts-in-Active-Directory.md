---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: 附录 D-保护 Active Directory 中的内置管理员帐户
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cc91ccd00c951863f4f9802f3d669e36ff3f3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367845"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 D：保护 Active Directory 中的内置管理员帐户

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>附录 D：保护 Active Directory 中的内置管理员帐户  
在 Active Directory 的每个域中，将在创建域的过程中创建管理员帐户。 默认情况下，此帐户是域中的 "域管理员" 和 "管理员" 组的成员，并且如果域是目录林根级域，则该帐户也是 "企业管理员" 组的成员。

只应为初始构建活动和灾难恢复方案保留域的管理员帐户的使用。 若要确保在不能使用其他帐户的情况下可以使用管理员帐户来影响修复，则不应在林中的任何域中更改管理员帐户的默认成员身份。 相反，你应该按照下一部分中所述，在林中的每个域中保护管理员帐户的安全，详细信息请参阅下面的分步说明。 

> [!NOTE]
> 本指南用于推荐禁用该帐户。 此操作已删除，因为林恢复白皮书利用了默认的管理员帐户。 原因是，这是唯一允许在没有全局编录服务器的情况下登录的帐户。


#### <a name="controls-for-built-in-administrator-accounts"></a>内置管理员帐户的控件  
对于林中每个域中的内置 Administrator 帐户，应配置下列设置：  

-   启用该**帐户是敏感帐户，不能**对该帐户进行委派标志。  

-   在帐户上启用 "**交互式登录" 标志需要智能卡**。  

-   配置 Gpo 以限制管理员帐户在已加入域的系统上的使用：  

    -   在创建的一个或多个 Gpo，并链接到每个域中的工作站和成员服务器 Ou，将每个域的管理员帐户添加到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \" 中的以下用户权限 \用户权限分配**：  

        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

> [!NOTE]  
> 向此设置添加帐户时，必须指定是要配置本地管理员帐户还是域管理员帐户。 例如，要将 NWTRADERS 域的管理员帐户添加到这些拒绝权限，则必须将该帐户键入为 NWTRADERS\Administrator，或浏览到 NWTRADERS 域的管理员帐户。 如果在组策略对象编辑器中的这些用户权限设置中键入 "管理员"，则会限制应用该 GPO 的每台计算机上的本地管理员帐户。
>   
> 建议以与基于域的管理员帐户相同的方式限制成员服务器和工作站上的本地管理员帐户。 因此，通常应将林中每个域的管理员帐户和本地计算机的管理员帐户添加到这些用户权限设置。 以下屏幕截图显示了配置这些用户权限以阻止本地管理员帐户的示例，以及域的管理员帐户执行这些帐户不需要的登录。  


![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   配置 Gpo 以限制域控制器上的管理员帐户  
    -   在林中的每个域中，应修改默认域控制器 GPO 或链接到域控制器 OU 的策略，以将每个域的管理员帐户添加到 "计算机配置 \windows 设置" 中的以下用户权限 **\安全 \ 本地策略 \ 特权分配**：   
        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

> [!NOTE]  
> 这些设置将确保域的内置管理员帐户无法用于连接到域控制器，但如果启用该帐户，该帐户可以本地登录到域控制器。 由于此帐户只应在灾难恢复方案中启用并使用，因此预计将至少有一个域控制器的物理访问可用，或者具有远程访问域控制器权限的其他帐户可以广泛.  

-   配置管理员帐户的审核  

    在保护每个域的管理员帐户并将其禁用后，应将审核配置为监视对帐户的更改。 如果该帐户已启用、其密码被重置或对该帐户进行了任何其他修改，则应将警报发送到负责管理 Active Directory 的用户或团队，以及组织中的事件响应团队。  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 中保护内置管理员帐户的分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **Active Directory 用户和计算机**"。  

2.  若要防止利用委派的攻击使用其他系统上的帐户凭据，请执行以下步骤：  

    1.  右键单击**管理员**帐户，然后单击 "**属性**"。  

    2.  单击 "**帐户**" 选项卡。  

    3.  在 "**帐户选项**" 下，选择 "**帐户是敏感的，不能被委派**" 标志，如以下屏幕截图所示，然后单击 **"确定"** 。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  若要启用该帐户上**交互式登录标志需要智能卡**，请执行以下步骤：  

    1.  右键单击**管理员**帐户，然后选择 "**属性**"。  

    2.  单击 "**帐户**" 选项卡。  

    3.  在 "**帐户**选项" 下，选择 "**交互式登录时需要智能卡**" 标志，如以下屏幕截图所示，然后单击 **"确定"** 。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>配置 Gpo 以限制域级别的管理员帐户  

> [!WARNING]  
> 此 GPO 绝不应链接到域级，因为它可以使内置管理员帐户不可用，即使在灾难恢复方案中也是如此。  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 "**组策略管理**"。  

2.  在控制台树中，展开 "<Forest> \ 域 @ no__t"，然后**组策略对象**（其中，@no__t 为林的名称，<Domain> 是要在其中创建组策略的域的名称）。  

3.  在控制台树中，右键单击**组策略对象**"，然后单击"**新建**"。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  在 "**新建 GPO** " 对话框中，键入 <GPO Name>，然后单击 **"确定"** （其中，<GPO Name> 是此 GPO 的名称），如以下屏幕截图所示。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  在详细信息窗格中，右键单击 <GPO Name>，然后单击 "**编辑**"。  

6.  导航到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略**"，然后单击 "**用户权限分配**"。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  通过执行以下操作，配置用户权限以阻止管理员帐户通过网络访问成员服务器和工作站：  

    1.  双击 **"拒绝从网络访问此计算机"** ，并选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrator**，单击 "**检查名称**"，然后单击 **"确定"** 。 根据以下屏幕截图中所示，验证帐户是否显示为 <DomainName> \ 用户名格式。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

8.  通过执行以下操作，将用户权限配置为阻止管理员帐户作为批处理作业登录：  

    1.  双击 "**拒绝作为批处理作业登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrator**，单击 "**检查名称**"，然后单击 **"确定"** 。 根据以下屏幕截图中所示，验证帐户是否显示为 <DomainName> \ 用户名格式。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

9. 通过执行以下操作，将用户权限配置为阻止管理员帐户作为服务登录：  

    1.  双击 "**拒绝作为服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrator**，单击 "**检查名称**"，然后单击 **"确定"** 。 根据以下屏幕截图中所示，验证帐户是否显示为 <DomainName> \ 用户名格式。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

10. 通过执行以下操作，配置用户权限以阻止 BA 帐户通过远程桌面服务访问成员服务器和工作站：  

    1.  双击 "**拒绝通过远程桌面服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Administrator**，单击 "**检查名称**"，然后单击 **"确定"** 。 根据以下屏幕截图中所示，验证帐户是否显示为 <DomainName> \ 用户名格式。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

11. 若要退出**组策略管理编辑器**，请单击 "**文件**"，然后单击 "**退出**"。  

12. 在**组策略管理**中，通过执行以下操作将 GPO 链接到成员服务器和工作站 ou：  

    1.  导航到 <Forest> 域 @ no__t @ no__t-2 （其中，<Forest> 是林的名称，<Domain> 是要在其中设置组策略的域的名称）。  

    2.  右键单击 GPO 将应用到的 OU，然后单击 "**链接现有 GPO**"。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  选择已创建的 GPO，然后单击 **"确定"** 。  

        ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  创建指向包含工作站的所有其他 Ou 的链接。  

    5.  创建指向包含成员服务器的所有其他 Ou 的链接。  

> [!IMPORTANT]  
> 将管理员帐户添加到这些设置时，你可以指定是要配置本地管理员帐户还是域管理员帐户。 例如，若要将 AZURE-TAILSPINTOYS 域的管理员帐户添加到这些拒绝权限，请浏览到 AZURE-TAILSPINTOYS 域的管理员帐户，该帐户将显示为 TAILSPINTOYS\Administrator。 如果在组策略对象编辑器中的 "用户权限" 设置中键入 "管理员"，则会限制应用 GPO 的每台计算机上的本地管理员帐户，如前文所述。  

#### <a name="verification-steps"></a>验证步骤  
此处所述的验证步骤特定于 Windows 8 和 Windows Server 2012。  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>验证 "交互式登录需要智能卡" 帐户选项  

1.  在受 GPO 更改影响的任何成员服务器或工作站上，尝试使用域的内置管理员帐户以交互方式登录到域。 尝试登录后，应显示类似于以下内容的对话框。  

![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>验证 "帐户已禁用" 帐户选项  

1.  在受 GPO 更改影响的任何成员服务器或工作站上，尝试使用域的内置管理员帐户以交互方式登录到域。 尝试登录后，应显示类似于以下内容的对话框。  

![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证 "拒绝从网络访问此计算机" GPO 设置  
在不受 GPO 更改（如跳转服务器）影响的任何成员服务器或工作站上，尝试通过受 GPO 更改影响的网络访问成员服务器或工作站。 若要验证 GPO 设置，请执行以下步骤，尝试使用**NET USE**命令映射系统驱动器：  

1.  使用域的内置管理员帐户登录到域。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 "**命令提示符**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**" 以打开提升的命令提示符。  

4.  当提示批准提升时，单击 **"是"** 。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  在 "**命令提示符**" 窗口中，键入**net use \\ @ no__t @ No__t-4Server name @ no__t-5\c $** ，其中 \<Server name @ no__t-7 是你尝试通过网络访问的成员服务器或工作站的名称。  

6.  以下屏幕截图显示应显示的错误消息。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证 "拒绝作为批处理作业登录" GPO 设置  

在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

###### <a name="create-a-batch-file"></a>创建批处理文件  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入**notepad**，然后单击 "**记事本**"。  

3.  在**记事本**中，键入**dir c：** 。  

4.  单击 "**文件**"，然后单击 "**另存为**"。  

5.  在 "**文件名**" 字段中，键入 **<Filename>** （其中 <Filename> 是新批处理文件的名称）。  

###### <a name="schedule-a-task"></a>计划任务  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**任务计划程序**"，然后单击 "**任务计划程序**"。  

    > [!NOTE]  
    > 在运行 Windows 8 的计算机上的 "搜索" 框中，键入 "**计划任务**"，然后单击 "**计划任务**"。  

3.  在**任务计划程序**上，单击 "**操作**"，然后单击 "**创建任务**"。  

4.  在 "**创建任务**" 对话框中，键入 **<Task Name>** （其中 **<Task Name>** 是新任务的名称）。  

5.  单击 "**操作**" 选项卡，然后单击 "**新建**"。  

6.  在 "**操作：** " 下，选择 "**启动程序**"。  

7.  在 "**程序/脚本：** " 下，单击 "**浏览**"，找到并选择在 "创建批处理文件" 部分创建的批处理文件，然后单击 "**打开**"。  

8.  单击 **“确定”** 。  

9. 单击“常规”选项卡。  

10. 在 "**安全**选项" 下，单击 "**更改用户或组**"。  

11. 在域级别键入 BA 帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

12. 选择 **"运行用户是否登录"，或者 "** 不**存储密码**"。 该任务将仅有权访问本地计算机资源。  

13. 单击 **“确定”** 。  

14. 应显示一个对话框，请求用户帐户凭据以运行任务。  

15. 输入凭据后，单击 **"确定"** 。  

16. 应出现如下所示的对话框。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证 "拒绝作为服务登录" GPO 设置  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**" 下，选择 "**此帐户**"。  

7.  单击 "**浏览**"，在域级别键入 BA 帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

8.  在 "**密码：** " 和 "**确认密码：** " 下，键入管理员帐户的密码，然后单击 **"确定"** 。  

9. 再单击 **"确定"** 三次。  

10. 右键单击**打印后台处理程序服务**，然后选择 "**重新启动**"。  

11. 重新启动该服务时，应显示类似于以下内容的对话框。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原对打印机后台处理程序服务所做的更改  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份：** " 下，选择 "**本地系统**" 帐户，然后单击 **"确定"** 。  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证 "拒绝通过远程桌面服务登录" GPO 设置

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**远程桌面连接**"，然后单击 "**远程桌面连接**"。  

3.  在 "**计算机**" 字段中，键入要连接到的计算机的名称，然后单击 "**连接**"。 （还可以键入 IP 地址而不是计算机名。）  

4.  出现提示时，请在域级别为 BA 帐户的名称提供凭据。  

5.  应出现如下所示的对话框。  

    ![保护内置管理员帐户](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
