---
title: 文件服务器资源管理器命令行工具
description: 本文介绍 Windows Server 2016 的命令行工具
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9b31c133b0ee4382b5b9aeded9b3852c7230d2d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858438"
---
# <a name="file-server-resource-manager-command-line-tools"></a>文件服务器资源管理器命令行工具

> 适用于：Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

文件服务器资源管理器安装[文件服务器资源管理器](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager) PowerShell cmdlet，及以下命令行工具：

-   **Dirquota.exe**。 用于创建和管理配额、自动应用配额和配额模板。
-   **Filescrn.exe**。 用于创建和管理文件屏蔽、文件屏蔽模板、文件屏蔽例外和文件组。
-   **Storrept.exe**。 用于配置报告参数并根据需要生成存储报告。 还可以用于创建报告任务，可以通过使用 **schtasks.exe** 计划使用此功能。

可以使用这些工具管理本地计算机或远程计算机上的存储资源。 有关这些命令行工具的详细信息，请参阅下列参考资料：

-   **Dirquota**: <https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**: <https://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**: <https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> 若要查看命令语法和可用的命令参数，请运行带有 <strong>/？</strong> 参数的 参数。


## <a name="remote-management-using-the-command-line-tools"></a>使用命令行工具进行远程管理

每个工具都提供若干种选项，用于执行于文件服务器资源管理器 MMC 管理单元中的可用操作类似的操作。 如果希望命令远程计算机（而不是本地计算机）执行操作，请使用 **/remote**:*ComputerName* 参数。

例如，**Dirquota.exe** 包括 **template export** 参数，用于将配额模板设置写入 XML 文件；以及 **template import** 参数，用于从 XML 文件导入模板设置。 将 **/remote**:*ComputerName* 参数添加至 **Dirquota.exe template import** 命令，此操作旨在将本地计算机上 XML 文件中的模板导入远程计算机。

> [!Note]
> 如果通过 **/remote**:<em>ComputerName</em> 参数运行命令行工具，从而在远程计算机上执行模板导出（或导入），则模板将被写入本地计算机上的 XML 文件或从本地计算机上的 XML 文件中复制模板。

<br />

## <a name="additional-considerations"></a>其他注意事项 

若要通过命令行工具管理远程资源：

-   必须使用属于本地计算机和远程计算机上的**管理员**组成员的域帐户登录。
-   必须从提升的“命令提示符”窗口运行命令行工具。 若要打开提升的命令提示符窗口，请单击“开始”、指向“所有程序”、单击“附件”、右键单击“命令提示符”，然后单击“以管理员身份运行”。
-   远程计算机必须正在运行 Windows Server，并且必须安装文件服务器资源管理器。
-   远程计算机上的**远程文件服务器资源管理器管理**例外必须处于启用状态。 通过使用“控制面板”中的 Windows 防火墙启用此例外。


## <a name="see-also"></a>请参阅

-   [管理远程存储资源](managing-remote-storage-resources.md)