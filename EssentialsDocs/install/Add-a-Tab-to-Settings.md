---
title: "添加到设置的选项卡"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-tab-to-settings"></a>添加到设置的选项卡

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

通过创建并安装代码装配，由操作系统中的设置管理器，你可以设置仪表板上添加一个选项卡。  
  
## <a name="add-a-tab-to-settings"></a>添加到设置的选项卡  
 通过执行以下的任务将选项卡添加到设置：  
  
-   [添加到组件 ISettingsData 接口的实现](Add-a-Tab-to-Settings.md#BKMK_ISettingsData)。  
  
-   [登录的验证码签名装配](Add-a-Tab-to-Settings.md#BKMK_SignAssembly)。  
  
-   [在参考计算机上安装程序集](Add-a-Tab-to-Settings.md#BKMK_InstallAssembly)。  
  
###  <a name="BKMK_ISettingsData"></a>添加到组件 ISettingsData 接口的实现  
 ISettingsData 界面包含 AdminCommon.dll 装配位于 \Program Files\ Windows Server \Bin 的 Microsoft.WindowsServerSolutions.Settings 命名空间中。  
  
##### <a name="to-add-the-isettingsdata-code-to-the-assembly"></a>若要添加到组件 ISettingsData 代码  
  
1.  以 administrator 身份打开 Visual Studio 2010，方法是右键单击在计划**开始**菜单并选择**以管理员身份运行**。  
  
2.  单击**文件**，单击**新建**，然后单击**项目**。  
  
3.  在**新项目**对话框中，单击**C#**，单击**类库**，输入**DashboardSettingsPage**的名称，然后单击解决方案，以及**确定**。  
  
    > [!IMPORTANT]
    >  在服务器上已安装的程序集必须命名 DashboardSettingsPage.dll，并到 %ProgramFiles%\ Windows Server \Bin\OEM 复制 dll。  
  
4.  创建你想要使用的选项卡中的控件。在此示例中设置控制 MySettingsControl 命名。  
  
5.  重命名 Class1.cs 文件。 例如，MySettingTab.cs。  
  
6.  添加对 AdminCommon.dll 文件。  
  
7.  添加以下使用声明：  
  
    ```c#  
    using Microsoft.WindowsServerSolutions.Settings;  
    ```  
  
8.  更改命名空间并类标头匹配下面的示例：  
  
    ```  
  
    namespace DashboardSettingsPage  
    {  
        public class MySettingTab : ISettingsData  
        {  
        }  
    }  
  
    ```  
  
9. 实例化实例对于选项卡上创建的控件。例如：  
  
    ```c#  
    private MySettingsControl tab;  
    ```  
  
10. 添加构造类函数。 下面的示例代码显示构造函数：  
  
    ```  
  
    public MySettingTab()  
    {  
       tab = new MySettingsControl();  
    }  
    ```  
  
11. 添加提交方法，将设置更改提交。 下面的示例代码显示提交方法：  
  
    ```  
  
    void ISettingsData.Commit(bool dismissed)  
    {  
       // Implement the code that is required to submit your setting changes  
    }  
    ```  
  
12. 添加卡方法，标识该选项卡的控制。下面的示例代码显示卡方法：  
  
    ```  
  
    System.Windows.Forms.Control ISettingsData.TabControl  
    {  
       get { return tab; }  
    }  
    ```  
  
13. 添加 TabId 方法，该选项卡中提供的唯一标识符。下面的示例代码显示 TabId 方法：  
  
    ```  
  
    private Guid id = Guid.NewGuid();  
  
    Guid ISettingsData.TabId  
    {  
       get { return id; }  
    }  
    ```  
  
14. 添加 TabOrder 方法，返回的选项卡上的顺序。下面的示例代码显示 TabOrder 方法：  
  
    ```  
  
    int ISettingsData.TabOrder  
    {  
       get { return 0; }  
    }  
    ```  
  
    > [!NOTE]
    >  使用数字 0 开始定义的选项卡上的顺序。 第一次显示 Microsoft 内置设置选项卡，然后基于你所定义的选项卡上顺序显示你的选项卡。 例如，如果你有三种设置选项卡，你为 0、1 和 2 根据所需的选项卡，要显示的顺序指定的选项卡上的顺序。  
  
15. 添加 TabTitle 方法，它可以提供的选项卡上标题。下面的示例代码显示 TabTitle 方法：  
  
    ```  
  
    string ISettingsData.TabTitle  
    {  
      get { return "My Settings Tab"; }  
    }  
    ```  
  
    > [!NOTE]
    >  标题文本也可能来自要容纳需求本地化的资源文件。  
  
16. 保存和版本解决方案。  
  
###  <a name="BKMK_SignAssembly"></a>登录验证码签名的装配  
 你必须为其用于操作系统中的验证码登录集。 有关集签名的详细信息，请参阅[签名和的验证码检查代码](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode)。  
  
###  <a name="BKMK_InstallAssembly"></a>在参考计算机上安装程序集  
 成功制订解决方案后，位置以下参考计算机上的文件夹中的一份 DashboardSettingsPage.dll 文件：  
  
 **%Programfiles%\ Windows Server \Bin\OEM**  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)