---
title: 向“设置”添加选项卡
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aac6b7f3-9020-46c3-a83f-b81542300385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9eaa1aa5a9c5e8d4c2e36f2000e0adecc83245d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854978"
---
# <a name="add-a-tab-to-settings"></a>向“设置”添加选项卡

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你可以向仪表板中的“设置”添加选项卡，方法是创建并安装代码程序集，该程序集由操作系统中的“设置管理器”使用。  
  
## <a name="add-a-tab-to-settings"></a>向“设置”添加选项卡  
 你可以通过执行以下任务向“设置”添加选项卡：  
  
-   [向程序集添加 ISettingsData 接口的实现](Add-a-Tab-to-Settings.md#BKMK_ISettingsData)。  
  
-   [使用验证码签名对程序集签名](Add-a-Tab-to-Settings.md#BKMK_SignAssembly)。  
  
-   [在引用计算机上安装程序集](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly)。  
  
###  <a name="BKMK_ISettingsData"></a> 对程序集添加 ISettingsData 接口的实现  
 ISettingsData 接口包含在 AdminCommon.dll 程序集的 Microsoft.WindowsServerSolutions.Settings 命名空间中，该程序集位于 \Program Files\Windows Server\Bin。  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>向程序集添加 ISettingsData 代码  
  
1.  通过在 **“开始”** 菜单中右键单击 Visual Studio 2010 并选择 **“以管理员身份运行”**，以管理员身份打开该程序。  
  
2.  依次单击 **“文件”**、 **“新建”** 和 **“项目”**。  
  
3.  在 **“新建项目”** 对话框中，单击 **“Visual C#”** 和 **“类库”**，输入解决方案名称 **DashboardSettingsPage**，然后单击 **“确定”**。  
  
    > [!IMPORTANT]
    >  安装在服务器上的程序集必须命名为 DashboardSettingsPage.dll，然后将该 dll 复制到 %ProgramFiles%\Windows Server\Bin\OEM。  
  
4.  创建希望在选项卡中使用的控件。在此示例中，设置控件被命名为 MySettingsControl。  
  
5.  重命名 Class1.cs 文件。 例如，MySettingTab.cs。  
  
6.  添加对 AdminCommon.dll 文件的引用。  
  
7.  使用语句添加以下内容：  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  根据以下示例更改命名空间和类头：  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. 实例化为选项卡创建的控件实例。例如：  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. 添加类的构造函数。 下面的代码示例演示了此构造函数：  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. 添加用于提交设置更改的 Commit 方法。 下面的代码示例显示了 Commit 方法：  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. 添加用于标识选项卡控件的 TabControl 方法。下面的代码示例显示了 TabControl 方法：  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. 添加用于为选项卡提供唯一标识符的 TabId 方法。下面的代码示例显示了 TabId 方法：  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. 添加用于返回选项卡顺序的 TabOrder 方法。下面的代码示例显示了 TabOrder 方法：  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  选项卡顺序使用从 0 开始的数字进行定义。 首先显示 Microsoft 内置设置选项卡，然后按你定义的选项卡顺序显示你的选项卡。 例如，如果有三个设置选项卡，则根据需要的选项卡显示顺序指定选项卡顺序 0、1 和 2。  
  
15. 添加用于提供选项卡标题的 TabTitle 方法。下面的代码示例显示了 TabTitle 方法：  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  标题文本也可以来自资源文件，以适应本地化需求。  
  
16. 保存并生成解决方案。  
  
###  <a name="BKMK_SignAssembly"></a> 使用验证码签名程序集签名  
 你必须使用验证码签名进行程序集签名，因为该签名将在操作系统中使用。 有关对程序集签名的详细信息，请参阅 [Signing and Checking Code with Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)（使用验证码对代码进行签名和检查）。  
  
###  <a name="BKMK_InstallAssembly"></a> 在引用计算机上安装该程序集  
 成功生成解决方案后，请在引用计算机上的以下文件夹中放入 DashboardSettingsPage.dll 文件的副本：  
  
 **%Programfiles%\Windows Server\Bin\OEM**  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [部署准备的映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)