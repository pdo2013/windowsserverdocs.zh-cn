---
title: AD FS 帮助诊断分析器
description: 本文档介绍 AD FS 帮助诊断分析器，以及如何使用 AD FS 诊断 PowerShell 模块执行基本检查。
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5d56a84680467359b68ae1ab115801d82a34c822
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407235"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>AD FS 帮助诊断分析器

AD FS 提供了许多设置，这些设置支持它提供的用于身份验证和应用程序开发的各种功能。 在故障排除过程中，建议确保正确配置所有 AD FS 设置。 手动检查这些设置有时会耗费时间。 AD FS 帮助诊断分析器可帮助使用 ADFSToolbox PowerShell 模块执行基本检查。 执行检查后，AD FS 帮助将提供[诊断分析器](https://aka.ms/adfsdiagnosticsanalyzer)来帮助你轻松地可视化结果并提供补救步骤。

完整操作采用3个简单步骤：

1. 在主 AD FS 服务器或 WAP 服务器上设置 ADFSToolbox 模块
2. 执行诊断，并将文件上传到 AD FS 帮助
3. 查看诊断分析并解决任何问题

若要开始进行故障排除，请参阅[AD FS 帮助诊断分析器（ https://aka.ms/adfsdiagnosticsanalyzer)](https://aka.ms/adfsdiagnosticsanalyzer) 。

![AD FS 诊断分析器工具 AD FS 帮助](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>第 1 步：在 AD FS server 上设置 ADFSToolbox 模块

若要运行[诊断分析器](https://aka.ms/adfsdiagnosticsanalyzer)，必须安装 ADFSToolbox PowerShell 模块。 如果 AD FS 服务器已连接到 internet，则可以直接从 PowerShell 库安装 ADFSToolbox 模块。 如果未连接到 internet，则可以手动安装它。 

[!WARNING]
如果使用 AD FS 2.1 或更低版本，则必须安装 ADFSToolbox 版本1.0.13。 ADFSToolbox 不再支持最新版本的 AD FS 2.1 或更低版本。

![AD FS 诊断分析器-安装程序](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>使用 PowerShell 库进行安装

如果 AD FS 服务器具有 internet 连接，建议使用下面提供的 PowerShell 命令直接从 PowerShell 库安装 ADFSToolbox 模块。

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>手动设置

必须将 ADFSToolbox 模块手动复制到 AD FS 或 WAP 服务器。 按照以下说明进行操作。

1. 在可访问 internet 的计算机上启动提升的 PowerShell 窗口。
2. 安装 AD FS 工具箱 "模块。

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. 将本地计算机 @no__t 上的 ADFSToolbox 文件夹复制到 AD FS 或 WAP 计算机上的同一位置。

4. 在 AD FS 机上启动提升的 PowerShell 窗口，并运行以下 cmdlet 以导入模块。

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>步骤 2：执行诊断 cmdlet

![AD FS 诊断分析器工具-执行并上传结果](media/ad-fs-diagonostics-analyzer/step2_v2.png)

可以使用单个命令在场中的所有 AD FS 服务器上方便地执行诊断测试。 PowerShell 模块将使用远程 PS 会话在场中的不同服务器之间执行诊断测试。

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

在服务器2016或更高版本 AD FS 场中，该命令将从 AD FS 配置中读取 AD FS 服务器的列表。 然后，将针对列表中的每个服务器尝试诊断测试。 如果 AD FS 服务器的列表不可用（例如2012R2），则将针对本地计算机运行测试。 若要指定要对其执行测试的服务器的列表，请使用**ServerNames**参数提供服务器的列表。 下面提供了一个示例

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

结果是在运行命令的同一目录中创建的 JSON 文件。 该文件的名称为 AdfsDiagnosticsFile-\<timestamp @ no__t。 示例文件名为 AdfsDiagnosticsFile-07312019-184201。

## <a name="step-3-upload-the-diagnostics-file"></a>步骤 3:上载诊断文件

在上的步骤3中[https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer)使用文件浏览器选择要上传的结果文件。

单击 "上**传**" 完成上传。

通过使用 Microsoft 帐户登录，可以保存诊断结果以供以后查看，并且可以发送给 Microsoft 支持部门。 如果你随时打开支持案例，Microsoft 将能够查看诊断分析器结果，并帮助你更快地解决问题。

![AD FS 诊断分析器工具-登录](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>步骤 4：查看诊断分析并解决任何问题

测试结果有五个部分：

1. 失败：此部分包含失败的测试的列表。 每个结果包含：
2. 警告：此部分包含导致警告的测试的列表。 这些问题不会导致在更广泛的范围内进行身份验证的任何问题，但应最早地解决这些问题。
3. 过来本部分包含通过的测试列表，并没有用户的操作项。
4. 不运行：此部分包含由于缺少信息而无法运行的测试的列表。
5. 不适用：本部分包含未执行的测试列表，因为这些测试不适用于执行命令的特定服务器。

@no__t 0AD FS 诊断分析器工具-"测试结果列表 @ no__t" 显示每个测试结果，其中包含描述测试的详细信息和步骤的解决方法：

1. 测试名称：已执行的测试的名称
2. 说明：对测试的说明。
3. 详细信息：在测试过程中执行的总体操作的说明
4. 解决方法步骤:解决测试突出显示的问题的建议步骤

![AD FS 诊断分析器工具-故障解决方法](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>Next

- [使用 AD FS 帮助 troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑难解答](ad-fs-tshoot-overview.md)
