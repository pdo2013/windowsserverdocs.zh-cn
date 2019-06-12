---
title: Windows Server 上安装 HYPER-V 角色
description: 提供有关如何安装 HYPER-V 的说明使用服务器管理器或 Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: 80154c569701608ad190fb76eb3737578895d187
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812377"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Windows Server 上安装 HYPER-V 角色

>适用于：Windows Server 2016 中，Windows Server 2019
  
若要创建和运行虚拟机，安装 HYPER-V 角色在 Windows Server 上使用服务器管理器或**Install-windowsfeature**在 Windows PowerShell cmdlet。 适用于 Windows 10，请参阅[Windows 10 上安装的 Hyper V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)。

若要了解有关 HYPER-V 的详细信息，请参阅[HYPER-V 技术概述](../Hyper-V-Technology-Overview.md)。 若要试用 Windows Server 2019，可以下载并安装的评估副本。 请参阅[评估中心](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)。

在安装 Windows Server 或添加的 HYPER-V 角色之前，请确保：
- 计算机硬件兼容。 有关详细信息，请参阅[Windows Server 系统要求](../../../get-started/System-Requirements.md)并[System requirements for Windows Server 上的 HYPER-V 要求](../System-requirements-for-Hyper-V-on-Windows.md)。
- 您不打算使用依赖于相同的 HYPER-V 要求的处理器功能的第三方虚拟化应用。 示例包括 VMWare Workstation 和 VirtualBox。 如果不卸载这些其他应用程序，可以安装 HYPER-V。 但是，如果您尝试使用它们来管理虚拟机运行的 HYPER-V 虚拟机监控程序时，虚拟机可能不会启动，或可能 unreliably 运行。 有关详细信息和关闭的 HYPER-V 虚拟机监控程序，如果您需要使用其中一个应用的说明，请参阅[虚拟化应用程序不起作用的 HYPER-V、 Device Guard 和 Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g)。

如果你想要只安装管理工具，如 HYPER-V 管理器中，请参阅[远程管理的 HYPER-V 主机的 HYPER-V 管理器与](../Manage/Remotely-manage-Hyper-V-hosts.md)。
  
## <a name="install-hyper-v-by-using-server-manager"></a>使用服务器管理器安装 HYPER-V  
  
1. 在“服务器管理器”  中的“管理”  菜单上，单击“添加角色和功能”  。  
  
2. 在 **“开始之前”** 页面上，确定目标服务器和网络环境已为要安装的角色和功能做好准备。 单击“下一步”  。  
  
3. 在“选择安装类型”  页上，选择“基于角色或功能的安装”  ，然后单击“下一步”  。  
  
4. 在“选择目标服务器”  页上，从服务器池中选择一台服务器，然后单击“下一步”  。  
  
5. 在“选择服务器角色”  页上，选择 Hyper-V  。  
  
6. 若要添加用于创建和管理虚拟机的工具，请单击“添加功能”  。 在“功能”页上，单击“下一步”  。  
  
7. 在“创建虚拟交换机”  页、“虚拟机迁移”  页和“默认存储”  页上，选择相应的选项。  
  
8. 在“确认安装选择”  页上，选择“如果需要，自动重新启动目标服务器”  ，然后单击“安装”  。  
  
9. 完成安装后，验证正确安装 HYPER-V。 打开**的所有服务器**页在服务器管理器，并选择在其安装 HYPER-V 的服务器。 检查**角色和功能**磁贴上所选服务器的页面。  
  
## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>使用 Install-windowsfeature cmdlet 安装 HYPER-V  
  
1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  
  
2. 右键单击 Windows PowerShell，然后选择**以管理员身份运行**。  
  
3. 若要远程连接到的服务器上安装 HYPER-V，请运行以下命令并替换`<computer_name>`与服务器的名称。  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    如果你已连接到服务器的本地，运行命令而不`-ComputerName <computer_name>`。  
  
4. 在服务器重启后，可以看到 HYPER-V 角色已安装并了解有哪些其他角色和功能安装通过运行以下命令：  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    如果你已连接到服务器的本地，运行命令而不`-ComputerName <computer_name>`。  
  
> [!NOTE]  
> 如果你在运行 Windows Server 2016 的服务器核心安装选项的服务器上安装此角色，并使用参数`-IncludeManagementTools`，仅 HYPER-V 模块用于 Windows PowerShell 的安装。 在另一台计算机远程管理运行服务器核心安装的 HYPER-V 主机上，可以使用 GUI 管理工具，HYPER-V 管理器。 有关远程连接的说明，请参阅[远程管理的 HYPER-V 主机的 HYPER-V 管理器与](../Manage/Remotely-manage-Hyper-V-hosts.md)。  
  
## <a name="see-also"></a>请参阅  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
