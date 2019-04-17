---
title: "自动在安装过程中安装的加载项"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>自动在安装过程中安装的加载项

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_AddIns"></a>在设置期间自动安装的加载项  
 若要在安装过程中安装的加载项，请使用 PostIC.cmd 方法，述[创建运行文章初始配置任务 PostIC.cmd 文件](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md)本文档部分。  
  
 添加到你 PostIC.cmd 以下项：  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 外接程序，现在支持预安装的和自定义卸载步骤。  
  
 安装所有之前运行预安装步骤**.msi** addin.xml 中指定的文件。 以交互模式运行，进度对话框将显示，但而无需更改进度。 在预安装阶段，已禁用取消按钮。 若要实现预安装步骤，添加 （直接下包） addin.xml 以下内容：  
  
> [!NOTE]
>  Xml 方案需要是完全遵循以下所示：  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>  
<Executable>exefile</Executable>  
<NormalArgs>args-for-interactive-mode</NormalArgs>  
<SilentArgs>args-for-silent-mode</SilentArgs>  
<IgnoreExitCode>true</IgnoreExitCode>  
  </Preinstall>  
  <UninstallConfirm>...</UninstallConfirm>      
</Package>  
<¦>  
<¦>  
```  
  
 Wherein **exefile**是可执行文件中外接程序包执行预安装步骤中，并且必须指定。 **NormalArgs**指定使用命令行时交互式型传递到 exefile 参数。 在此模式中，exefile 可以弹出窗口的用户交互某些对话框。 **SilentArgs**指定使用命令行，当静默型传递到 exefile 参数 (的 q 指定调用 installaddin.exe 时)。 Exefile 应不弹出在此模式中的任何 windows。 如果**IgnoreExitCode**预安装的步骤始终被认为是成功，否则为退出音 0 指示成功、 1 表示取消，和其他值表示故障与 true，指定。 标记**NormalArgs**， **SilentArgs**，并**IgnoreExitCode**都是可选的。  
  
 自定义的卸载步骤可用于以下任一操作：  
  
-   更换内置确认对话框。  
  
-   填充自定义之前卸载的对话框。  
  
-   执行某些在卸载之前的任务。  
  
 若要实现卸载步骤，添加 （直接下包） addin.xml 以下内容：  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>¦</Preinstall>  
<UninstallConfirm>  
<Executable>full-path-to-exefile</Executable>  
<Arguments>command-line-arguments</Arguments>  
</UninstallConfirm>  
</Package>  
```  
  
 Wherein**全屏 exefile 路径**指定 exefile 系统上已安装。 **参数**是可选的并指定 exefile 的命令行参数。 之前内置卸载确认调用 exefile 对话框弹出。  
  
 Exefile 可以执行下列在此阶段任务：  
  
-   弹出用户交互的某些对话框。  
  
-   执行一些后台任务。  
  
 此 exe 文件退出代码确定卸载过程向前移动的方式：  
  
-   0: 卸载过程在继续进行而又不想内置确认对话框中，用户是否已确认一样。 （这种方法可用于替换内置确认对话框）;  
  
-   1： 取消卸载进程，并为最后向用户显示已取消的消息。 所有内容将保持不变。  
  
-   其他： 卸载过程继续内置确认对话框中，就像自定义的卸载步骤不存在。  
  
 调用 exefile 任何故障会导致相同的行为，因为 exefile 返回 0 或 1 以外的代码。  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)