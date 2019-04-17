---
title: "自定义共享的文件夹"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>自定义共享的文件夹

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

默认情况下上最大的数据分区 0 磁盘上, 创建服务器文件夹。 合作伙伴可以自定义的位置以及指定其他服务器的文件夹，通过以下步骤：  
  
1.  使用自定义的分区配置，创建出厂图像，并使用 sysprep 之前创建新的存储注册表项。 在初始配置（集成电路），存储集成电路任务上检查此注册表项。 如果存在，默认服务器文件夹 C:\ServerFolders 目录中创建。  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>若要创建新的存储注册表项  
  
    1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
    2.  在搜索框中，键入**regedit**，然后单击**Regedit**应用程序。  
  
    3.  在导航窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，然后展开**Microsoft**。  
  
    4.  右键单击**Windows Server**，单击**新建**，然后单击**键**。  
  
    5.  密钥命名**存储**。  
  
    6.  在导航窗格中，右键单击新存储注册表项，请单击**新建**，然后单击**DWORD（32 位）值**。  
  
    7.  命名字符串**CreateFoldersOnSystem**。  
  
    8.  右键单击**CreateFoldersOnSystem**，然后单击**修改**。 **编辑字符串**显示对话框。  
  
    9. 将此新关键值设置**1**，然后单击**确定**。  
  
2.  若要将文件夹移到另一个位置，或创建其他文件夹，请使用 PostIC.cmd 脚本。 请参阅下面的示例：[示例 1 部分：创建自定义的文件夹，然后将默认文件夹从移动到新位置 PostIC.cmd 通过使用 Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1)。  
  
3.  若要将文件夹移到另一个位置，或创建其他文件夹，请使用 Windows Server 解决方案 SDK。 请参阅下面的示例：[示例 2 部分：创建自定义文件夹并使用 Windows Server 解决方案 SDK 移动现有文件夹](Customize-Shared-Folders.md#BKMK_Example2)。  
  
 （可选）合作伙伴可以保留数据文件夹上驱动器 c。这样的最终用户或经销商，以确定数据驱动器上的数据文件夹中的布局。  
  
###  <a name="BKMK_Example1"></a>示例 1：创建自定义的文件夹，然后将默认文件夹从移动到新位置 PostIC.cmd 通过使用 Windows PowerShell  
  
1.  创建详见运行发布初始配置任务 PostIC.cmd 文件[为运行文章初始配置任务创建 PostIC.cmd 文件](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)部分。  
  
2.  使用记事本，创建一个名为文件**customizefolders.ps1**中 C:\Windows\Setup\Scripts 文件夹，然后粘贴以下 Windows PowerShell® 命令到该文件（具体取决于所需的行为相应行取消标记）。  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  添加到运行此脚本 PostIC.cmd 文件以下行。  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>示例 2：创建自定义文件夹并使用 Windows Server 解决方案 SDK 移动现有文件夹  
 你创建的代码可以会编译为可执行文件，然后从 PostIC.cmd 文件称为或称为直接从已安装的加载项。  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)