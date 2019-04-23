---
title: 向“设置”、“加载项”、“快速状态”和“帮助链接”添加条目
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864388"
---
# <a name="add-entries-to-setup-add-ins-quick-status-and-help-links"></a>向“设置”、“加载项”、“快速状态”和“帮助链接”添加条目

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以向 **“设置”**、 **“加载项”**、 **“快速状态”** 任务列表中添加任务，并且你可以向仪表板主页中的“社区链接”部分添加链接。 通过将名为 OEMHomePageContent.home 的 XML 文件或名为 OEMHomePageContent.dll 的嵌入式资源文件放置在 %ProgramFiles%\Windows Server\Bin\Addins\Home 中，可以将任务和链接添加到这些列表和部分。 嵌入式资源文件可以用于对所添加的任务和链接中的文本进行本地化。 .home 文件包含任务和链接的 XML 定义。  
  
## <a name="adding-tasks-to-the-setup-add-ins-quick-status-task-lists-and-adding-links-to-help-task"></a>向“设置”、“加载项”、“快速状态”任务列表添加任务，并向“帮助”任务添加链接  
 通过使用 XML 定义任务和链接，你可以向 **“设置”**、 **“加载项”**、 **“快速状态”** 任务列表添加任务，以及向 **“帮助”** 任务添加链接，还可以选择性地创建嵌入式资源文件，以及在服务器上安装文件。 如果在不使用资源文件的情况下将 XML 文件安装在服务器上，则它必须命名为 OEMHomePageContent.home。 如果使用程序集安装 XML 文件和资源文件，则它必须命名为 OEMHomePageContent.dll 并且必须进行验证码签名。  
  
### <a name="define-the-tasks-and-links"></a>定义任务和链接  
 你可以使用文本编辑器（例如记事本）创建 .home 文件，或者如果你还要创建嵌入式资源文件，则可以使用 Visual Studio 2010 或更高版本来定义这些文件。 以下过程说明如何使用 Visual Studio 2010 或更高版本创建文件。  
  
##### <a name="to-define-the-tasks-and-links"></a>定义任务和链接  
  
1.  通过在“开始”菜单中右键单击 Visual Studio 2010 或更高版本并选择 **“以管理员身份运行”**，以管理员身份打开该程序。  
  
2.  依次单击 **“文件”**、 **“新建”** 和 **“项目”**。  
  
3.  在 **“模板”** 窗格中，单击 **“类库”**，在 **“名称”** 框中输入 **OEMHomePageContent** ，然后单击 **“确定”**。  
  
4.  删除 Class1.cs 文件。  
  
5.  右键单击此新项目，单击 **“添加”**，然后单击 **“新项目”**。  
  
6.  在 **“模板”** 窗格中，单击 **“XML 文件”**，在 **“名称”** 框中键入 **OEMHomePageContent.home** ，然后单击 **“添加”**。  
  
    > [!NOTE]
    >  如果在不使用资源文件的情况下安装 XML 文件，则它必须命名为 OEMHomePageContent.home。 如果它包含在程序集中，则可以向它提供扩展名为 .home 的任何名称。  
  
7.  将下列 XML 代码添加到 OEMHomePageContent.home 文件中：  
  
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
  
     其中：  
  
    |特性|描述|  
    |---------------|-----------------|  
    |名称（任务）|列表中任务的显示名称。 如果要创建嵌入式资源文件，则此属性的值为字符串资源。|  
    |说明（任务）|任务的说明。 如果要创建嵌入式资源文件，则此属性的值为字符串资源。|  
    |ID（任务）|任务的标识符。 此标识符必须是 GUID。 你可以为 **exe** 任务创建新的 GUID，但是对于**全局**任务，你仅可以使用在为子选项卡的任务窗格定义任务时所创建的 GUID。有关创建 GUID 的详细信息，请参阅 [创建 GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098)。|  
    |image|将忽略此字段。|  
    |名称（操作）|显示任务的名称。|  
    |类型（操作）|描述任务的类型。 任务可以是以下任务之一：**全局**任务、**exe** 或 url 任务。 **全局** 任务与你在为子选项卡的任务窗格定义任务时所创建的全局任务相同。有关创建子选项卡的任务窗格和在主页的开始任务或常见任务列表中可用的全局任务的详细信息，请参阅支持类 œCreating？在为 œ：创建子选项卡？[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。 **exe** 任务可用于从“开始任务”或“常见任务”列表运行应用程序。|  
    |exelocation|与任务相关联的应用程序的路径。 此属性仅供 **exe** 任务使用。|  
    |replaceid|替换为此任务的任务标识符。|  
    |程序集|提供类以实现快速状态查询的程序集的 AssemblyName。 程序集需要位于 Program files\ windows server\bin\\。|  
    |类|实现快速状态查询的类的名称。 该类需要实现 **ITaskStatusQuery** 接口。|  
    |标题（链接）|链接的显示文本。 如果要创建嵌入式资源文件，则此属性的值为字符串资源。|  
    |说明（链接）|链接目标的说明。 如果要创建嵌入式资源文件，则此属性的值为字符串资源。|  
    |ShellExecPath|应用程序或 URL 的路径。<br /><br /> **注意：** ShellExecPath 属性支持环境变量。|  
  
     以下代码示例说明如何定义指向应用程序的链接：  
  
    ```  
    <Links>  
       <Link Title="Calc" Description="Launches Calc" ShellExecPath="%windir%\system32\calc.exe" />  
    </Links>  
    ```  
  
     以下代码示例说明如何定义指向网页的链接：  
  
    ```  
    <Links>  
       <Link Title="Browser" Description="Open browser" ShellExecPath="http://www.adventureworks.com/" />  
    </Links>  
    ```  
  
8.  更改表示任务或链接的属性值。  
  
9. 在 **“解决方案资源管理器”** 中，右键单击 **“OEMHomePageContent.home”**，然后单击 **“属性”**。  在 **“属性”** 窗格中的 **“生成操作”** 下，选择 **“嵌入的资源”**。  
  
10. 保存 OEMHomePageContent.home 文件。  
  
 有关如何实现快速状态查询，请参考 [Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)中的文档和示例。  
  
#### <a name="change-the-status-of-a-setupadd-ins-task"></a>更改“设置/加载项”任务的状态  
 “设置”和“加载项”中列出的任务可以在已完成（针对加载项配置）和未完成（不针对加载项配置）状态间切换。  
  
 定义与新任务相关联的应用程序时，你可以使用 Microsoft.WindowsServerSolutions.Administration.ObjectModel.TaskStatusHelper 命名空间的 SetTaskStatus 方法（包含在内，但未在 Windows Server Solutions SDK 中介绍）来更改任务状态。 例如，你可以通过调用带有 TaskStatus.Complete 枚举值的 SetTaskStatus 方法（SetTaskStatus(id, TaskStatus.Complete)，其中 **id** 是任务的标识符）将复选标记从灰色更改为绿色。 可以使用的枚举值包括 TaskStatus.Complete、TaskStatus.Incomplete 或 TaskStatus.Hidden。  
  
##### <a name="replace-tasks"></a>替换任务  
 你可以替换在“开始任务”或“常见任务“列表中预定义的任务，方法是在任务定义的 replaceid 属性中为此任务添加 GUID。 下表列出了可在仪表板中替换的任务和相应标识符：  
  
|任务名称|标识符|  
|---------------|----------------|  
|获取其他 Microsoft 产品的更新|8412D35A-13EE-4112-AE0B-F7DBC83EA83D|  
|设置服务器备份|F68B3F3F-19DE-499D-9ACB-4BB41B8FF420|  
|设置“随处访问”|8991302D-676A-4A7C-B244-D1E08AE0EFEA|  
|设置电子邮件警报通知|DE6F2B36-F19C-4FAF-998B-9772300E3530|  
|添加用户帐户|6D5B5D5F-2EC7-4B1F-9580-4DB084B278B1|  
|添加服务器文件夹|03F1F438-D94E-439B-A9F7-0C817C37D625|  
|随处访问 - 快速状态|6093B462-1F04-4212-8804-9BC823070FAD|  
|服务器备份 - 快速状态|156947D8-21DC-45FE-A9A8-2F80AE304191|  
|服务器文件夹 - 快速状态|C657E8AB-AC1F-4AA1-8E80-5AF6BB27C314|  
|活动用户帐户 - 快速状态|68BCB125-CE8F-467F-B65B-56AD45A614B5|  
|电子邮件警报通知 - 快速状态|75AB06E7-A679-4D62-A5EC-65362FE4F9DB|  
|计算机 - 快速状态|7966A974-D52D-4F5D-B37F-05C1B73CEEF3|  
  
##### <a name="optional-create-the-resource-file"></a>（可选）创建资源文件  
 如果你要本地化添加到“开始任务”、“常见任务”和“社区链接”中的任务的文本，则必须创建一个包含 .home 文件和 .home.resx 文件的程序集， .home.resx 文件中需定义文本字符串。  
  
###### <a name="to-create-the-resource-file"></a>创建资源文件  
  
1.  右键单击你为任务创建的项目，单击 **“添加”**，然后单击 **“新项目”**。  
  
2.  在 **“模板”** 窗格中，单击 **“资源文件”**，在 **“名称”** 框中输入 **OEMHomePageContent.home.resx**，然后单击 **“添加”**。  
  
    > [!NOTE]
    >  可以为该资源文件提供任意扩展名为 .home.resx 的文件名。  
  
3.  对于添加的每个任务或链接，你必须将与在 OEMHomePageContent.home 文件中定义的任务和链接元素相匹配的字符串和值添加到 OEMHomePageContent.home.resx 文件中。 以下代码示例显示了针对该资源文件进行结构化的 Tasks.xml 文件的示例：  
  
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
    >  属性中用于本地化的标识符不能包含空格。  
  
4.  将 MyTask、MyTaskDescription、MyActionName 和 IconForAction 资源名称及相应的值添加到 .resx 文件中。  
  
5.  保存 OEMHomePageContent.home.resx 文件，然后生成解决方案。  
  
#####  <a name="BKMK_SignAssembly"></a> 使用验证码签名程序集签名  
 你必须使用验证码签名进行程序集签名，因为该签名将在操作系统中使用。 有关对程序集签名的详细信息，请参阅 [Signing and Checking Code with Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)（使用验证码对代码进行签名和检查）。  
  
##### <a name="install-the-task-files"></a>安装任务文件  
 创建 .home 和 .home.resx 文件后，你必须在服务器上安装这两个文件。  
  
###### <a name="to-install-the-task-files"></a>安装任务文件  
  
1.  确保正确生成解决方案。  
  
2.  如果尚未创建嵌入式资源文件，则将 OEMHomePageContent.home 文件复制到服务器上的 **%ProgramFiles%\Windows Server\Bin\Addins\Home**。 如果已创建嵌入式资源文件，则将 OEMHomePageContent.dll 文件复制到服务器上的 **%ProgramFiles%\Windows Server\Bin\Addins\Home**。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)