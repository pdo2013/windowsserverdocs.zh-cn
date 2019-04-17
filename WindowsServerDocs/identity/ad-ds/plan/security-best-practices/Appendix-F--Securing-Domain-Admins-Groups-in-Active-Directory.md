---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: "附录 F-保护域的 Active Directory 的管理员组"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3535714e0df43cb94c7e89c503bc5f875cd49e28
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附录 f：保护域的 Active Directory 的管理员组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>附录 f：保护域的 Active Directory 的管理员组  
一样与企业管理员 (EA) 组这种情况，应仅在版本或灾难恢复方案中所需域管理员 (DA) 组中的成员。 应该会有任何日常用户帐户 DA 该组除外内置的管理员帐户的域中，如果安全中所述[附录 d： 保护内置管理员帐户中 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

默认情况下的所有成员服务器和各自域中的工作站上的本地管理员组成员是管理员域。 此默认嵌套不应的支持和灾难恢复修改。 如果已从成员服务器上的本地管理员组删除域管理员，该组应添加到每个成员服务器和域中的工作站上的管理员组。 应该安全每个域域管理员一组，述按照的分步说明进行操作。  

森林中的每个域管理员组：  

1.  提供保护中所述，从组中，通过内置的管理员帐户域，可能异常删除的所有成员[附录 d： 保护内置管理员帐户中 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

2.  应 Gpo 链接到华丽绚烂包含成员服务器和工作站每个域中的，在添加 DA 组在以下的用户权限**计算机配置 \ 策略 \windows \ 设置本地 Settings\User 权限分配**:  

    -   拒绝该计算机的访问从网络  

    -   作为批量作业拒绝登录  

    -   作为一项服务拒绝登录  

    -   本地拒绝登录  

    -   通过远程桌面服务用户权利拒绝登录  

3.  应配置审核如果属性或域管理组成员进行的任何修改发送警报。  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>删除域管理员组中的所有成员的分步说明  

1.  在**服务器管理器**，单击**工具**，然后单击**Active Directory 用户和计算机**。  

2.  若要删除的所有成员从 DA 组中，执行以下步骤：  

    1.  双击**域管理员**分组，并单击**成员**选项卡。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  选择组成员的上，单击**删除**，单击**是**，然后单击**确定**。  

3.  直到 DA 组中的所有成员已都删除，请重复第 2 步。  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>安全域管理员 Active Directory 中的分步说明  

1.  在**服务器管理器**，单击**工具**，然后单击**组策略管理**。  

2.  控制台树中，展开 \ < Forest\ > \\Domains\\\ < Domain\ >，然后**组策略对象**(其中 \ < Forest\ > 是森林的名称和 \ < Domain\ > 是你想要在组策略设置的域的名称)。  

3.  控制台树中，右键单击**组策略对象**，然后单击**新建**。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  在**新 GPO**对话框中，键入 \ < GPO Name\ >，并单击**确定**(其中 \ < GPO Name\ > 是此 GPO 的名称)。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  在详细信息窗格中，右键单击 \ < GPO Name\ >，然后单击**编辑**。  

6.  导航到**计算机配置 \ 策略 \windows \ 本地策略**，然后单击**用户权限分配**。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  配置的用户权限，以防止域管理员组成员通过网络访问成员服务器和工作站通过执行以下操作：  

    1.  双击**拒绝到此计算机的访问，从网络**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**域管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  单击**确定**，并**确定**再次。  

8.  配置用户权限若要防止 DA 组成员作为批量作业登录通过执行以下操作：  

    1.  双击**拒绝作为批量作业登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**域管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  单击**确定**，并**确定**再次。  

9. 配置用户权限若要防止 DA 组成员作为一项服务登录通过执行以下操作：  

    1.  双击**拒绝服务作为登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**域管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  单击**确定**，并**确定**再次。  

10. 配置用户权限若要防止域管理员组成员本地登录到成员服务器和工作站通过执行以下操作：  

    1.  双击**本地拒绝登录**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**域管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  单击**确定**，并**确定**再次。  

11. 配置用户防止域管理员组成员通过执行以下访问成员服务器和工作站通过远程桌面服务的权限：  

    1.  双击**拒绝登录通过远程桌面服务**选择**定义这些策略设置**。  

    2.  单击**添加的用户或组**单击**浏览**。  

    3.  键入**域管理员**，单击**检查名称**，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  单击**确定**，并**确定**再次。  

12. 若要退出**组策略管理编辑器**，单击**文件**，然后单击**退出**。  

13. 在组策略管理 GPO 链接到成员服务器和工作站华丽绚烂通过执行以下操作：  

    1.  导航到 \ < Forest\ > \Domains\\\ < Domain\ > (其中 \ < Forest\ > 是森林的名称和 \ < Domain\ > 是你想要在组策略设置的域的名称)。  

    2.  右键单击 GPO 将应用于和单击的 OU**链接现有的组策略对象**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  选择刚刚创建的 GPO，然后单击**确定**。  

        ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  创建包含工作站所有其他华丽绚烂的链接。  

    5.  创建包含成员服务器所有其他华丽绚烂的链接。  

        > [!IMPORTANT]  
        > 如果用于管理域控制器和 Active Directory 跳转服务器，请确保跳转服务器位于的 OU Gpo 未为此链接。  

#### <a name="verification-steps"></a>验证步骤  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>验证"拒绝访问到此计算机的网络"GPO 设置  
从任何成员服务器或不受更改 （例如"跳转服务器"） 的工作站上，尝试访问成员服务器或工作站通过受更改的网络。 若要验证 GPO 设置，请尝试使用映射系统驱动器**网络使用**命令。  

1.  本地使用的是域管理组成员的帐户登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**权限的命令提示符**，右键单击**权限的命令提示符**，然后单击**以管理员身份运行**打开提升了权限的命令提示符窗口。  

4.  当批准提升提示，单击**是**。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  在**权限的命令提示符**窗口中，键入**网络使用 \\\ < 服务器 Name\ > \c$**，其中 \ < 服务器 Name\ > 是成员服务器或工作站在尝试访问通过网络的名称。  

6.  下面的屏幕截图显示应该会显示错误消息。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>验证"拒绝作为登录批量作业"GPO 设置  

任何成员服务器或更改受影响的工作站上，从本地登录。  

###### <a name="create-a-batch-file"></a>创建批量文件  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**记事本**，然后单击**记事本**。  

3.  在**记事本**，类型**目录 c:**。  

4.  单击**文件**，然后单击**另存为**。  

5.  在**文件**字段名称、 类型**\ < Filename\ >.bat** (其中 \ < Filename\ > 是新批量文件的名称)。  

###### <a name="schedule-a-task"></a>计划任务  

1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**任务计划程序**，然后单击**任务计划程序**。  

    > [!NOTE]  
    > 在计算机上运行 Windows 8，请在**搜索**框中，键入**计划任务**，然后单击**计划任务**。  

3.  在**任务计划程序**菜单栏中，单击**操作**，然后单击**创建任务**。  

4.  在**创建任务**对话框中，键入**\ < 任务 Name\ >** (其中 \ < 任务 Name\ > 被新任务的名称)。  

5.  单击**操作**选项卡，然后单击**新建**。  

6.  在**操作**字段中，选择**启动程序**。  

7.  下**程序/脚本**，单击**浏览**，找到并选择在创建的批量文件**创建批量文件**部分，然后单击**打开**。  

8.  单击**确定**。  

9. 单击**常规**选项卡。  

10. 下**安全**选项，请单击**更改用户或组**。  

11. 键入的是域管理员组成员的帐户名称、 单击**检查名称**，然后单击**确定**。  

12. 选择**运行或无法用户是否登录**选择**不要将密码存储**。 此任务将只能访问本地计算机资源。  

13. 单击**确定**。  

14. 应该会显示一个对话框中，请求的用户帐户凭据运行任务。  

15. 输入凭据后, 单击**确定**。  

16. 应该会显示类似于以下对话框。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>验证"拒绝登录即服务"GPO 设置  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  下**作为登录**、 选择**此帐户**选项。  

7.  单击**浏览**、 键入的是域管理员组成员的帐户名称、 单击**检查名称**，然后单击**确定**。  

8.  下**密码**和**确认密码**、 键入所选的帐户的密码，然后单击**确定**。  

9. 单击**确定**三次。  

10. 右键单击**打印后台处理程序**单击**重启**。  

11. 该服务重新启动后，应该会显示类似于以下对话框。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>还原到打印机的后台服务的更改  

1.  任何成员服务器或更改受影响的工作站上，从本地登录。  

2.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

3.  在**搜索**框中，键入**服务**，然后单击**服务**。  

4.  找到并双击**打印后台处理程序**。  

5.  单击**登录**选项卡。  

6.  下**作为登录**、 选择**本地系统**帐户，然后单击**确定**。  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>验证"拒绝登录本地"GPO 设置  

1.  从任何成员服务器或更改受影响的工作站上，尝试使用本地组成员的域管理员帐户登录。 应该会显示类似于以下对话框。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>验证"拒绝登录远程桌面服务通过"GPO 设置    
1.  使用鼠标，到屏幕的右上角或的右下角中移动指针。 当**超级按钮**栏显示，请单击**搜索**。  

2.  在**搜索**框中，键入**远程桌面连接**，然后单击**远程桌面连接**。  

3.  在**计算机**字段中，键入你想要连接到和单击的计算机名称**连接**。 （你可以键入 IP 地址代替计算机名称）。  

4.  看到提示后，提供组成员的域管理员帐户凭据。  

5.  应该会显示类似于以下对话框。  

    ![安全域管理员组](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
