---
title: 重新生成 Tokens.dat 文件
description: 排查 Windows 激活问题时如何重新生成 Tokens.dat 文件
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 8a5835cd601b2eb327c8605d70bf075e6c8e8414
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71962993"
---
# <a name="rebuild-the-tokensdat-file"></a>重新生成 Tokens.dat 文件

解决 Windows 激活问题时，可能需要重新生成 Tokens.dat 文件。 本文详细介绍如何完成此操作。

## <a name="resolution"></a>分辨率

若要重新生成 Tokens.dat 文件，请执行以下步骤：

1. 打开提升的命令提示符窗口：  
   **对于 Windows 10**

   1. 打开“开始”菜单，然后输入cmd   。
   1. 在搜索结果中，右键单击“命令提示符”，然后选择“以管理员身份运行”   。  

   **对于 Windows 8.1**
   1. 从屏幕右边缘轻扫，然后点击“搜索”  。 或者，如果使用鼠标，请指向屏幕右下角，然后选择“搜索”  。
   1. 在搜索框中，输入 cmd  。
   1. 轻扫或右键单击显示的“命令提示符”图标  。
   1. 点击或单击“以管理员身份运行”  。

   **对于 Windows 7**
   1. 打开“开始”菜单，然后输入cmd   。
   1. 在搜索结果中，右键单击“cmd.exe”，然后选择“以管理员身份运行”   。

1. 输入适用于你的操作系统的命令列表。  

   对于 Windows 10、Windows Server 2016 及更高版本的 Windows，按顺序输入以下命令：
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   对于 Windows 8.1、Windows Server 2012 和 Windows Server 2012 R2，按顺序输入以下命令：
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   对于 Windows 7、Windows Server 2008 和 Windows Server 2008 R2，按顺序输入以下命令：
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. 重新启动计算机。

## <a name="more-information"></a>详细信息

重新生成 Tokens.dat 文件后，必须使用以下方法之一重新安装产品密钥：

- 在同一提升的提示符命令下，键入以下命令，然后按 Enter：

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > 请勿使用 /upk 开关来卸载产品密钥  。 若要在现有产品密钥上安装产品密钥，请使用 /ipk 开关  。
- 右键单击“我的计算机”，选择“属性”，然后选择“更改产品密钥”    。

有关 KMS 客户端安装密钥的详细信息，请参阅 [KMS 客户端安装密钥](kmsclientkeys.md)。
