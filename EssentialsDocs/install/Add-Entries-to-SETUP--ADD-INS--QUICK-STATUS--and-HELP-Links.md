---
title: "添加到设置外接程序、快速状态，并帮助链接的项目"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0a8f10d-fd85-4c8d-b9bb-176cb1db1f46
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6d3303f2c6d84932ad9d5dee8a547cd478447732
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>添加到设置外接程序、快速状态，并帮助链接的项目

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以添加到任务**设置**，**外接程序**，**快速状态**任务列表，以及你可以添加到仪表板中的家庭页面的社区链接部分的链接。 任务和链接通过放置 XML 文件名为 OEMHomePageContent.home 文件或 Windows Server \Bin\Addins\Home %ProgramFiles%\ 名称 OEMHomePageContent.dll 嵌入的资源文件添加到这些列表和部分。 嵌入的资源文件可用于本地化在任务以及您添加的链接文本。 .Home 文件包含任务和链接 XML 的定义。  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>设置，加载项快速状态的任务列表中添加任务，并帮助任务到添加链接  
 你可以添加到任务**设置**，**外接程序**，**快速状态**任务列表，并提供指向**帮助**任务通过定义任务和使用 XML、链接或者创建嵌入的资源文件，并安装在服务器上的文件。 当 XML 文件安装在不使用的资源文件服务器，必须对该命名 OEMHomePageContent.home。 如果组件用于安装 XML 文件和资源文件，必须具有 OEMHomePageContent.dll 名称，并且必须登录的验证码。  
  
### <a name="define-the-tasks-and-links"></a>定义任务和链接  
 可用于文本编辑器如记事本创建.home 文件，或者如果你还将创建嵌入的资源文件你可以使用 Visual Studio 2010 或更高版本定义文件。 下面的过程中展示了如何使用 Visual Studio 2010 或更高版本，以创建该文件。  
  
##### <a name="to-define-the-tasks-and-links"></a>若要定义任务和链接  
  
1.  打开 Visual Studio 2010 或更高版本，以管理员身份登录，方法是右键单击开始菜单中的程序，并选择**以管理员身份运行**。  
  
2.  单击**文件**，单击**新建**，然后单击**项目**。  
  
3.  在**模板**窗格中，单击**类库**，类型**OEMHomePageContent**中**名称**框中，，然后单击**确定**。  
  
4.  删除 Class1.cs 文件。  
  
5.  右键单击新项目，请单击**添加**，然后单击**新项目**。  
  
6.  在**模板**窗格中，单击**XML 文件**，类型**OEMHomePageContent.home**中**名称**框中，，然后单击**添加**。  
  
    > [!NOTE]
    >  如果没有的资源文件的情况下安装的了 XML 文件，必须对其命名 OEMHomePageContent.home。 如果它将包括在装配，它可获得任何名，只要它具有.home 扩展。  
  
7.  添加到 OEMHomePageContent.home 文件以下代码：  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyTaskDescription"  
             id="GUID">  
                  <Action   
                  name=?MyAction1Name?   
                  image=?IconForAction1?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                  <Action   
                  name=?MyAction2Name?   
                  image=?IconForAction2?  
                  type=?TaskType?  
                  exelocation=?ActionExeLocation? />  
                   ¦  
           </Task>  
                   ¦  
        </SetupMyServerTasks>  
    <MailServiceTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œConnect to Email Service? category. -->  
    </MailServiceTasks>  
    <LineOfBusinessTasks>  
         <!-- Same schema as in œSetupMyServerTasks? but the tasks are shown in œAdd-ins? category. -->  
  
    <GetQuickStatusTasks>  
          <Task name="MyQuickStatusTask1"  
             description="MyQuickStatusTask1Desc   "  
             id="GUID"  
             assembly="AssemblyName of quick status query implementation"  
             class="ClassName of quick status query implementation"           
             replaceid="GUID"/>  
               <!--  Same schema as Actions in œSetupMyServerTasks? -->   
             </Task>  
    </GetQuickStatusTasks>  
       <Links>  
          <Link  
             ID=?GUID?  
             Title="Displayed text of the link"  
             Description="A very short description"  
             ShellExecPath="Path to the application or URL"/>  
       </Links>  
    </Tasks>  
    ```  
  
     位置：  
  
    |属性|描述|  
    |---------------|-----------------|  
    |名称（任务）|为的任务列表中显示的名称。 如果您创建嵌入的资源文件时，此属性的价值是字符串资源。|  
    |描述（任务）|任务描述。 如果您创建嵌入的资源文件时，此属性的价值是字符串资源。|  
    |id（任务）|任务的标识符。 此标识符必须 GUID。 创建的新 GUID **exe**任务中，但对于**全球**任务中，你可以使用定义子的选项卡的任务窗格的任务时创建了 GUID。有关创建 GUID 的详细信息，请参阅[创建 Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。|  
    |图像|此字段将会忽略。|  
    |名称（操作）|显示名称的任务。|  
    |键入（操作）|描述类型的任务。 任务可以下列情况之一:-**全球**任务，**exe**，或 url 任务。 一个**全球**任务为你创建的子的选项卡中定义的任务窗格的任务时相同全球任务。有关创建可用于全球任务在子的选项卡这两个任务窗格中，在主页上的获取启动任务或常见任务列表的详细信息，请参阅支持类别 œCreating？在到 œHow：创建子的选项卡？[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。 **Exe**任务可用于运行来自启动任务的应用程序或列出了常见任务。|  
    |exelocation|关联的任务的应用程序的路径。 此特性仅用于**exe**任务。|  
    |replaceid|将替换为此任务中的任务的标识符。|  
    |组件|它提供类实现快速状态查询装配程序集名称。 需要位于计划 files\ windows server\bin\\ 集。|  
    |类|此类名称实现查询快速状态。 需要实现此类**ITaskStatusQuery**界面。|  
    |标题（链接）|所显示的链接文本。 如果您创建嵌入的资源文件时，此属性的价值是字符串资源。|  
    |描述（链接）|链接目标的说明。 如果您创建嵌入的资源文件时，此属性的价值是字符串资源。|  
    |ShellExecPath|应用程序或 URL 的路径。<br /><br /> **注意：**环境变量 ShellExecPath 属性中受支持。|  
  
     下面的示例代码显示定义链接向应用程序的方法：  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     下面的示例代码显示定义到网页上某个链接的方法：  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  更改特性值代表你的任务或链接。  
  
9. 在**Solution Explorer**，右键单击**OEMHomePageContent.home**，然后单击**属性**。  在**属性**窗格下**版本操作**、选择**Embedded 资源**。  
  
10. 保存 OEMHomePageContent.home 文件。  
  
 有关如何实现快速状态查询，请参考文档和中的示例[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>更改设置/加载项任务的状态  
 在设置和加载项中列出的任务可切换的状态中完成（配置 Add-ins），并且不完成（未配置为 Add-ins）。  
  
 定义与您的新任务关联的应用程序时，你可以使用 SetTaskStatus 付款方式 Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper 命名空间（包括中，但不是记录在 Windows Server 解决方案 SDK）来更改任务的状态。 例如，你可以更改复选标记灰色从回绿色通过调用 TaskStatus.Complete 枚举值 SetTaskStatus (SetTaskStatus (id、TaskStatus.Complete)，其中**id**任务的标识符)。 可以使用枚举值都是 TaskStatus.Complete、TaskStatus.Incomplete 或 TaskStatus.Hidden。  
  
##### <a name="replace-tasks"></a>更换任务  
 可替换加上的任务定义 replaceid 特性任务 GUID 预定义的启动任务或常见任务列表中的任务。 下表列出了这些任务，并可以更换仪表板中的相应标识符：  
  
|任务名称|标识符|  
|---------------|----------------|  
|获取其他 Microsoft 产品的更新|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|设置服务器备份|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|随时随地访问设置|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|设置电子邮件通知|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|添加用户帐户|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|添加服务器文件夹|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|随时随地访问-快速状态|6093B462-1F04-4212-8804-9BC823070FAD|  
|服务器备份-快速状态|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|服务器文件夹-快速状态|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|活动的用户帐户-快速状态|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|通过电子邮件发送通知的通知-快速状态|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|计算机-快速状态|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>（可选）创建的资源文件  
 如果想要进行本地化您添加到获取启动任务、常见任务，并社区链接的任务文本必须创建包含.home 文件集和。定义的文本字符串 home.resx 文件。  
  
###### <a name="to-create-the-resource-file"></a>若要创建的资源文件  
  
1.  右键单击你为你的任务创建该项目，请单击**添加**，然后单击**新项目**。  
  
2.  在**模板**窗格中，单击**的资源文件**，类型**OEMHomePageContent.home.resx**中**名称**框中，，然后单击**添加**。  
  
    > [!NOTE]
    >  资源文件可获得任何名，只要有。home.resx 扩展。  
  
3.  对于每个任务或您添加的链接，你必须到匹配 OEMHomePageContent.home 文件中定义的任务和链接元素 OEMHomePageContent.home.resx 文件添加字符串和值。 下面的示例代码显示的资源文件结构 Tasks.xml 文件的示例：  
  
    ```  
  
    <Tasks version=?2.0? xmlns=?https://schemas.microsoft.com/WindowsServerSolutions/2010/01/Dashboard>  
       <SetupMyServerTasks>  
          <Task name="MyTask"  
             description="MyDescription"  
             id="GUID">  
             <Action  
             name="MyActionname"  
             image="IconForAction"  
             type="TaskType"  
             exelocation="ActionExeLocation" />  
          </Task>  
       </SetupMyServerTasks>  
    </Tasks>  
  
    ```  
  
    > [!NOTE]
    >  在适用于本地化的特性标识符无法包含空格。  
  
4.  添加到相应的值.resx 文件 MyTask、MyTaskDescription、MyActionName 和 IconForAction 资源名称。  
  
5.  保存 OEMHomePageContent.home.resx 文件，然后生成解决方案。  
  
#####  <a name="BKMK_SignAssembly"></a>登录验证码签名的装配  
 你必须为其用于操作系统中的验证码登录集。 有关集签名的详细信息，请参阅[签名和的验证码检查代码](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
##### <a name="install-the-task-files"></a>安装任务文件  
 创建.home 后并。home.resx 文件，你必须安装它们在服务器上。  
  
###### <a name="to-install-the-task-files"></a>若要安装任务文件  
  
1.  确保你解决方案版本不会出错。  
  
2.  如果你未创建嵌入的资源文件，复制到 OEMHomePageContent.home 文件**%ProgramFiles%\ Windows Server \Bin\Addins\Home**服务器上。 如果你创建嵌入的资源文件，复制到 OEMHomePageContent.dll 文件**%ProgramFiles%\ Windows Server \Bin\Addins\Home**服务器上。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)