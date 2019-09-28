---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: 附录 E-保护 Active Directory 中企业管理员组的安全
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 51337e5d4940a490045e146d32408d4f709bb6ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408714"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附录 E：在 Active Directory 中保护 Enterprise Admins 组

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附录 E：在 Active Directory 中保护 Enterprise Admins 组  
位于目录林根级域中的 Enterprise Admins （EA）组在日常工作中不应包含任何用户，只要根域的管理员帐户有可能例外，只要该帐户受到 [Appendix D：Active Directory @ no__t 中保护内置管理员帐户。  

默认情况下，企业管理员是林中每个域中 Administrators 组的成员。 不应从每个域的 Administrators 组中删除 EA 组，因为在林灾难恢复方案中，可能需要 EA 权限。 应该对林的 Enterprise Admins 组进行安全，详细信息请参阅下面的分步说明。  

对于林中的 Enterprise Admins 组：  

1.  在链接到包含每个域中成员服务器和工作站的 Ou 的 Gpo 中，Enterprise Admins 组应添加到 "**计算机配置 \windows 设置 \ 本地策略 \ 本地策略 \ 用户权限" 中的以下用户权限分配**：  

    -   拒绝从网络访问这台计算机  

    -   拒绝作为批处理作业登录  

    -   拒绝以服务身份登录  

    -   拒绝本地登录  

    -   拒绝通过远程桌面服务登录  

2.  配置审核，以便在对 Enterprise Admins 组的属性或成员身份进行任何修改时发送警报。  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>删除 Enterprise Admins 组中的所有成员的分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 " **Active Directory 用户和计算机**"。  

2.  如果你不管理林的根域，请在控制台树中右键单击 <Domain>，然后单击 "**更改域**" （其中 <Domain> 是当前正在管理的域的名称）。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  在 "**更改域**" 对话框中，单击 "**浏览**"，选择林的根域，然后单击 **"确定"** 。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  若要从 EA 组中删除所有成员：  

    1.  双击 " **Enterprise Admins** " 组，然后单击 "**成员**" 选项卡。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  选择组成员，单击 "**删除**"，单击 **"是**"，然后单击 **"确定"** 。  

5.  重复步骤2，直到删除 EA 组的所有成员。  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>在 Active Directory 中保护企业管理员的分步说明  

1.  在**服务器管理器**中，单击 "**工具**"，然后单击 "**组策略管理**"。  

2.  在控制台树中，展开 "<Forest> \ 域 @ no__t"，然后**组策略对象**（其中，@no__t 为林的名称，<Domain> 是要设置组策略的域的名称）。  

    > [!NOTE]  
    > 在包含多个域的林中，应在需要保护 Enterprise Admins 组的每个域中创建类似的 GPO。  

3.  在控制台树中，右键单击**组策略对象**"，然后单击"**新建**"。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  在 "**新建 GPO** " 对话框中，键入 <GPO Name>，然后单击 **"确定"** （其中 <GPO Name> 是此 GPO 的名称）。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  在详细信息窗格中，右键单击 <GPO Name>，然后单击 "**编辑**"。  

6.  导航到 "**计算机配置 \windows 设置 \ 安全设置 \ 本地策略**"，然后单击 "**用户权限分配**"。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  通过执行以下操作，将用户权限配置为阻止企业管理员组的成员访问网络上的成员服务器和工作站：  

    1.  双击 **"拒绝从网络访问此计算机"** ，并选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

    3.  键入**Enterprise Admins**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

8.  通过执行以下操作，将用户权限配置为阻止企业管理员组的成员作为批处理作业登录：  

    1.  双击 "**拒绝作为批处理作业登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

        > [!NOTE]  
        > 在包含多个域的林中，单击 "**位置**"，然后选择林的根域。  

    3.  键入**Enterprise Admins**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

9. 通过执行以下操作，将用户权限配置为禁止将 EA 组的成员作为服务登录：  

    1.  双击 "**拒绝日志为服务**"，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

        > [!NOTE]  
        > 在包含多个域的林中，单击 "**位置**"，然后选择林的根域。  

    3.  键入**Enterprise Admins**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

10. 通过执行以下操作，配置用户权限以阻止 Enterprise Admins 组的成员在本地登录到成员服务器和工作站：  

    1.  双击 "**拒绝本地登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

        > [!NOTE]  
        > 在包含多个域的林中，单击 "**位置**"，然后选择林的根域。  

    3.  键入**Enterprise Admins**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

11. 通过执行以下操作，配置用户权限以阻止 Enterprise Admins 组的成员通过远程桌面服务访问成员服务器和工作站：  

    1.  双击 "**拒绝通过远程桌面服务登录"** ，然后选择 "**定义这些策略设置**"。  

    2.  单击 "**添加用户或组**"，然后单击 "**浏览**"。  

        > [!NOTE]  
        > 在包含多个域的林中，单击 "**位置**"，然后选择林的根域。  

    3.  键入**Enterprise Admins**，单击 "**检查名称**"，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  单击 **"确定**"，然后再次单击 **"确定"** 。  

12. 若要退出**组策略管理编辑器**，请单击 "**文件**"，然后单击 "**退出**"。  

13. 在**组策略管理**中，通过执行以下操作将 GPO 链接到成员服务器和工作站 ou：  

    1.  导航到 <Forest> 域 @ no__t @ no__t-2 （其中，<Forest> 是林的名称，<Domain> 是要在其中设置组策略的域的名称）。  

    2.  右键单击 GPO 将应用到的 OU，然后单击 "**链接现有 GPO**"。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  选择刚创建的 GPO，然后单击 **"确定"** 。  

        ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  创建指向包含工作站的所有其他 Ou 的链接。  

    5.  创建指向包含成员服务器的所有其他 Ou 的链接。  

    6.  在包含多个域的林中，应在需要保护 Enterprise Admins 组的每个域中创建类似的 GPO。  

> [!IMPORTANT]  
> 如果使用跳转服务器来管理域控制器和 Active Directory，请确保 "跳转服务器" 位于此 Gpo 未链接到的 OU 中。  

### <a name="verification-steps"></a>验证步骤  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证 "拒绝从网络访问此计算机" GPO 设置  
在不受 GPO 更改（如 "跳转服务器"）影响的任何成员服务器或工作站上，尝试通过受 GPO 更改影响的网络访问成员服务器或工作站。 若要验证 GPO 设置，请执行以下步骤，尝试使用**NET USE**命令映射系统驱动器：  

1.  使用作为 EA 组成员的帐户在本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 "**命令提示符**"，右键单击 "**命令提示符**"，然后单击 "以**管理员身份运行**" 以打开提升的命令提示符。  

4.  当提示批准提升时，单击 **"是"** 。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  在 "**命令提示符**" 窗口中，键入**net use \\ @ no__t @ No__t-4Server name @ no__t-5\c $** ，其中 \<Server name @ no__t-7 是你尝试通过网络访问的成员服务器或工作站的名称。  

6.  以下屏幕截图显示应显示的错误消息。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证 "拒绝作为批处理作业登录" GPO 设置  

在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

##### <a name="create-a-batch-file"></a>创建批处理文件  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入**notepad**，然后单击 "**记事本**"。  

3.  在**记事本**中，键入**dir c：** 。  

4.  单击 "**文件**"，然后单击 "**另存为**"。  

5.  在 "**文件名" 框**中，键入 **<Filename>** （其中，<Filename> 是新批处理文件的名称）。  

##### <a name="schedule-a-task"></a>计划任务  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**任务计划程序**"，然后单击 "**任务计划程序**"。  

    > [!NOTE]  
    > 在运行 Windows 8 的计算机上的 "**搜索**" 框中，键入 "**计划任务**"，然后单击 "**计划任务**"。  

3.  单击 "**操作**"，然后单击 "**创建任务**"。  

4.  在 "**创建任务**" 对话框中，键入 **<Task Name>** （其中 @no__t 为新任务的名称）。  

5.  单击 "**操作**" 选项卡，然后单击 "**新建**"。  

6.  在 "**操作**" 字段中，选择 "**启动程序**"。  

7.  在 "**程序/脚本**" 下，单击 "**浏览**"，找到并选择 "**创建批处理文件**" 部分中创建的批处理文件，然后单击 "**打开**"。  

8.  单击 **“确定”** 。  

9. 单击“常规”选项卡。  

10. 在 "**安全选项**" 字段中，单击 "**更改用户或组**"。  

11. 键入作为 EAs 组成员的帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

12. 选择 **"不管用户是否登录都要运行"** ，并选择 "不**存储密码**"。 该任务将仅有权访问本地计算机资源。  

13. 单击 **“确定”** 。  

14. 应显示一个对话框，请求用户帐户凭据以运行任务。  

15. 输入凭据后，单击 **"确定"** 。  

16. 应出现如下所示的对话框。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证 "拒绝作为服务登录" GPO 设置  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**" 下，选择 "**此帐户**"。  

7.  单击 "**浏览**"，键入作为 EAs 组成员的帐户的名称，单击 "**检查名称**"，然后单击 **"确定"** 。  

8.  在 "**密码：** " 和 "**确认密码**" 下，键入选定帐户的密码，然后单击 **"确定"** 。  

9. 再单击 **"确定"** 三次。  

10. 右键单击**打印后台处理程序**服务，然后选择 "**重新启动**"。  

11. 重新启动该服务时，应显示类似于以下内容的对话框。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>还原对打印机后台处理程序服务所做的更改  

1.  在受 GPO 影响的任何成员服务器或工作站上，本地登录。  

2.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

3.  在**搜索**框中，键入 " **services**"，然后单击 "**服务**"。  

4.  找到并双击 "**打印后台处理程序**"。  

5.  单击 **“登录”** 选项卡。  

6.  在 "**登录身份**" 下，选择 "**本地系统**" 帐户，然后单击 **"确定"** 。  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>验证 "拒绝本地登录" GPO 设置  

1.  在受 GPO 更改影响的任何成员服务器或工作站上，尝试使用作为 EA 组成员的帐户在本地登录。 应出现如下所示的对话框。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证 "拒绝通过远程桌面服务登录" GPO 设置  

1.  用鼠标将指针移到屏幕的右上角或右下角。 出现**超级按钮**栏时，单击 "**搜索**"。  

2.  在**搜索**框中，键入 "**远程桌面连接**"，然后单击 "**远程桌面连接**"。  

3.  在 "**计算机**" 字段中，键入要连接到的计算机的名称，然后单击 "**连接**"。 （还可以键入 IP 地址而不是计算机名。）  

4.  出现提示时，请提供作为 EA 组成员的帐户的凭据。  

5.  应出现如下所示的对话框。  

    ![保护企业管理组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
