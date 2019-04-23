---
title: AD FS 帮助诊断分析器
description: 本文档介绍了 AD FS 帮助诊断分析器和如何执行基本检查使用 AD FS 诊断 PowerShell 模块。
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5b5f895a0575094f8f1af950bde82e1d56325b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829118"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>AD FS 帮助诊断分析器

AD FS 具有大量支持各种各样的功能，它提供用于身份验证和应用程序开发的设置。 在疑难解答期间，建议以确保所有 AD FS 设置正确配置。 手动检查这些设置有时会耗费大量时间。 AD FS 帮助诊断分析器可帮助执行基本检查使用 ADFSToolbox PowerShell 模块。 后执行检查，AD FS 帮助提供[诊断分析器](https://aka.ms/adfsdiagnosticsanalyzer)以帮助您轻松地可视化结果并提供补救步骤。

完成操作采用 3 个简单步骤：

1. 安装程序的主 AD FS 服务器或 WAP 服务器上的 ADFSToolbox 模块
2. 执行诊断，并将该文件上传到 AD FS 帮助
3. 查看诊断分析并解决任何问题

转到[AD FS 帮助诊断分析器 (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer)开始进行故障排除。

![AD FS 帮助 AD FS 诊断分析器工具](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>第 1 步：安装 AD FS 服务器上的 ADFSToolbox 模块

若要运行[诊断分析器](https://aka.ms/adfsdiagnosticsanalyzer)，必须安装 ADFSToolbox PowerShell 模块。 如果 AD FS 服务器已连接到 internet，您可以直接从 PowerShell 库安装 ADFSToolbox 模块。 如果没有连接到 internet，克隆的 GitHub 存储库的手动安装。 

![AD FS 诊断分析器的安装程序](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>使用 PowerShell 库设置

如果 AD FS 服务器具有 internet 连接，建议直接从 PowerShell 库使用下面提供的 PowerShell 命令安装 ADFSToolbox 模块。
 
   ```powershell 
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>通过克隆存储库手动设置

ADFSToolbox 模块可以手动安装从 GitHub 直接。 请按照下面的说明的克隆存储库并在 AD FS 服务器上安装 ADFSToolbox 模块。

1. 下载[存储库](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. 解压缩下载的文件并将 adfsToolbox master 文件夹复制到 %SYSTEMDRIVE%\\Program Files\\WindowsPowerShell\\模块\\。
3. 导入 PowerShell 模块。 在提升的 PowerShell 窗口，运行以下命令：
 
   ```powershell 
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>步骤 2：执行诊断，并将该文件上传到 AD FS 帮助

单个命令可用于方便地跨场中的所有 AD FS 服务器执行诊断测试。 PowerShell 模块将使用远程 PS 会话在不同的服务器场中执行的诊断测试。

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

在 Server 2016 AD FS 场中，该命令将从 AD FS 配置中读取 AD FS 服务器的列表。 针对列表中的每个服务器然后尝试诊断测试。 如果 AD FS 服务器的列表将不可用 (示例 2012R2)，然后针对本地计算机运行测试。 若要指定对其的测试都要执行的服务器的列表，请使用**adfsServers**参数，以提供服务器的列表。 下面提供了一个示例

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

结果是相同的目录中创建运行命令的 JSON 文件。 该文件的名称是 ADFSDiagnosticsFile-\<时间戳\>。 示例文件名称是 ADFSDiagnosticsFile 20180716124030。

在第 2 步上[ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer)使用文件浏览器来选择要上传的结果文件。

![AD FS 诊断分析器工具的执行，并将结果上传](media/ad-fs-diagonostics-analyzer/step2.png)

单击**上传**完成上传并将移动到下一步。

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>步骤 3:查看诊断分析并解决任何问题

有四个部分的测试结果：

1. 失败：本部分包含失败的测试的列表。 每个结果组成：
2. 警告：本部分包含的测试具有导致了警告的列表。 这些问题将不可能会导致任何问题更广泛的刻度上的身份验证，但应该努力尽快解决。
3. 不适用：本部分包含不执行，因为它们不是适用于特定的服务器在其执行该命令的测试的列表。
4. 传递：本部分包含传递，并具有用户没有操作项的测试的列表。

每个测试结果将显示描述测试和解决方法步骤的详细信息：

1. 测试名称：已执行的测试的名称
2. 详细信息：在测试期间执行的整体操作的说明
3. 解决方法步骤:建议的步骤来解决此问题由测试突出显示

![AD FS 诊断分析器工具-测试结果列表](media/ad-fs-diagonostics-analyzer/step3a.png)

![AD FS 诊断分析器工具-失败解决方法](media/ad-fs-diagonostics-analyzer/step3b.png)

## <a name="next"></a>Next

- [使用 AD FS 帮助 troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)

 
