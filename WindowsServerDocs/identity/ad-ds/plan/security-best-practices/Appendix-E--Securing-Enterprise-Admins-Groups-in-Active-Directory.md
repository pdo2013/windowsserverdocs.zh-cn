---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: 附录 E-保护 Active Directory 中的 Enterprise Admins 组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856718"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附录 E：保护 Active Directory 中的 Enterprise Admins 组

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>附录 E：保护 Active Directory 中的 Enterprise Admins 组  
Enterprise Admins (EA) 组中，该表存放在目录林根域中，应包含在日常工作中，但可能根域的管理员帐户的任何用户提供保护中所述[附录 d:保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

默认情况下，在林中每个域中的 Administrators 组的成员是 Enterprise Admins。 由于发生林灾难恢复方案中，将可能需要 EA 权限，不应从每个域中的管理员组删除 EA 组。 应保护林的 Enterprise Admins 组的分步说明，请按照中所述。  

对于在林中 Enterprise Admins 组：  

1.  应将 Gpo 链接到包含成员服务器和工作站中的每个域的 Ou，企业管理员组添加到中的以下用户权限**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略 \ Policies\用户权限分配**:  

    -   拒绝从网络访问这台计算机  

    -   拒绝作为批处理作业登录  

    -   拒绝以服务身份登录  

    -   拒绝本地登录  

    -   拒绝通过远程桌面服务登录  

2.  配置审核，以发送警报，如果对属性或 Enterprise Admins 组的成员身份进行任何修改。  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>从 Enterprise Admins 组中删除所有成员的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  如果你不管理根域林，在控制台树中，右键单击<Domain>，然后单击**更改域**(其中<Domain>是你当前管理的域的名称)。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  在中**更改域**对话框中，单击**浏览**，选择根域林，然后单击**确定**。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  若要从 EA 组中删除所有成员：  

    1.  双击**Enterprise Admins**组，然后单击**成员**选项卡。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  选择组的成员，单击**删除**，单击**是**，然后单击**确定**。  

5.  只有在删除后 EA 组的所有成员，请重复步骤 2。  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>保护 Active Directory 中的 Enterprise Admins 的分步说明  

1.  在中**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  在控制台树中，展开<Forest>\Domains\\<Domain>，然后**组策略对象**(其中<Forest>是林的名称和<Domain>是，您希望对域的名称设置组策略）。  

    > [!NOTE]  
    > 在包含多个域的林中，应在需要保护 Enterprise Admins 组的每个域中创建类似 GPO。  

3.  在控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  在中**新的 GPO**对话框中，键入<GPO Name>，然后单击**确定**(其中<GPO Name>是此 GPO 的名称)。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  在细节窗格中，右键单击<GPO Name>，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows 设置 \ 安全设置 \ 本地策略**，然后单击**用户权限分配**。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  配置用户权限来防止 Enterprise Admins 组的成员通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝从网络访问这台计算机**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

    3.  类型**Enterprise Admins**，单击**检查名称**，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  单击**确定**，并**确定**试。  

8.  配置用户权限以防止 Enterprise Admins 组的成员作为批处理作业登录通过执行以下操作：  

    1.  双击**拒绝作为批处理作业登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**然后单击**浏览**。  

        > [!NOTE]  
        > 在包含多个域的林中，请单击**位置**和选择的目录林根域。  

    3.  类型**Enterprise Admins**，单击**检查名称**，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  单击**确定**，并**确定**试。  

9. 配置用户权限以防止 EA 组的成员作为服务登录通过执行以下操作：  

    1.  双击**拒绝日志作为一项服务**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，然后单击**浏览**。  

        > [!NOTE]  
        > 在包含多个域的林中，请单击**位置**和选择的目录林根域。  

    3.  类型**Enterprise Admins**，单击**检查名称**，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  单击**确定**，并**确定**试。  

10. 配置用户权限以防止 Enterprise Admins 组的成员在本地登录到成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝本地登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，然后单击**浏览**。  

        > [!NOTE]  
        > 在包含多个域的林中，请单击**位置**和选择的目录林根域。  

    3.  类型**Enterprise Admins**，单击**检查名称**，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  单击**确定**，并**确定**试。  

11. 配置用户权限来防止 Enterprise Admins 组的成员访问成员服务器和工作站通过远程桌面服务通过执行以下操作：  

    1.  双击**拒绝通过远程桌面服务登录**，然后选择**定义这些策略设置**。  

    2.  单击**添加用户或组**，然后单击**浏览**。  

        > [!NOTE]  
        > 在包含多个域的林中，请单击**位置**和选择的目录林根域。  

    3.  类型**Enterprise Admins**，单击**检查名称**，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  单击**确定**，并**确定**试。  

12. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

13. 在中**组策略管理**，将 GPO 链接到的成员服务器和工作站的 Ou，执行以下操作：  

    1.  导航到<Forest>\Domains\\ <Domain> (其中<Forest>是林的名称和<Domain>是你想要将组策略设置的域的名称)。  

    2.  右键单击该 GPO 将应用于和单击的 OU**链接现有 GPO**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  选择刚创建的 GPO，然后单击**确定**。  

        ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  创建链接到包含工作站的所有其他 Ou。  

    5.  创建链接到包含成员服务器的所有其他 Ou。  

    6.  在包含多个域的林中，应在需要保护 Enterprise Admins 组的每个域中创建类似 GPO。  

> [!IMPORTANT]  
> 如果跳转服务器用于管理域控制器和 Active Directory，请确保跳转服务器位于 Gpo 未链接到此 OU 中。  

### <a name="verification-steps"></a>验证步骤  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝从网络访问这台计算机"GPO 设置  
从任何成员服务器或不受 GPO 更改 （例如"跳转服务器"） 的工作站，尝试通过 GPO 更改的会影响网络访问的成员服务器或工作站。 若要验证的 GPO 设置，请尝试通过使用系统驱动器映射**NET USE**命令，请执行以下步骤：  

1.  本地使用的是 EA 组的成员的帐户登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在**搜索**框中，键入**命令提示符**，右键单击**命令提示符下**，然后单击**以管理员身份运行**若要打开提升命令提示符。  

4.  当系统提示您批准提升，请单击**是**。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  在中**命令提示符**窗口中，键入**net 使用\\ \\\<服务器名称\>\c$**，其中\<服务器名称\>是成员服务器或要通过网络访问的工作站的名称。  

6.  下面的屏幕截图显示了应出现的错误消息。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝登录作为批处理作业"GPO 设置  

从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

##### <a name="create-a-batch-file"></a>创建一个批处理文件  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在中**记事本**，类型**dir c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在中**文件**名称框中，键入 **<Filename>.bat** (其中<Filename>是新的批处理文件的名称)。  

##### <a name="schedule-a-task"></a>计划的任务  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在计算机上运行 Windows 8 中，在**搜索**框中，键入**计划任务**，然后单击**计划任务**。  

3.  单击**操作**，然后单击**创建任务**。  

4.  在中**创建任务**对话框中，键入**<Task Name>** (其中<Task Name>是新的任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在中**操作**字段中，选择**启动程序**。  

7.  下**程序/脚本**，单击**浏览**，找到并选择中创建的批处理文件**创建一个批处理文件**部分，然后单击**打开**.  

8.  单击 **“确定”**。  

9. 单击“常规”选项卡。  

10. 在中**安全选项**字段中，单击**更改用户或组**。  

11. 键入该帐户是 EAs 组的成员的名称，单击**检查名称**，然后单击**确定**。  

12. 选择**运行或不用户是否登录**，然后选择**不存储密码**。 任务将仅具有本地计算机资源的访问权限。  

13. 单击 **“确定”**。  

14. 应出现一个对话框，请求的用户帐户凭据运行任务。  

15. 输入凭据后，单击**确定**。  

16. 应显示类似于以下的对话框。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝作为登录服务"GPO 设置  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  下**作为登录**，选择**此帐户**。  

7.  单击**浏览**，键入该帐户是 EAs 组的成员的名称，单击**检查名称**，然后单击**确定**。  

8.  下**密码：** 并**确认密码**，键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**服务，然后选择**重新启动**。  

11. 重新启动该服务时，应显示类似于以下的对话框。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到的打印机后台处理程序服务所做的更改  

1.  从任何成员服务器或工作站的 GPO 更改影响，在本地登录。  

2.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

3.  在中**搜索**框中，键入**services**，然后单击**Services**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击 **“登录”** 选项卡。  

6.  下**作为登录**，选择**本地系统**帐户，然后单击**确定**。  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>验证"拒绝本地登录"GPO 设置  

1.  从任何成员服务器或工作站的 GPO 更改影响，尝试在本地使用的是 EA 组的成员的帐户登录。 应显示类似于以下的对话框。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝通过登录远程桌面服务"GPO 设置  

1.  使用鼠标，将指针移动到屏幕的右上角或右下角。 当**超级按钮**栏出现后，单击**搜索**。  

2.  在中**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在中**计算机**字段中，键入你想要连接的对象，然后单击的计算机的名称**Connect**。 （也可以键入而不是计算机名称的 IP 地址）。  

4.  出现提示时，是 EA 组的成员的帐户提供凭据。  

5.  应显示类似于以下的对话框。  

    ![保护企业管理员组](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
