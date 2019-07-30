---
title: 常见远程桌面连接故障排除
description: 排查远程桌面连接出现的“没有注册类”错误。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: a1a2e325f3860f0b06353a59c5d37f6b5b2a1d11
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529927"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>常见远程桌面连接故障排除

如果远程桌面客户端无法连接到远程桌面，但未提供可帮助识别原因的消息或其他症状，请使用以下步骤。

## <a name="check-the-status-of-the-rdp-protocol"></a>检查 RDP 协议的状态

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>检查本地计算机上 RDP 协议的状态

若要检查和更改本地计算机上 RDP 协议的状态，请参阅[如何启用远程桌面](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 如果远程桌面选项不可用，请参阅[检查组策略对象是否正在阻止 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>检查远程计算机上 RDP 协议的状态

> [!IMPORTANT]  
> 请认真遵循本部分的说明。 如果错误地修改了注册表，可能会出现严重问题。 开始修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，这样就可以在出错时还原它。

若要检查和更改远程计算机上 RDP 协议的状态，请使用网络注册表连接：

1. 首先转到“开始”菜单，然后选择“运行”   。 在出现的文本框中，输入“regedt32”。 
2. 在注册表编辑器中选择“文件”，然后选择“连接网络注册表”。  
3. 在“选择计算机”对话框中输入远程计算机的名称，然后依次选择“检查名称”、“确定”。   
4. 导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server”。   
   ![注册表编辑器，其中显示了 fDenyTSConnections 条目](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - 如果 **fDenyTSConnections** 项的值为 **0**，则表明已启用 RDP。
   - 如果 **fDenyTSConnections** 项的值为 **1**，则表明已禁用 RDP。
5. 若要启用 RDP，请将 **fDenyTSConnections** 的值从 **1** 更改为 **0**。

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>检查组策略对象 (GPO) 是否正在阻止本地计算机上的 RDP

如果无法在用户界面中启用 RDP，或者在更改 **fDenyTSConnections** 的值后该值恢复为 **1**，则表明某个 GPO 可能取代了计算机级别的设置。

若要检查本地计算机上的组策略配置，请以管理员身份打开命令提示符窗口，然后输入以下命令：

```cmd
gpresult /H c:\gpresult.html
```

完成此命令后，打开 gpresult.html。 在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”中，找到“允许用户通过使用远程桌面服务进行远程连接”策略。  

- 如果此策略的设置为“启用”，则表明组策略未阻止 RDP 连接。 
- 如果此策略的设置为“已禁用”，请检查“入选的 GPO”。   正在此 GPO 在阻止 RDP 连接。
  ![gpresult.html 的示例片段，其中的域级别 GPO“阻止 RDP”正在禁用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![gpresult.html 的示例片段，其中的“本地组策略”正在禁用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>检查 GPO 是否正在阻止远程计算机上的 RDP

若要检查远程计算机上的组策略配置，所用的命令基本上与本地计算机相同：

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

此命令生成的文件 (**gpresult-\<computer name\>.html**) 使用的信息格式与本地计算机版本 (**gpresult.html**) 使用的格式相同。

### <a name="modifying-a-blocking-gpo"></a>修改阻止 GPO

可以在组策略对象编辑器 (GPE) 和组策略管理控制台 (GPM) 中修改这些设置。 有关如何使用组策略的详细信息，请参阅[高级组策略管理](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/)。

若要修改阻止策略，请使用以下方法之一：

- 在 GPE 中访问 GPO 的相应级别（例如本地或域），然后导航到“计算机配置” > “管理模板” > “Windows 组件” > “远程桌面服务” > “远程桌面会话主机” > “连接” > “允许用户通过使用远程桌面服务进行远程连接”。         
   1. 将策略设置为“启用”或“不配置”。  
   2. 在受影响的计算机上，以管理员身份打开命令提示符窗口，然后运行 **gpupdate /force** 命令。
- 在 GPM 中，导航到其中的阻止策略已应用到受影响计算机的组织单位 (OU)，并从此 OU 中删除该策略。

## <a name="check-the-status-of-the-rdp-services"></a>检查 RDP 服务的状态

在本地（客户端）计算机和远程（目标）计算机上，以下服务应该正在运行：

- 远程桌面服务 (TermService)
- 远程桌面服务用户模式端口重定向程序 (UmRdpService)

可以使用“服务”MMC 管理单元在本地或远程管理这些服务。 还可以通过本地或远程方式使用 PowerShell 来管理服务（如果远程计算机配置为接受远程 PowerShell cmdlet）。

![“服务”MMC 管理单元中的远程桌面服务。 不要修改默认服务设置。](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

在任一计算机上，如果上述一个或两个服务未运行，请启动它们。

> [!NOTE]  
> 如果启动了“远程桌面服务”服务，请单击“是”自动重启“远程桌面服务用户模式端口重定向程序”服务。 

## <a name="check-that-the-rdp-listener-is-functioning"></a>检查 RDP 侦听器是否正常运行

> [!IMPORTANT]  
> 请认真遵循本部分的说明。 如果错误地修改了注册表，可能会出现严重问题。 开始修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，这样就可以在出错时还原它。

### <a name="check-the-status-of-the-rdp-listener"></a>检查 RDP 侦听器的状态

请使用具有管理权限的 PowerShell 实例完成此过程。 对于本地计算机，还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为同一 cmdlet 可以通过本地和远程方式运行。

1. 若要连接到远程计算机，请运行以下 cmdlet：

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

2. 输入 **qwinsta**。 
    ![qwinsta 命令会列出在计算机端口上侦听的进程。](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. 如果列表中包含状态为 **Listen** 的 **rdp-tcp**，则表示 RDP 侦听器正在运行。 继续[检查 RDP 侦听器端口](#check-the-rdp-listener-port)。 否则，请继续执行步骤 4。
4. 从工作计算机导出 RDP 侦听器配置。
    1. 登录到操作系统版本与受影响计算机相同的计算机，并访问该计算机的注册表（例如，使用注册表编辑器）。
    2. 导航到以下注册表项：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. 将该项导出到 .reg 文件。 例如，在注册表编辑器中，右键单击该项，选择“导出”，然后输入所导出设置的文件名。 
    4. 将导出的 .reg 文件复制到受影响的计算机。
5. 若要导入 RDP 侦听器配置，请在受影响的计算机上打开具有管理权限的 PowerShell 窗口（或打开 PowerShell 窗口并远程连接到受影响的计算机）。
   1. 若要备份现有的注册表项，请输入以下 cmdlet：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要删除现有的注册表项，请输入以下 cmdlet：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要导入新的注册表项并重启服务，请输入以下 cmdlet：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      将 \<filename\> 替换为导出的 .reg 文件的名称。

6. 通过再次尝试远程桌面连接来测试配置。 如果仍然无法连接，请重启受影响的计算机。
7. 如果仍然无法连接，请[检查 RDP 自签名证书的状态](#check-the-status-of-the-rdp-self-signed-certificate)。

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>检查 RDP 自签名证书的状态

1. 如果仍然无法连接，请打开“证书”MMC 管理单元。 根据提示选择要管理的证书存储，选择“计算机帐户”，然后选择受影响的计算机。 
2. 在“远程桌面”下的“证书”文件夹中，删除 RDP 自签名证书。   
    ![“证书”MMC 管理单元中的远程桌面证书。](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. 在受影响的计算机上，重启“远程桌面服务”服务。
4. 刷新“证书”管理单元。
5. 如果尚未重新创建 RDP 自签名证书，请[检查 MachineKeys 文件夹的权限](#check-the-permissions-of-the-machinekeys-folder)。

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>检查 MachineKeys 文件夹的权限

1. 在受影响的计算机上打开“资源管理器”，然后导航到 **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 右键单击“MachineKeys”，依次选择“属性”、“安全性”、“高级”。    
3. 确保已配置以下权限：
      - Builtin\\Administrators：完全控制
      - Everyone：读取、写入

## <a name="check-the-rdp-listener-port"></a>检查 RDP 侦听器端口

在本地（客户端）计算机和远程（目标）计算机上，RDP 侦听器应在端口 3389 上运行。 不应有任何其他应用程序正在使用此端口。

> [!IMPORTANT]  
> 请认真遵循本部分的说明。 如果错误地修改了注册表，可能会出现严重问题。 开始修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，这样就可以在出错时还原它。

若要检查或更改 RDP 端口，请使用注册表编辑器：

1. 转到“开始”菜单，选择“运行”  ，然后将 **regedt32** 输入显示的文本框中。
      - 若要连接到远程计算机，请依次选择“文件”、“连接网络注册表”。  
      - 在“选择计算机”对话框中输入远程计算机的名称，然后依次选择“检查名称”、“确定”。   
2. 打开注册表并导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<侦听器\>”。  
    ![RDP 协议的 PortNumber 子项。](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. 如果 **PortNumber** 的值不是 **3389**，请将其更改为 **3389**。 
   > [!IMPORTANT]  
    > 可以使用另一个端口来操作远程桌面服务。 但是，我们不建议这样做。 本文不介绍如何排查该类配置的问题。
4. 更改端口号后，重启“远程桌面服务”服务。

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>检查另一个应用程序是否未尝试使用同一端口

请使用具有管理权限的 PowerShell 实例完成此过程。 对于本地计算机，还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为同一 cmdlet 可以通过本地和远程方式运行。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入 **Enter-PSSession -ComputerName \<计算机名\>** 。
2. 输入以下命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![netstat 命令会生成端口列表以及正在侦听这些端口的服务列表。](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. 查找状态为“正在监听”  的 TCP 端口 3389（或分配的 RDP 端口）条目。 
    > [!NOTE]  
   > 使用该端口的进程或服务的进程标识符 (PID) 显示在 PID 列下。
4. 若要确定哪个应用程序正在使用端口 3389（或分配的 RDP 端口），请输入以下命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![tasklist 命令会报告特定进程的详细信息。](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. 查找与该端口关联的 PID 号条目（查看 **netstat** 输出）。 右列会显示与该 PID 关联的服务或进程。
6. 如果除远程桌面服务 (TermServ.exe) 以外的某个应用程序或服务正在使用该端口，你可以使用以下方法之一来解决冲突：
      - 将该应用程序或服务配置为使用其他端口（建议）。
      - 卸载该应用程序或服务。
      - 将 RDP 配置为使用其他端口，然后重启“远程桌面服务”服务（不建议）。

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>检查防火墙是否正在阻止 RDP 端口

使用 **psping** 工具测试是否可以通过端口 3389 访问受影响的计算机。

1. 转到另一台不受影响的计算机，从 <https://live.sysinternals.com/psping.exe> 下载 **psping**。
2. 以管理员身份打开命令提示符窗口，切换到安装了 **psping** 的目录，然后输入以下命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 在 **psping** 命令的输出中检查如下所示的结果：  
      - **Connecting to \<计算机 IP\>** ：远程计算机可访问。
      - **(0% loss)** ：所有连接尝试均成功。
      - **The remote computer refused the network connection**：远程计算机不可访问。
      - **(100% loss)** ：所有连接尝试均失败。
1. 在多台计算机上运行 **psping**，以测试它们是否能够连接到受影响的计算机。
1. 请注意受影响的计算机是阻止了来自所有其他计算机的连接、来自某些其他计算机的连接，还是仅来自其他一台计算机的连接。
1. 建议的后续步骤：
      - 咨询网络管理员，验证网络是否允许 RDP 流量传输到受影响的计算机。
      - 调查源计算机与受影响计算机之间的任何防火墙配置（包括受影响计算机上的 Windows 防火墙），以确定防火墙是否正在阻止 RDP 端口。
