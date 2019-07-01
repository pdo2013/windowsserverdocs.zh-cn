---
title: 排查远程桌面连接问题
description: 按症状列出的故障排除过程
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284140"
---
# <a name="troubleshooting-remote-desktop-connections"></a>排查远程桌面连接问题
有关几种最常见远程桌面服务 (RDS) 问题的简要说明，请参阅[有关远程桌面客户端的常见问题解答](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文介绍排查连接问题的几种高级方法。 不管要排查的是简单配置（例如，一台物理计算机连接到另一台物理计算机）还是较复杂的配置，其中的许多故障排除过程都适用。 某些过程可以解决只会在更复杂的多用户方案中出现的问题。 有关远程桌面组件及其如何协同工作的详细信息，请参阅[远程桌面服务体系结构](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 本文所述的许多过程要求访问多台计算机，而且需要远程访问其中的某些计算机。 有关远程管理工具及其配置方式的详细信息，请参阅[适用于 Windows 操作系统的远程服务器管理工具 (RSAT)](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在以下列表中，找到你（或你的用户）当前遇到的症状类型。

- [远程桌面客户端无法连接到远程桌面，但没有任何具体症状或消息（常规故障排除步骤）](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [远程桌面客户端无法连接到远程桌面，并收到“类未注册”消息](#client-cannot-connect-class-not-registered)
- [远程桌面客户端无法连接到远程桌面，并收到“没有可用的许可证”或“安全错误”消息](#client-cannot-connect-no-licenses-available)
- [用户收到“拒绝访问”消息，或必须提供凭据两次](#user-cannot-authenticate-or-must-authenticate-twice)
- [连接时收到“远程桌面服务当前繁忙”消息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [远程桌面客户端断开连接且无法重新连接到同一会话](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [用户通过无线网络连接到远程笔记本电脑，但随后笔记本电脑断开网络连接](#remote-laptop-disconnects-from-wireless-network)。
- [用户在使用远程应用程序时遇到性能不佳的情况或问题](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 有关最新的 RDP 断开连接代码列表，请参阅 [ExtendedDisconnectReasonCode 枚举](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>没有具体的症状或消息（常规故障排除步骤）

如果远程桌面客户端无法连接到远程桌面，但未提供可帮助识别原因的消息或其他症状，请使用以下步骤。 若要解决此类问题的许多最常见原因，请使用以下方法：

- [检查 RDP 协议的状态](#check-the-status-of-the-rdp-protocol)
- [检查 RDP 服务的状态](#check-the-status-of-the-rdp-services)
- [检查 RDP 侦听器是否正常运行](#check-that-the-rdp-listener-is-functioning)
- [检查 RDP 侦听器端口](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>检查 RDP 协议的状态

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>检查本地计算机上 RDP 协议的状态

若要检查和更改本地计算机上 RDP 协议的状态，请参阅[如何启用远程桌面](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 如果远程桌面选项不可用，请参阅[检查组策略对象是否正在阻止 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>检查远程计算机上 RDP 协议的状态

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以便在出现问题时可以还原。

若要检查和更改远程计算机上 RDP 协议的状态，请使用网络注册表连接：

1. 依次选择“开始”、“运行”，然后输入 **regedt32**。  
2. 在注册表编辑器中，依次选择“文件”、“连接网络注册表”。  
3. 在“选择计算机”对话框中输入远程计算机的名称，然后依次选择“检查名称”、“确定”。   
4. 导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server”。   
   ![注册表编辑器，其中显示了 fDenyTSConnections 条目](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - 如果 **fDenyTSConnections** 项的值为 **0**，则表示已启用 RDP
   - 如果 **fDenyTSConnections** 项的值为 **1**，则表示已禁用 RDP
5. 若要启用 RDP，请将 **fDenyTSConnections** 的值从 **1** 更改为 **0**。

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>检查组策略对象 (GPO) 是否正在阻止本地计算机上的 RDP

如果无法在用户界面中启用 RDP，或者在更改 **fDenyTSConnections** 的值后该值恢复为 **1**，则可能表示某个 GPO 取代了计算机级别的设置。

若要检查本地计算机上的组策略配置，请以管理员身份打开命令提示符窗口，然后输入以下命令：
```
gpresult /H c:\gpresult.html
```
完成此命令后，打开 gpresult.html。 在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”中，找到“允许用户通过使用远程桌面服务进行远程连接”策略。  

- 如果此策略的设置为“已启用”，则表示组策略未阻止 RDP 连接。 
- 如果此策略的设置为“已禁用”，请检查“入选的 GPO”。   正在此 GPO 在阻止 RDP 连接。
  ![gpresult.html 的示例片段，其中的域级别 GPO“阻止 RDP”正在禁用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![gpresult.html 的示例片段，其中的“本地组策略”正在禁用 RDP。](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>检查 GPO 是否正在阻止远程计算机上的 RDP

若要检查远程计算机上的组策略配置，所用的命令基本上与本地计算机相同：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令生成的文件 (**gpresult-\<computer name\>.html**) 使用的信息格式与本地计算机版本 (**gpresult.html**) 使用的格式相同。

#### <a name="modifying-a-blocking-gpo"></a>修改阻止 GPO

可以在组策略对象编辑器 (GPE) 和组策略管理控制台 (GPM) 中修改这些设置。 有关如何使用组策略的详细信息，请参阅[高级组策略管理](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/)。

若要修改阻止策略，请使用以下方法之一：

- 在 GPE 中访问 GPO 的相应级别（例如本地或域），然后导航到“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”\\“允许用户通过使用远程桌面服务进行远程连接”。    
   1. 将策略设置为“已启用”或“未配置”。  
   2. 在受影响的计算机上，以管理员身份打开命令提示符窗口，然后运行 **gpupdate /force** 命令。
- 在 GPM 中，导航到其中的阻止策略已应用到受影响计算机的 OU，并从该 OU 中删除该策略。

### <a name="check-the-status-of-the-rdp-services"></a>检查 RDP 服务的状态

在本地（客户端）计算机和远程（目标）计算机上，以下服务应该正在运行：

- 远程桌面服务 (TermService)
- 远程桌面服务用户模式端口重定向程序 (UmRdpService)

可以使用“服务”MMC 管理单元在本地或远程管理这些服务。 还可以在本地或远程使用 PowerShell（如果远程计算机配置为接受远程 PowerShell 命令）。

![“服务”MMC 管理单元中的远程桌面服务。 不要修改默认服务设置。](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

在任一计算机上，如果上述一个或两个服务未运行，请启动它们。

> [!NOTE]  
> 如果启动了“远程桌面服务”服务，请单击“是”自动重启“远程桌面服务用户模式端口重定向程序”服务。 

### <a name="check-that-the-rdp-listener-is-functioning"></a>检查 RDP 侦听器是否正常运行

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以便在出现问题时可以还原。

#### <a name="check-the-status-of-the-rdp-listener"></a>检查 RDP 侦听器的状态

请使用具有管理权限的 PowerShell 实例完成此过程。 对于本地计算机，还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为相同的命令可以在本地和远程运行。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入 **Enter-PSSession -ComputerName \<计算机名\>** 。
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
   1. 若要备份现有的注册表项，请输入以下命令：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要删除现有的注册表项，请输入以下命令：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要导入新的注册表项并重启服务，请输入以下命令：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      其中，\<filename\> 是导出的 .reg 文件的名称。
6. 通过再次尝试远程桌面连接来测试配置。 如果仍然无法连接，请重启受影响的计算机。
7. 如果仍然无法连接，请[检查 RDP 自签名证书的状态](#check-the-status-of-the-rdp-self-signed-certificate)。

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>检查 RDP 自签名证书的状态

1. 如果仍然无法连接，请打开“证书”MMC 管理单元。 根据提示选择要管理的证书存储，选择“计算机帐户”，然后选择受影响的计算机。 
2. 在“远程桌面”下的“证书”文件夹中，删除 RDP 自签名证书。   
    ![“证书”MMC 管理单元中的远程桌面证书。](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. 在受影响的计算机上，重启“远程桌面服务”服务。
4. 刷新“证书”管理单元。
5. 如果尚未重新创建 RDP 自签名证书，请[检查 MachineKeys 文件夹的权限](#check-the-permissions-of-the-machinekeys-folder)。

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>检查 MachineKeys 文件夹的权限

1. 在受影响的计算机上打开“资源管理器”，然后导航到 **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 右键单击“MachineKeys”，依次选择“属性”、“安全性”、“高级”。    
3. 确保已配置以下权限：
      - Builtin\\Administrators：完全控制
      - Everyone：读取、写入

### <a name="check-the-rdp-listener-port"></a>检查 RDP 侦听器端口

在本地（客户端）计算机和远程（目标）计算机上，RDP 侦听器应在端口 3389 上运行。 不应有任何其他应用程序正在使用此端口。

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以便在出现问题时可以还原。

若要检查或更改 RDP 端口，请使用注册表编辑器：

1. 依次选择“开始”、“运行”，然后输入 **regedt32**。 
      - 若要连接到远程计算机，请依次选择“文件”、“连接网络注册表”。  
      - 在“选择计算机”对话框中输入远程计算机的名称，然后依次选择“检查名称”、“确定”。   
2. 打开注册表并导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<侦听器\>”。  
    ![RDP 协议的 PortNumber 子项。](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. 如果 **PortNumber** 的值不是 **3389**，请将其更改为 **3389**。 
   > [!IMPORTANT]  
    > 可以使用另一个端口来操作远程桌面服务。 但是，我们不建议这样做。 排查此类配置问题不属于本文的讨论范围。
4. 更改端口号后，重启“远程桌面服务”服务。

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>检查另一个应用程序是否未尝试使用同一端口

请使用具有管理权限的 PowerShell 实例完成此过程。 对于本地计算机，还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为相同的命令可以在本地和远程运行。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入 **Enter-PSSession -ComputerName \<计算机名\>** 。
2. 输入以下命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![netstat 命令会生成端口列表以及正在侦听这些端口的服务列表。](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. 查找状态为“正在监听”  的 TCP 端口 3389（或分配的 RDP 端口）条目。 
    > [!NOTE]  
   > 使用此端口的服务或进程的 PID（进程标识符）将出现在 PID 列下。
4. 若要确定哪个应用程序正在使用端口 3389（或分配的 RDP 端口），请输入以下命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![tasklist 命令会报告特定进程的详细信息。](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. 查找与该端口关联的 PID 号条目（查看 **netstat** 输出）。 右侧将会显示与该 PID 关联的服务或进程。
6. 如果除远程桌面服务 (TermServ.exe) 以外的某个应用程序或服务正在使用该端口，你可以使用以下方法之一来解决冲突：
      - 将该应用程序或服务配置为使用其他端口（建议）。
      - 卸载该应用程序或服务。
      - 将 RDP 配置为使用其他端口，然后重启“远程桌面服务”服务（不建议）。

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>检查防火墙是否正在阻止 RDP 端口

使用 **psping** 工具测试是否可以通过端口 3389 访问受影响的计算机。

1. 在非影响的计算机上，从 <https://live.sysinternals.com/psping.exe> 下载 **psping**。
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

## <a name="client-cannot-connect-class-not-registered"></a>客户端无法连接并出现“类未注册”错误

当你尝试使用运行 Windows 10 版本 1709 或更高版本的客户端连接到远程计算机时，该客户端无法连接，同时，远程桌面会话主机服务器报告一条包含“类未注册(0x80040154)”错误代码的消息。

如果尝试建立连接的用户必须要提供某个用户配置文件，则会发生此问题。 若要解决此问题，请安装 [2018 年 7 月 24 日 — KB4338817（OS 内部版本 16299.579）](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817)Windows 10 更新。

## <a name="client-cannot-connect-no-licenses-available"></a>客户端无法连接并出现“没有可用的许可证”错误

这种情况应用于包含 RDSH 服务器和远程桌面许可服务器的部署。

识别用户看到的行为种类：

- 由于没有可用的许可证或者没有可用的许可证服务器，会话已断开连接
- 安全错误导致访问被拒绝

以域管理员身份登录到 RD 会话主机，然后打开 RD 许可证诊断程序。 找到如下所示的消息：

  - 远程桌面会话主机服务器的宽限期已过，但未使用任何许可证服务器配置 RD 会话主机服务器。 除非为 RD 会话主机服务器配置了许可证服务器，否则将拒绝与 RD 会话主机服务器建立连接。
  - 许可证服务器 \<计算机名\> 不可用。 原因可能是出现网络连接问题、远程桌面许可服务已在许可证服务器上停止，或者 RD 许可服务不再安装在计算机上。

这些问题往往与以下用户消息相关联：

  - 由于此计算机没有可用的远程桌面客户端访问许可证，远程会话已断开连接。
  - 由于没有远程桌面许可证服务器可以提供许可证，远程会话已断开连接。

对于这种情况，请[配置 RD 许可服务](#configure-the-rd-licensing-service)。

如果 RD 许可证诊断程序列出了其他问题，例如“RDP 协议组件 X.224 在协议流中检测到了错误，并已断开连接客户端”，则可能是出现了影响许可证证书的问题。 此类问题往往与如下所示的用户消息相关联：

由于出现安全错误，客户端无法连接到终端服务器。 确保已登录到网络后，重试连接到服务器。

对于这种情况，请[刷新 X509 证书注册表项](#refresh-the-x509-certificate-registry-keys)。

### <a name="configure-the-rd-licensing-service"></a>配置 RD 许可服务

以下过程使用服务器管理器进行配置更改。 有关如何配置和使用服务器管理器的信息，请参阅[服务器管理器](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager)。

1. 打开服务器管理器并导航到“远程桌面服务”。 
2. 在“部署概述”中，依次选择“任务”、“编辑部署属性”。   
3. 选择“RD 许可”，然后选择部署的相应许可模式（“按设备”或“按用户”）。   
4. 输入 RD 许可证服务器的完全限定域名 (FQDN)，然后选择“添加”。 
5. 如果你有多个 RD 许可证服务器，请针对每个服务器重复步骤 4。 
    ![服务器管理器中的 RD 许可证服务器配置选项。](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>刷新 X509 证书注册表项

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以便在出现问题时可以还原。

若要解决此问题，请备份然后删除 X509 证书注册表项，重启计算机，然后重新激活 RD 许可服务器。 为此，请按照以下步骤操作。

> [!NOTE]  
> 在每个 RDSH 服务器上执行以下过程。

1. 打开注册表编辑器，导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM”。 
2. 在“注册表”菜单中，选择“导出注册表文件”。 
3. 在“文件名”框中键入 **exported- Certificate**，然后选择“保存”。  
4. 右键单击以下每个值，选择“删除”，然后选择“是”以确认删除：    
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. 退出注册表编辑器，然后重启 RDSH 服务器。

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>用户无法完成身份验证或必须完成身份验证两次

有几个问题可能会导致用户身份验证出现问题。 本部分讨论如何解决以下情况：

  - [拒绝用户访问并出现“受限制的登录类型”消息](#access-denied-restricted-type-of-logon)
  - [拒绝用户或应用程序访问并发生事件 16965“已拒绝远程调用 SAM 数据库”](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [用户无法使用智能卡登录](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果远程电脑已锁定，则用户需要输入密码两次](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [用户无法登录并收到“身份验证错误”和“CredSSP 加密 Oracle 修正”消息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [更新客户端计算机后，某些用户需要登录两次](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [在使用 RemoteGuard 且包含多个 RD 连接代理的部署中，用户被拒绝访问](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>拒绝访问并出现“受限制的登录类型”消息

在此情况下，尝试连接到 Windows 10 或 Windows Server 2016 计算机的 Windows 10 用户被拒绝访问并会收到以下消息：

> 远程桌面连接：  
> 系统管理员限制了可以使用的登录类型（网络或交互式）。 如需帮助，请与系统管理员或技术支持人员联系。

如果 RDP 连接需要网络级别身份验证 (NLA)，而用户不是“远程桌面用户”组的成员，则会出现此问题。  如果没有为“远程桌面用户”组分配“从网络访问此计算机”用户权限，则也会出现此问题。  

若要解决此问题，请执行以下步骤之一：

  - [修改用户的组成员身份或用户权限分配](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 禁用 NLA（不建议）。
  - 使用除 Windows 10 以外的远程桌面客户端。 例如，Windows 7 客户端就不会出现此问题。

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改用户的组成员身份或用户权限分配

如果此问题只是影响一个用户，则最直接的解决方法就是将该用户添加到“远程桌面用户”组。 

如果该用户已是此组的成员（或者多个组成员遇到相同的问题），请检查远程 Windows 10 或 Windows Server 2016 计算机上的用户权限配置。

1. 打开组策略对象编辑器 (GPE) 并连接到远程计算机的本地策略。
2. 导航到“计算机配置”\\“Windows 设置”\\“安全设置”\\“本地策略”\\“用户权限分配”，右键单击“从网络访问此计算机”，然后选择“属性”。   
3. 检查“远程桌面用户”（或父组）的用户和组列表。 
4. 如果该列表不包含“远程桌面用户”（或类似于“任何人”的父组），则需要将它添加到列表（如果部署中有一台或两台计算机，请使用组策略对象）。    
    例如，“从网络访问此计算机”的默认成员身份包括“任何人”。   如果部署使用组策略对象来删除“任何人”，则你可能需要通过更新组策略对象来添加“远程桌面用户”，以还原访问权限。  

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>拒绝访问并出现“已拒绝远程调用 SAM 数据库”消息

如果域控制器运行 Windows Server 2016 或更高版本，而用户使用自定义的连接应用尝试建立连接，则很有可能会出现此行为。 具体而言，访问 Active Directory 中用户配置文件信息的应用程序将被拒绝访问。

此行为是 Windows 中的一项更改造成的。 在 Windows Server 2012 R2 和更低版本中，当用户登录到远程桌面时，远程连接管理器 (RCM) 会联系域控制器 (DC)，以查询特定于远程桌面的有关 Active Directory 域服务 (AD DS) 中的用户对象的配置。 此信息显示在“Active Directory 用户和计算机”MMC 管理单元中用户对象属性的“远程桌面服务配置文件”选项卡上。

从 Windows Server 2016 开始，RCM 不再查询 AD DS 中的用户对象。 如果你由于使用的是远程桌面服务属性而需要 RCM 查询 AD DS，必须手动启用以前的 RCM 行为。

> [!IMPORTANT]  
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)，以便在出现问题时可以还原。

若要在 RD 会话主机服务器上启用旧的 RCM 行为，请配置以下注册表项，然后重启“远程桌面服务”服务：   
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Name：**fQueryUserConfigFromDC**
      - 键入：**Reg\_DWORD**
      - 值：**1**（十进制）

若要在除 RD 会话主机服务器以外的服务器上启用旧的 RCM 行为，请配置上述注册表项和以下附加的注册表项（然后重启该服务）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

有关此行为的详细信息，请参阅 KB 3200967 [Windows Server 中远程连接管理器的更改](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>用户无法使用智能卡登录

本文将介绍如何解决用户无法使用智能卡登录到远程桌面的三种常见情况：  
  - [用户无法登录到使用只读域控制器 (RODC) 的分支机构](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [用户无法登录到最近已更新的 Windows Server 2008 SP2 计算机](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [用户无法在最近更新的 Windows 计算机上保持登录状态，且“远程桌面服务”服务无响应（挂起）](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>无法通过智能卡登录到使用 RODC 的分支机构

使用 RODC 的分支站点中包含 RDSH 服务器的部署会出现此问题。 RDSH 服务器托管在根域中。 分支站点上的用户属于一个子域，并使用智能卡进行身份验证。 RODC 配置为缓存用户密码（RODC 属于“允许的 RODC 密码复制组”）。  当用户尝试登录到 RDSH 服务器上的会话时，他们会收到类似于“尝试的登录无效。 原因是用户名或身份验证信息错误”的消息。

这是一个与根 DC 和 RODC 管理用户凭据加密的方式相关的已知问题。 根 DC 使用加密密钥来加密凭据，RODC 为客户端提供解密密钥。 但是，这两个密钥不匹配。

若要解决此问题，请考虑通过在 RODC 上禁用密码缓存或者将可写 DC 部署到分支站点，来更改 DC 拓扑。 一种替代方法是将 RDSH 服务器移到用户所在的同一个子域。 另一种替代方法是允许用户不使用智能卡登录。 上述任意解决方法都可能需要降低性能或安全级别。

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>无法使用智能卡登录到 Windows Server 2008 SP2 计算机

当用户登录到已使用 KB4093227 (2018.4B) 更新的 Windows Server 2008 SP2 计算机时，会出现此问题。 当用户尝试使用智能卡登录时，他们被拒绝访问并收到类似于“找不到有效的证书。 请检查卡是否已正确稳固地插入”的消息。 同时，Windows Server 计算机会记录应用程序事件“从插入的智能卡检索数字证书时出错。 签名无效。”

若要解决此问题，请使用 KB 4093227 的 2018.06 B 再发行版 [Windows Server 2008 中 Windows 远程桌面协议 (RDP) 拒绝服务漏洞的安全更新说明：2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)更新 Windows Server 计算机。

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>无法使用智能卡保持登录状态且“远程桌面服务”服务挂起

当用户登录到已使用 KB 4056446 更新的 Windows 或 Windows Server 计算机时，会出现此问题。 一开始，用户也许可以使用智能卡登录到该系统，但随后会接收“SCARD\_E\_NO\_SERVICE”错误消息。 远程计算机可能无响应。

若要解决此问题，请重启远程计算机。

若要解决此问题，请使用相应的修复程序更新远程计算机：

  - Windows Server 2008 SP2：KB 4090928 [Windows 泄漏 lsm.exe 进程中的句柄，智能卡应用程序可能会显示“SCARD\_E\_NO\_SERVICE”错误](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724 [2018 年 5 月 17 — KB4103724（月度汇总预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10 版本 1607：KB 4103720 [2018 年 5 月 17 — KB4103720（OS 内部版本 14393.2273）](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>如果远程电脑已锁定，则用户需要输入密码两次

当用户尝试连接到部署中运行 Windows 10 版本 1709 的远程桌面，而该部署中的 RDP 连接不需要 NLA 时，可能会出现此问题。 在这种情况下，如果远程桌面已锁定，则用户在连接时需要输入其凭据两次。

若要解决此问题，请使用 KB 4343893 [2018 年 8 月 30 日 - KB4343893（OS 内部版本 16299.637）](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893)更新 Windows 10 版本 1709 计算机。

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>用户无法登录并收到“身份验证错误”和“CredSSP 加密 Oracle 修正”消息

当用户尝试从任何 Windows Vista SP2 和更高版本或者 Windows Server 2008 SP2 和更高版本登录时，他们被拒绝访问并收到如下所示的消息：

  - 发生身份验证错误。 不支持请求的功能。
  - 原因可能是需要进行 CredSSP 加密 Oracle 修正

“CredSSP 加密 Oracle 修正”是指在 2018 年 3 月、4 月和 5 月发布的一组安全更新。 CredSSP 是一个身份验证提供程序，用于处理其他应用程序的身份验证请求。 在 2018 年 3 月 13 日，“3B”和后续更新解决了一个漏洞，攻击者可能会利用该漏洞来中继用户凭据，以在目标系统上执行代码。

初始更新添加了对新的组策略对象“加密 Oracle 修正”的支持，该对象的可能设置如下： 

  - **有漏洞**。 使用 CredSSP 的客户端应用程序可以回退到不安全的版本，但此行为会将远程桌面透露给攻击者。 使用 CredSSP 的服务接受尚未更新的客户端。
  - **已缓解**。 使用 CredSSP 的客户端应用程序无法回退到不安全的版本，但使用 CredSSP 的服务接受尚未更新的客户端。
  - **强制更新的客户端**。 使用 CredSSP 的客户端应用程序无法回退到不安全的版本，使用 CredSSP 的服务不会接受未修补的客户端。 
    注意：在所有远程主机支持最新版本之前，不应部署此设置。

2018 年 5 月 8 日更新已将默认的“加密 Oracle 修正”设置从“有漏洞”更改为“已缓解”。    进行此项更改后，包含更新的远程桌面客户端无法连接到不包含更新的服务器（或尚未重启的已更新服务器）。 有关更新的影响及其阻止的通信类型的详细信息，请参阅 KB 4093492 [CVE-2018-0886 的 CredSSP 更新](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解决此问题，请确保所有系统已完全更新并已重启。 有关更新的完整列表以及有关漏洞的详细信息，请参阅 [CVE-2018-0886 | CredSSP 远程代码执行漏洞](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

在完成更新之前若要解决此问题，请在 KB 4093492 中查看允许的连接类型。 如果没有可行的替代方法，可以考虑以下方法之一：

- 对于受影响的客户端计算机，将“加密 Oracle 修正”策略设置回到“有漏洞”。  
- 在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“安全性”组策略文件夹中修改以下策略：   
  - 将“要求对远程(RDP)连接使用特定的安全层”设置为“已启用”，并选择“RDP”。   
  - 将“要求使用网络级别身份验证来远程连接的用户进行身份验证”设置为“已禁用”。  
    > [!IMPORTANT]  
    > 这些修改会降低部署安全性。 如果确实需要使用这些设置，也只能暂时性地使用。

有关使用组策略的详细信息，请参阅[修改阻止 GPO](#modifying-a-blocking-gpo)。

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>更新客户端计算机后，某些用户需要登录两次

某些 Windows 7 或 Windows 10 版本 1709 上的用户在登录到远程桌面会话时，会立即看到再次登录的屏幕。 如果客户端计算机收到了以下更新，则会出现此问题：

  - Windows 7：KB 4103718 [2018 年 5 月 8 日 - KB4103718（每月汇总）](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709：KB 4103727 [2018 年 5 月 8 日 - KB4103727（OS 内部版本 16299.431）](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解决此问题，请确保用户要连接到的计算机（以及 RDSH 或 RDVI 服务器）已完全通过 2018 年 6 月更新包更新。 这包括以下更新：

  - Windows Server 2016：KB 4284880 [2018 年 6 月 12 日 - KB4284880（OS 内部版本 14393.2312）](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815 [2018 年 6 月 12 日 - KB4284815（每月汇总）](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012：KB 4284855 [2018 年 6 月 12 日 - KB4284855（每月汇总）](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826 [2018 年 6 月 12 日 - KB4284826（每月汇总）](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2：KB4056564 [Windows Server 2008、Windows Embedded POSReady 2009 和 Windows Embedded Standard 2009 中 CredSSP 远程代码执行漏洞的安全更新说明：2018 年 3 月 13 日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>在使用 Remote Credential Guard 且包含多个 RD 连接代理的部署中，用户被拒绝访问

如果使用了 Windows Defender Remote Credential Guard，使用两个或更多个远程桌面连接代理的高可用性部署中会出现此问题。 用户无法登录到远程桌面。

出现此问题的原因是 Remote Credential Guard 使用 Kerberos 进行身份验证，并限制 NTLM。 但是，在使用负载均衡的高可用性配置中，RD 连接代理无法支持 Kerberos 操作。

如果需要将高可用性配置与负载均衡的 RD 连接代理配合使用，可以通过禁用 Remote Credential Guard 来解决此问题。 有关如何管理 Windows Defender Remote Credential Guard 的详细信息，请参阅[使用 Windows Defender Remote Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>连接时用户收到“远程桌面服务当前繁忙”消息

若要确定此问题的相应对策，请使用以下步骤：

1. “远程桌面服务”服务是否无响应（例如，远程桌面客户端在“欢迎”屏幕上呈“挂起”状态）。  
      - 如果该服务无响应，请参阅 [RDSH 服务器内存问题](#rdsh-server-memory-issue)。
      - 如果客户端似乎能够正常地与该服务交互，请继续执行下一步骤。
2. 如果一个或多个用户断开其远程桌面会话的连接，这些用户是否可以再次连接？  
   - 如果不管有多少个用户断开连接其会话，该服务仍会不断地拒绝连接，请参阅 [RD 侦听器问题](#rd-listener-issue)。
   - 如果在一些用户断开连接其会话后，该服务再次开始接受连接，请[检查连接限制策略](#check-the-connection-limit-policy)。

### <a name="rdsh-server-memory-issue"></a>RDSH 服务器内存问题

在某些 Windows Server 2012 R2 RDSH 服务器上发现了内存泄漏。 一段时间后，这些服务器会开始拒绝远程桌面连接和本地控制台登录，并显示如下所示的消息：

> 由于远程桌面服务当前繁忙，无法完成你要执行的任务。 请过几分钟重试。 其他用户应该仍可登录。

尝试连接的远程桌面客户端也无响应。

若要解决此问题，请重启 RDSH 服务器。

若要解决此问题，请向 RDSH 服务器应用 KB 4093114 [2018 年 4 月 10 日 - KB4093114（每月汇总）](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup))。

### <a name="rd-listener-issue"></a>RD 侦听器问题

在直接从 Windows Server 2008 R2 升级到 Windows Server 2012 R2 或 Windows Server 2016 的某些 RDSH 服务器上注意到了一个问题。 当远程桌面客户端连接到 RDSH 服务器时，RDSH 服务器会为用户会话创建一个 RD 侦听器。 受影响的服务器将统计每次用户连接时都会递增，但永远不会递减的 RD 侦听器计数。

若要解决此问题，可使用以下方法：

  - 重启 RDSH 服务器，以重置 RD 侦听器的计数
  - 修改连接限制策略，将其设置为很大的值。 有关管理连接限制策略的详细信息，请参阅[检查连接限制策略](#check-the-connection-limit-policy)。

若要解决此问题，请将以下更新应用到 RDSH 服务器：

  - Windows Server 2012 R2：KB 4343891 [2018 年 8 月 30 日 - KB4343891（月度汇总预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016：KB 4343884 [2018 年 8 月 30 日 - KB4343884（OS 内部版本 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>检查连接限制策略

可以在单个计算机级别或者通过配置组策略对象 (GPO) 来设置同时建立的远程桌面连接数限制。 默认未设置该项限制。

若要检查当前设置并识别 RDSH 服务器上的任何现有 GPO，请以管理员身份打开命令提示符窗口，然后输入以下命令：
  
```
gpresult /H c:\gpresult.html
```
   
完成此命令后，打开 gpresult.html，在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”中，找到“限制连接数”策略。  

  - 如果此策略的设置为“已禁用”，则表示组策略未限制 RDP 连接数。 
  - 如果此策略的设置为“已启用”，请检查“入选的 GPO”。   如果需要删除或更改连接限制，请编辑此 GPO。

若要强制实施策略更改，请在受影响的计算机上打开命令提示符窗口并输入以下命令：
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>RD 客户端断开连接且无法重新连接到同一会话

在远程桌面客户端从远程桌面断开连接后，该客户端无法立即重新连接。 用户会收到如下所示的错误消息：

  - 由于出现安全错误，客户端无法连接到终端服务器。 确保已登录到网络后，重试连接到服务器。
  - 远程桌面已断开连接。 由于出现安全错误，客户端无法连接到远程计算机。 请验证是否已登录到网络，然后重试连接。

在一段时间后，远程桌面客户端将重新连接到远程桌面。 但是，RDSH 服务器不会将客户端连接到原始会话，而将其连接到新的会话。 检查 RDSH 服务器可以发现，原始会话并未进入断开连接状态，而是保持活动状态。

若要解决此问题，可以在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”组策略文件夹中启用“配置 keep-alive 连接间隔”策略。   如果启用此策略，则必须输入 keep-alive 间隔。 keep-alive 间隔确定服务器检查会话状态的频率（以分钟为单位）。

此问题可能是错误的身份验证配置和不当的配置设置造成的。 可以在服务器级别或通过使用 GPO 配置这些设置。 “计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“安全性”组策略文件夹中提供了组策略设置。 

1. 在 RD 会话主机服务器上，打开“远程桌面会话主机配置”。
2. 在“连接”下，右键单击连接的名称，然后单击“属性”。  
3. 在该连接的“属性”对话框中的“常规”选项卡上，在“安全”层中选择一种安全方法。   
4. 在“加密级别”中，单击所需的级别。  可以选择“低”、“客户端兼容”、“高”或“符合 FIPS”。    

> [!NOTE]  
>  - 如果客户端与 RD 会话主机服务器之间的通信需要最高级别的加密，请使用“符合 FIPS”加密。
>  - 在“组策略”中配置的任何加密级别设置会替代使用远程桌面服务配置工具设置的配置。 此外，如果启用[系统加密:使用符合 FIPS 的算法进行加密、哈希处理和签名](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))策略，则此设置会替代“设置客户端连接加密级别”策略。  系统加密策略位于“计算机配置”\\“Windows 设置”\\“安全设置”\\“本地策略”\\“安全选项”文件夹中。 
>  - 更改加密级别后，新的加密级别将在用户下一次登录时生效。 如果需要在一台服务器上使用多个加密级别，请安装多个网络适配器并单独配置每个适配器。
>  - 若要验证证书是否有相应的私钥，请在远程桌面服务配置中，右键单击要查看其证书的连接，然后依次选择“常规”、“编辑”、要查看的证书、“查看证书”。    在“常规”选项卡的底部，应会显示“你已有对应于此证书的私钥”语句。  也可以使用“证书”管理单元查看此信息。
>  - “符合 FIPS”加密（远程桌面服务器配置中的“系统加密:  使用符合 FIPS 的算法进行加密、哈希处理和签名”策略或“符合 FIPS”设置）将使用 Microsoft 加密模块通过联邦信息处理标准 (FIPS) 140-1 加密算法，对从客户端发送到服务器以及从服务器发送到客户端的数据进行加密和解密。  有关详细信息，请参阅 [FIPS 140 验证](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)。
>  - “高”设置使用强式 128 位加密算法对从客户端发送到服务器以及从服务器发送到客户端的数据进行加密。 
>  - “客户端兼容”设置以客户端支持的最大密钥强度加密客户端与服务器之间发送的数据。 
>  - “低”设置使用 56 位加密算法对从客户端发送到服务器的数据进行加密。 

## <a name="remote-laptop-disconnects-from-wireless-network"></a>远程笔记本电脑从无线网络断开连接

当远程桌面客户端使用 802.1x 无线网络连接到笔记本电脑时，可能会出现此问题。 笔记本电脑间歇性地从无线网络断开连接，并不会自动重新连接。

当无线网络连接的网络身份验证设置为“用户身份验证”时，会发生这种已知问题。 

若要解决此问题，请将网络身份验证设置指定为“用户或计算机身份验证”或者“计算机身份验证”。  

 > [!NOTE]  
> 若要更改一台计算机上的网络身份验证设置，可能需要使用“网络和共享中心”控制面板来以新的设置创建新的无线连接。

有关如何使用 GPO 配置无线网络设置的完整说明，请参阅[配置无线网络 (IEEE 802.11) 策略](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="user-experiences-poor-performance-or-application-problems"></a>用户遇到性能不佳的情况或应用程序问题

本部分介绍如何解决用户在使用远程桌面功能时可能会遇到的几种常见问题：

  - [连接到 Microsoft Azure 新虚拟机的用户间歇性地遇到问题](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [连接到 Windows 10 版本 1709 远程桌面的用户在播放视频时可能会遇到问题](#video-playback-issues-on-windows-10-version-1709)。
  - [连接到 Windows 10 远程桌面且具有只读用户配置文件的用户无法共享其桌面。](#desktop-sharing-issues-on-windows-10)
  - [在禁用 NLA 的情况下，使用早期 Windows 10 版本上运行的客户端连接到 Windows 10 远程桌面的用户遇到性能不佳的问题](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [当用户在远程桌面会话中运行多个应用程序并频繁地断开连接和重新连接时，他们在重新连接时可能会遇到黑屏。](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>使用 Microsoft Azure 新虚拟机时出现的间歇性问题

此问题影响最近已预配的虚拟机。 在用户连接到虚拟机后，远程桌面会话可能无法正确加载该用户的所有设置。

若要解决此问题，请从虚拟机断开连接，等待至少 20 分钟，然后再次连接。

若要解决此问题，请适当地将以下更新应用到虚拟机：

  - Windows 10 和 Windows Server 2016：KB 4343884 [2018 年 8 月 30 日 - KB4343884（OS 内部版本 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891 [2018 年 8 月 30 日 - KB4343891（月度汇总预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Windows 10 版本 1709 上的视频播放问题

当用户连接到运行 Windows 10 版本 1709 的远程计算机时，会出现此问题。 当这些用户使用 VMR9（视频混合渲染器 9）编解码器播放视频时，播放器只会显示黑屏。

这是 Windows 10 版本 1709 中的一个已知问题。 Windows 10 版本 1703 中不会出现此问题。

### <a name="desktop-sharing-issues-on-windows-10"></a>Windows 10 上的桌面共享问题

如果用户具有只读的用户配置文件和关联的注册表配置单元（例如在网亭方案中），则会出现此问题。 当此类用户连接到运行 Windows 10 版本 1803 的远程计算机时，他们无法共享其桌面。

若要解决此问题，请应用 Windows 10 更新 4340917 [2018 年 7 月 24 日 - KB4340917（OS 内部版本 17134.191）](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>在禁用 NLA 的情况下混合 Windows 10 版本时的性能问题

如果已禁用 NLA，并且运行 Windows 10 的远程桌面客户端计算机连接到运行不同 Windows 10 版本的远程桌面，则会出现此问题。 运行 Windows 10 版本 1709 或更低版本的计算机上的远程桌面客户端用户在连接到运行 Windows 10 版本 1803 或更高版本的远程桌面时，会遇到性能不佳的问题。

此问题的原因是，当禁用了 NLA 时，旧式客户端计算机在连接到 Windows 10 版本 1803 或更高版本时，会使用速度较慢的协议。

若要解决此问题，请应用 KB 4340917 [2018 年 7 月 24 日 - KB4340917（OS 内部版本 17134.191）](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑屏问题

此问题出现在 Windows 8.0、Windows 8.1、Windows 10 RTM 和 Windows Server 2012 R2 中。 用户在远程桌面中启动多个应用程序，然后从会话断开连接。 用户定期重新连接到远程桌面以便与应用程序交互，然后再次断开连接。 当用户在某个时间重新连接时，远程桌面会话只显示黑屏。 用户必须从远程计算机的控制台（或 RDSH 服务器控制台）结束远程桌面会话。 此操作会停止应用程序。

若要解决此问题，请适当地应用以下更新：

  - Windows 8 和 Windows Server 2012：KB4103719 [2018 年 5 月 17 日 - KB4103719（月度汇总预览）](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2：KB4103724 [2018 年 5 月 17 日 - KB4103724（月度汇总预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)和 KB 4284863 [2018 年 6 月 21 日 - KB4284863（月度汇总预览）](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10：已在 KB4284860 [2018 年 6 月 12 日 - KB4284860（OS 内部版本 10240.17889）](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)中予以修复
