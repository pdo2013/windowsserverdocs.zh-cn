---
title: 在 Windows Server 上安装 Hyper-v 角色
description: 提供有关使用服务器管理器或 Windows PowerShell 安装 Hyper-v 的说明
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: 2687a907852e2a81f03b147df1425cd01b34fb76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392808"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>在 Windows Server 上安装 Hyper-v 角色

>适用于：Windows Server 2016、Windows Server 2019
  
若要创建和运行虚拟机，请在 Windows Server 上安装 Hyper-v 角色，方法是使用服务器管理器或在 Windows PowerShell 中使用**add-windowsfeature** cmdlet。 对于 Windows 10，请参阅[在 windows 10 上安装 hyper-v](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)。

若要了解有关 Hyper-v 的详细信息，请参阅[Hyper-v 技术概述](../Hyper-V-Technology-Overview.md)。 若要试用 Windows Server 2019，你可以下载并安装评估副本。 请参阅[评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)。

在安装 Windows Server 或添加 Hyper-v 角色之前，请确保：
- 计算机硬件兼容。 有关详细信息，请参阅 windows server 上的[系统要求](../../../get-started/System-Requirements.md)和 windows [Server 上的 hyper-v 系统要求](../System-requirements-for-Hyper-V-on-Windows.md)。
- 不打算使用依赖于 Hyper-v 所需的相同处理器功能的第三方虚拟化应用程序。 示例包括 VMWare Workstation 和 VirtualBox。 你可以在不卸载这些其他应用的情况下安装 Hyper-v。 但是，如果你在运行 Hyper-v 虚拟机监控程序时尝试使用它们来管理虚拟机，则虚拟机可能无法启动或运行 unreliably。 有关关闭 Hyper-v 虚拟机监控程序的详细信息和说明，请参阅[虚拟化应用程序不能与 hyper-v、Device Guard 和 Credential guard 一起](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g)使用。

如果要仅安装管理工具（如 Hyper-v 管理器），请参阅[通过 Hyper-v 管理器远程管理 hyper-v 主机](../Manage/Remotely-manage-Hyper-V-hosts.md)。
  
## <a name="install-hyper-v-by-using-server-manager"></a>使用服务器管理器安装 Hyper-v  
  
1. 在“服务器管理器”中的“管理”菜单上，单击“添加角色和功能”。  
  
2. 在 **“开始之前”** 页面上，确定目标服务器和网络环境已为要安装的角色和功能做好准备。 单击“下一步”。  
  
3. 在“选择安装类型”页上，选择“基于角色或功能的安装”，然后单击“下一步”。  
  
4. 在“选择目标服务器”页上，从服务器池中选择一台服务器，然后单击“下一步”。  
  
5. 在“选择服务器角色”页上，选择 Hyper-V。  
  
6. 若要添加用于创建和管理虚拟机的工具，请单击“添加功能”。 在“功能”页上，单击“下一步”。  
  
7. 在“创建虚拟交换机”页、“虚拟机迁移”页和“默认存储”页上，选择相应的选项。  
  
8. 在“确认安装选择”页上，选择“如果需要，自动重新启动目标服务器”，然后单击“安装”。  
  
9. 安装完成后，请验证是否正确安装了 Hyper-v。 在服务器管理器中打开 "**所有服务器**" 页，然后选择安装了 hyper-v 的服务器。 在选定服务器的页面上检查 "**角色和功能**" 磁贴。  
  
## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>使用 Add-windowsfeature cmdlet 安装 Hyper-v  
  
1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  
  
2. 右键单击 "Windows PowerShell" 并选择 "以**管理员身份运行**"。  
  
3. 若要在您远程连接到的服务器上安装 Hyper-v，请运行以下命令，并将 `<computer_name>` 替换为服务器的名称。  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    如果以本地方式连接到服务器，请运行命令，但不 `-ComputerName <computer_name>`。  
  
4. 服务器重新启动后，你可以看到已安装了 Hyper-v 角色，并通过运行以下命令来查看安装了哪些其他角色和功能：  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    如果以本地方式连接到服务器，请运行命令，但不 `-ComputerName <computer_name>`。  
  
> [!NOTE]  
> 如果在运行 Windows Server 2016 的服务器核心安装选项的服务器上安装此角色，并使用参数 `-IncludeManagementTools`，则只会安装 Windows PowerShell 的 Hyper-v 模块。 你可以在另一台计算机上使用 GUI 管理工具 Hyper-v 管理器来远程管理在服务器核心安装上运行的 Hyper-v 主机。 有关远程连接的说明，请参阅[通过 Hyper-v 管理器远程管理 hyper-v 主机](../Manage/Remotely-manage-Hyper-V-hosts.md)。  
  
## <a name="see-also"></a>请参阅  
  
- [Add-windowsfeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
