---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: 附录 H-保护本地管理员帐户和组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7e0cff62851250009d8af6ec7d87ec8191dcaec0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408634"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 H：保护本地管理员帐户和组

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>附录 H：保护本地管理员帐户和组  
在目前处于主流支持的所有 Windows 版本中，本地管理员帐户在默认情况下处于禁用状态，这将使该帐户无法用于传递哈希和其他凭据被盗攻击。 但是，在包含旧版操作系统的环境中或在其中启用了本地管理员帐户的环境中，可以使用这些帐户，如前面所述，在成员服务器和工作站之间传播安全漏洞。 应按照下面的分步说明中所述，保护每个本地管理员帐户和组。  

有关保护内置管理员（BA）组的注意事项的详细信息，请参阅[实施最少特权管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)。  

#### <a name="controls-for-local-administrator-accounts"></a>本地管理员帐户的控件  
对于林中每个域中的本地管理员帐户，应配置下列设置：  

-   配置 Gpo 以限制域的管理员帐户在加入域的系统上的使用  
    -   在你创建的一个或多个 Gpo 中，并链接到每个域中的工作站和成员服务器 Ou，将管理员帐户添加到 "**计算机配置 \windows 设置 \ 本地策略 \ 本地策略 \ 用户" 权限中的以下用户权限分配**：  

        -   拒绝从网络访问这台计算机  

        -   拒绝作为批处理作业登录  

        -   拒绝以服务身份登录  

        -   拒绝通过远程桌面服务登录  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>保护本地管理员组的分步说明  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>在已加入域的系统上配置 Gpo 以限制管理员帐户  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 "**组策略管理**"。  

2.  在控制台树中，展开 "<Forest> \ 域 @ no__t"，然后**组策略对象**（其中，@no__t 为林的名称，<Domain> 是要设置组策略的域的名称）。  

3.  在控制台树中，右键单击**组策略对象**"，然后单击"**新建**"。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  在 "**新建 GPO** " 对话框中，键入 **<GPO Name>** ，然后单击 **"确定"** （其中 <GPO Name> 是此 GPO 的名称）。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  在详细信息窗格中，右键单击 **<GPO Name>** ，然后单击 "**编辑**"。  

6.  导航到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略**"，然后单击 "**用户权限分配**"。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  通过执行以下操作，配置用户权限以阻止本地管理员帐户通过网络访问成员服务器和工作站：  

    1.  双击 **"拒绝从网络访问此计算机"** ，并选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，键入本地管理员帐户的用户名，然后单击 **"确定"** 。 此用户名将是 "**管理员**"，默认情况下安装 Windows 时。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  单击“确定”。  

        > [!IMPORTANT]  
        > 将管理员帐户添加到这些设置时，你可以指定是要配置本地管理员帐户还是域管理员帐户。 例如，若要将 AZURE-TAILSPINTOYS 域的管理员帐户添加到这些拒绝权限，请浏览到 AZURE-TAILSPINTOYS 域的管理员帐户，该帐户将显示为 TAILSPINTOYS\Administrator。 如果在组策略对象编辑器中的 "用户权限" 设置中键入**管理员**，则会限制应用 GPO 的每台计算机上的本地管理员帐户，如前文所述。  

8.  通过执行以下操作，配置用户权限以阻止本地管理员帐户作为批处理作业登录：  

    1.  双击 "**拒绝作为批处理作业登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，键入本地管理员帐户的用户名，然后单击 **"确定"** 。 此用户名将是 "**管理员**"，默认情况下安装 Windows 时。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  单击 **“确定”** 。  

        > [!IMPORTANT]  
        > 当你将管理员帐户添加到这些设置时，你将指定你是通过标记帐户的方式配置本地管理员帐户还是域管理员帐户。 例如，若要将 AZURE-TAILSPINTOYS 域的管理员帐户添加到这些拒绝权限，请浏览到 AZURE-TAILSPINTOYS 域的管理员帐户，该帐户将显示为 TAILSPINTOYS\Administrator。 如果在组策略对象编辑器中的 "用户权限" 设置中键入**管理员**，则会限制应用 GPO 的每台计算机上的本地管理员帐户，如前文所述。  

9. 通过执行以下操作，配置用户权限以阻止本地管理员帐户作为服务登录：  

    1.  双击 "**拒绝作为服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，键入本地管理员帐户的用户名，然后单击 **"确定"** 。 此用户名将是 "**管理员**"，默认情况下安装 Windows 时。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  单击 **“确定”** 。  

        > [!IMPORTANT]  
        > 当你将管理员帐户添加到这些设置时，你将指定你是通过标记帐户的方式配置本地管理员帐户还是域管理员帐户。 例如，若要将 AZURE-TAILSPINTOYS 域的管理员帐户添加到这些拒绝权限，请浏览到 AZURE-TAILSPINTOYS 域的管理员帐户，该帐户将显示为 TAILSPINTOYS\Administrator。 如果在组策略对象编辑器中的 "用户权限" 设置中键入**管理员**，则会限制应用 GPO 的每台计算机上的本地管理员帐户，如前文所述。  

10. 通过执行以下操作，配置用户权限以阻止本地管理员帐户通过远程桌面服务访问成员服务器和工作站：  

    1.  双击 "**拒绝通过远程桌面服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，键入本地管理员帐户的用户名，然后单击 **"确定"** 。 此用户名将是 "**管理员**"，默认情况下安装 Windows 时。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  单击 **“确定”** 。  

        > [!IMPORTANT]  
        > 当你将管理员帐户添加到这些设置时，你将指定你是通过标记帐户的方式配置本地管理员帐户还是域管理员帐户。 例如，若要将 AZURE-TAILSPINTOYS 域的管理员帐户添加到这些拒绝权限，请浏览到 AZURE-TAILSPINTOYS 域的管理员帐户，该帐户将显示为 TAILSPINTOYS\Administrator。 如果在组策略对象编辑器中的 "用户权限" 设置中键入**管理员**，则会限制应用 GPO 的每台计算机上的本地管理员帐户，如前文所述。  

11. 若要退出**组策略管理编辑器**，请单击 "**文件**"，然后单击 "**退出**"。  

12. 在**组策略管理**中，通过执行以下操作将 GPO 链接到成员服务器和工作站 ou：  

    1.  导航到 <Forest> 域 @ no__t @ no__t-2 （其中，<Forest> 是林的名称，<Domain> 是要在其中设置组策略的域的名称）。  

    2.  右键单击 GPO 将应用到的 OU，然后单击 "**链接现有 GPO**"。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  选择已创建的 GPO，然后单击 **"确定"** 。  

        ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  创建指向包含工作站的所有其他 Ou 的链接。  

    5.  创建指向包含成员服务器的所有其他 Ou 的链接。  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证 "拒绝从网络访问此计算机" GPO 设置  

在不受 GPO 更改（如跳转服务器）影响的任何成员服务器或工作站上，尝试通过受 GPO 更改影响的网络访问成员服务器或工作站。 若要验证 GPO 设置，请尝试使用**NET USE**命令映射系统驱动器。  

1.  本地登录到不受 GPO 更改影响的任何成员服务器或工作站。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 "**命令提示符**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**" 以打开提升的命令提示符。  

4.  当提示批准提升时，单击 **"是"** 。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  在 "**命令提示符**" 窗口中，键入**net use \\ @ no__t-3 @ no__t-4\c $/user： <Server Name> \ 管理员**，其中 <Server Name> 是你尝试通过网络访问的成员服务器或工作站的名称。  

    > [!NOTE]  
    > 本地管理员凭据必须来自你尝试通过网络访问的同一系统。  

6.  以下屏幕截图显示应显示的错误消息。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证 "拒绝作为批处理作业登录" GPO 设置  
在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

###### <a name="create-a-batch-file"></a>创建批处理文件  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入**notepad**，然后单击 "**记事本**"。  

3.  在**记事本**中，键入**dir c：** 。  

4.  单击 "**文件**"，然后单击 "**另存为**"。  

5.  在 "**文件名" 框中**，键入 **<Filename>** （其中，<Filename> 是新批处理文件的名称）。  

###### <a name="schedule-a-task"></a>计划任务  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "任务计划程序"，然后单击 "**任务计划程序**"。  

    > [!NOTE]  
    > 在运行 Windows 8 的计算机上的 "**搜索**" 框中，键入 "**计划任务**"，然后单击 "**计划任务**"。  

3.  单击 "**操作**"，然后单击 "**创建任务**"。  

4.  在 "**创建任务**" 对话框中，键入 **<Task Name>** （其中 @no__t 为新任务的名称）。  

5.  单击 "**操作**" 选项卡，然后单击 "**新建**"。  

6.  在 "**操作**" 字段中，单击 "**启动程序**"。  

7.  在 "**程序/脚本**" 字段中，单击 "**浏览**"，找到并选择 "**创建批处理文件**" 部分中创建的批处理文件，然后单击 "**打开**"。  

8.  单击 **“确定”** 。  

9. 单击“常规”选项卡。  

10. 在 "**安全选项**" 字段中，单击 "**更改用户或组**"。  

11. 键入系统本地管理员帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

12. 选择 **"运行用户是否登录"，或者 "** 不**存储密码**"。 该任务将仅有权访问本地计算机资源。  

13. 单击 **“确定”** 。  

14. 应显示一个对话框，请求用户帐户凭据以运行任务。  

15. 输入凭据后，单击 **"确定"** 。  

16. 应出现如下所示的对话框。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证 "拒绝作为服务登录" GPO 设置  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录为**" 字段中，单击 "**此帐户**"。  

7.  单击 "**浏览**"，键入系统的本地管理员帐户，单击 "**检查名称**"，然后单击 **"确定"** 。  

8.  在 "**密码**" 和 "**确认密码**" 字段中，键入所选帐户的密码，然后单击 **"确定"** 。  

9. 再单击 **"确定"** 三次。  

10. 右键单击 "**打印后台处理程序**"，然后单击 "**重新启动**"。  

11. 重新启动该服务时，应显示类似于以下内容的对话框。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>还原对打印机后台处理程序服务所做的更改  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**：" 字段中，选择 " **Local Systemaccount**"，然后单击 **"确定"** 。  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证 "拒绝通过远程桌面服务登录" GPO 设置  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**远程桌面连接**"，然后单击 "**远程桌面连接**"。  

3.  在 "**计算机**" 字段中，键入要连接到的计算机的名称，然后单击 "**连接**"。 （还可以键入 IP 地址而不是计算机名。）  

4.  出现提示时，提供系统本地**管理员**帐户的凭据。  

5.  应出现如下所示的对话框。  

    ![保护本地管理帐户和组](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
