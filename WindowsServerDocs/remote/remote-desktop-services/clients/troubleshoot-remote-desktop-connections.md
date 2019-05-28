---
title: 远程桌面连接故障排除
description: 故障排除过程按症状排列
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
ms.openlocfilehash: 4bbdd17f5e6e2b161e0dda0e172ea862a9107841
ms.sourcegitcommit: 564158d760f902ced7f18e6d63a9daafa2a92bd4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/01/2019
ms.locfileid: "64988339"
---
# <a name="troubleshooting-remote-desktop-connections"></a>远程桌面连接故障排除
有关的几个最常见的远程桌面服务 (RDS) 问题的简短说明，请参阅[有关远程桌面客户端常见问题](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文介绍排查连接问题的几个更高级的方法。 这些过程的许多应用是否正在排查的简单配置，如一台物理计算机连接到另一台物理计算机或更复杂的配置。 某些过程解决问题，仅在更复杂的多用户方案中出现。 有关远程桌面组件以及它们如何协同工作的详细信息，请参阅[远程桌面服务体系结构](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 本文中介绍的过程的许多要求访问多个计算机，其中一些您可能需要远程访问。 有关远程管理工具以及如何将其配置的详细信息，请参阅[远程服务器管理工具 (RSAT) 的 Windows 操作系统](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在以下列表中，确定你 （或你的用户） 遇到的症状的类型。

- [远程桌面客户端无法连接到远程桌面，但没有任何具体的症状或消息 （常规故障排除步骤）](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [远程桌面客户端无法连接到远程桌面，并收到"未注册类"消息](#client-cannot-connect-class-not-registered)
- [远程桌面客户端无法连接到远程桌面，并收到"无可用的许可证"或"安全错误"消息](#client-cannot-connect-no-licenses-available)
- [用户收到"拒绝访问"消息时，或者必须两次提供凭据](#user-cannot-authenticate-or-must-authenticate-twice)
- [在连接时，收到"远程桌面服务是当前正忙"消息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [远程桌面客户端断开连接，并不能重新连接到同一个会话](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [用户通过无线网络连接到远程的便携式计算机和便携式计算机然后从网络断开连接](#remote-laptop-disconnects-from-wireless-network)。
- [用户感觉到性能不佳或远程应用程序的问题](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 有关 RDP 断开连接代码的当前列表，请参阅[ExtendedDisconnectReasonCode 枚举](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>没有具体的症状或消息 （常规故障排除步骤）

远程桌面客户端无法连接到远程桌面，但不提供消息或其他症状会帮助确定原因时，请使用以下步骤。 若要解决许多这类问题的最常见原因，请使用以下方法：

- [检查 RDP 协议的状态](#check-the-status-of-the-rdp-protocol)
- [检查的 RDP 服务的状态](#check-the-status-of-the-rdp-services)
- [检查 RDP 侦听器正常工作](#check-that-the-rdp-listener-is-functioning)
- [检查 RDP 侦听器端口](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>检查 RDP 协议的状态

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>检查本地计算机上的 RDP 协议的状态

若要检查并更改本地计算机上的 RDP 协议的状态，请参阅[如何启用远程桌面](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 如果远程桌面选项不可用，请参阅[检查组策略对象是否阻止 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>检查远程计算机上的 RDP 协议的状态

> [!IMPORTANT]  
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在出现问题时。

若要检查并更改远程计算机上的 RDP 协议的状态，使用网络注册表连接：

1. 选择**启动**，选择**运行**，然后输入**regedt32**。
2. 在注册表编辑器中，选择**文件**，然后选择**连接网络注册表**。
3. 在中**选择计算机**对话框框中，输入远程计算机的名称，选择**检查名称**，然后选择**确定**。
4. 导航到**HKEY\_本地\_机\\系统\\CurrentControlSet\\控制\\终端服务器**。  
   ![注册表编辑器中，显示 fDenyTSConnections 条目](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - 如果的值**fDenyTSConnections**该键**0**，则启用 RDP
   - 如果的值**fDenyTSConnections**该键**1**，则禁用 RDP
5. 若要启用 RDP，更改的值**fDenyTSConnections**从**1**到**0**。

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>检查组策略对象 (GPO) 是否阻止本地计算机上的 RDP

如果你不能启用的用户界面中的 RDP 或如果的值**fDenyTSConnections**将恢复为**1** GPO 已更改后，可能会覆盖计算机级别设置。

若要检查本地计算机上的组策略配置，请打开命令提示符窗口，以管理员身份，并输入以下命令：
```
gpresult /H c:\gpresult.html
```
此命令完成后，打开 gpresult.html。 在中**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\连接**，查找**允许用户使用远程桌面服务连接远程**策略。

- 如果为此策略设置为**已启用**，组策略不会阻止 RDP 连接。
- 如果为此策略设置为**已禁用**，检查**入选 GPO**。 这是正在阻止 RDP 连接的 GPO。
![Gpresult.html，在其中一个示例段域级别 GPO * * 块 RDP * * 正在禁用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html，在其中一个示例段 * * 本地组策略 * * 正在禁用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>检查 GPO 是否阻止远程计算机上的 RDP

若要检查远程计算机上的组策略配置，该命令与本地计算机几乎相同：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令将生成的文件 (**gpresult-\<计算机名\>.html**) 作为本地计算机版本使用相同的信息格式 (**gpresult.html**) 使用。

#### <a name="modifying-a-blocking-gpo"></a>修改阻塞的 GPO

可以修改的组策略对象编辑器 (GPE) 和组策略管理控制台 (GPM) 中的这些设置。 有关如何使用组策略的详细信息，请参阅[高级组策略管理](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/)。

若要修改的阻止策略，请使用以下方法之一：

- 在 GPE，访问 （例如本地或域） 的适当级别的 GPO，并导航到**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\连接**\\**允许用户使用远程桌面服务连接远程**。  
   1. 将策略设置为**已启用**或**未配置**。
   2. 在受影响的计算机上打开命令提示符窗口，以管理员身份，并运行**gpupdate /force**命令。
- 在 GPM，导航到在其中阻止策略应用于受影响的计算机的 OU 删除和策略的 OU。

### <a name="check-the-status-of-the-rdp-services"></a>检查的 RDP 服务的状态

在本地 （客户端） 计算机和远程 （目标） 计算机上，应运行以下服务：

- 远程桌面服务 (TermService)
- 远程桌面服务 UserMode 端口重定向程序 (UmRdpService)

可以使用服务 MMC 管理单元用于本地或远程管理的服务。 本地或远程方式，还可以使用 PowerShell （如果远程计算机配置为接受远程 PowerShell 命令）。

![在服务 MMC 管理单元中的远程桌面服务。 不要修改默认服务设置。](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

在任何一台计算机，如果一个或两个服务未运行，启动它们。

> [!NOTE]  
> 如果您启动远程桌面服务的服务，请单击**是**自动重新启动远程桌面服务 UserMode 端口重定向程序服务。

### <a name="check-that-the-rdp-listener-is-functioning"></a>检查 RDP 侦听器正常工作

> [!IMPORTANT]  
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在出现问题时。

#### <a name="check-the-status-of-the-rdp-listener"></a>检查 RDP 侦听器的状态

对于此过程，使用具有管理权限的 PowerShell 实例。 对于本地计算机，您还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为相同的命令都能正常工作本地和远程。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入**Enter-pssession-ComputerName\<计算机名\>** 。
2. 输入**qwinsta**。 
    ![Qwinsta 命令将列出计算机的端口上侦听的进程。](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. 如果列表中包含**rdp tcp**状态为**侦听**，RDP 侦听器是否正常工作。 请继续执行[检查 RDP 侦听器端口](#check-the-rdp-listener-port)。 否则，继续执行步骤 4。
4. 从工作的计算机导出 RDP 侦听器配置。
    1. 登录到具有相同的操作系统版本，因为受影响的计算机，计算机并访问该计算机的注册表 （例如，通过使用注册表编辑器）。
    2. 导航到以下注册表项：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. 将条目导出到.reg 文件。 例如，在注册表编辑器中，右键单击该条目，选择**导出**，并在导出的设置中输入文件名。
    4. 将导出的.reg 文件复制到受影响的计算机。
5. 若要导入 RDP 侦听器配置，请打开 PowerShell 窗口，在受影响的计算机上具有管理权限 （或打开 PowerShell 窗口并远程连接到受影响的计算机）。
   1. 若要备份的现有的注册表项，请输入以下命令：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要删除现有的注册表项，请输入以下命令：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要导入新的注册表条目，然后重新启动该服务，请输入以下命令：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      其中\<文件名\>是导出的.reg 文件的名称。
6. 尝试远程桌面连接再次测试配置。 如果仍然无法连接，请重新启动受影响的计算机。
7. 如果仍然无法连接，[检查 RDP 自签名证书的状态](#check-the-status-of-the-rdp-self-signed-certificate)。

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>检查 RDP 自签名证书的状态

1. 如果仍然无法连接，打开证书 MMC 管理单元中。 当提示选择要管理中，选择的证书存储**计算机帐户**，然后选择受影响的计算机。
2. 在中**证书**下的文件夹**远程桌面**，删除 RDP 自签名的证书。 
    ![在 MMC 证书管理单元中的远程桌面证书。](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. 在受影响的计算机上重新启动远程桌面服务服务。
4. 证书管理单元中的刷新。
5. 如果尚未重新创建 RDP 自签名的证书[检查的 MachineKeys 文件夹权限](#check-the-permissions-of-the-machinekeys-folder)。

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>检查的 MachineKeys 文件夹的权限

1. 在受影响的计算机中，打开资源管理器，，然后导航到**c:\\ProgramData\\Microsoft\\Crypto\\RSA\\** 。
2. 右键单击**MachineKeys**，选择**属性**，选择**安全**，然后选择**高级**。
3. 请确保配置以下权限：
      - 内置\\管理员：完全控制
      - 任何人：读取、 写入

### <a name="check-the-rdp-listener-port"></a>检查 RDP 侦听器端口

在本地 （客户端） 计算机和远程 （目标） 计算机上，应在端口 3389 上侦听 RDP 侦听器。 任何其他应用程序应不使用此端口。

> [!IMPORTANT]  
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在出现问题时。

若要检查或更改 RDP 端口，请使用注册表编辑器：

1. 选择开始，选择**运行**，然后输入**regedt32**。
      - 若要连接到远程计算机，请选择**文件**，然后选择**连接网络注册表**。
      - 在中**选择计算机**对话框框中，输入远程计算机的名称，选择**检查名称**，然后选择**确定**。
2. 打开注册表并导航到**HKEY\_本地\_机\\系统\\CurrentControlSet\\控制\\终端服务器\\WinStations\\\<侦听器\>** 。 
    ![RDP 协议在端口号子项。](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. 如果**PortNumber**而不具有值**3389**，将其更改为**3389**。 
   > [!IMPORTANT]  
    > 可以运行远程桌面服务使用另一个端口。 但是，我们不建议执行此操作。 解决此类配置不属于本文的讨论范围。
4. 更改端口号后，重新启动远程桌面服务服务。

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>检查另一个应用程序未尝试使用相同的端口

对于此过程，使用具有管理权限的 PowerShell 实例。 对于本地计算机，您还可以使用具有管理权限的命令提示符。 但是，此过程使用 PowerShell，因为相同的命令处理本地和远程。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入**Enter-pssession-ComputerName\<计算机名\>** 。
2. 输入以下命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令生成端口和侦听这些服务的列表。](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. 查找状态为“正在监听”  的 TCP 端口 3389（或分配的 RDP 端口）条目。 
    > [!NOTE]  
   > 使用此端口的服务或进程的 PID（进程标识符）将出现在 PID 列下。
1. 若要确定哪个应用程序正在使用端口 3389 （或分配的 RDP 端口），请输入以下命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令会报告特定过程的详细信息。](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. 查找与端口相关联的 PID 号条目 (从**netstat**输出)。 服务或与该 PID 关联的进程显示在右侧。
1. 如果应用程序或远程桌面服务 (TermServ.exe) 以外的服务正在使用该端口，可以通过使用以下方法之一来解决冲突：
      - 配置的其他应用程序或服务以使用其他端口 （推荐）。
      - 卸载其他应用程序或服务。
      - 配置 RDP 使用其他端口，然后重新启动远程桌面服务服务 （不推荐）。

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>检查防火墙是否阻止 RDP 端口

使用**psping**工具以测试是否可以通过使用端口 3389 来访问受影响的计算机。

1. 不同于受影响的计算机的计算机上, 下载**psping**从<https://live.sysinternals.com/psping.exe>。
2. 打开命令提示符窗口，以管理员身份，更改到在其中安装目录**psping**，然后输入以下命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 检查的输出**psping**命令的结果如下所示：  
      - **连接到\<计算机 IP\>** :访问远程计算机。
      - **(0 %loss)** :所有连接尝试成功。
      - **在远程计算机被拒绝的网络连接**:在远程计算机不可访问。
      - **（100%丢失）** :所有连接尝试失败。
1. 运行**psping**多台计算机来测试其能否连接到受影响的计算机上。
1. 请注意是否受影响的计算机上阻止从所有其他计算机、 某些其他计算机或只有一台其他计算机的连接。
1. 建议的后续步骤：
      - 请咨询你的网络管理员以验证网络允许 RDP 流量传输到受影响的计算机。
      - 调查源计算机与受影响的计算机 （包括在受影响的计算机上的 Windows 防火墙） 之间的任何防火墙的配置以确定防火墙是否阻止 RDP 端口。

## <a name="client-cannot-connect-class-not-registered"></a>客户端无法连接，"未注册类"

当你尝试使用正在运行 Windows 10 版本 1709年的客户端连接到远程计算机或更高版本，客户端可能无法连接到远程桌面会话主机服务器报告一条消息时，包含"类未注册 (0x80040154)"错误代码。

如果尝试连接的用户具有强制用户配置文件，会发生此问题。 若要解决此问题，请安装[2018 年 7 月 24 日 — KB4338817 (OS 生成 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) Windows 10 更新。

## <a name="client-cannot-connect-no-licenses-available"></a>客户端无法连接，无可用的许可证

这种情况下应用于包括 RDSH 服务器和远程桌面授权服务器的部署。

确定哪种类型的用户看到的行为：

- 由于没有许可证可用，或没有许可证可用服务器的会话已断开连接
- 由于安全错误，访问被拒绝

登录到 RD 会话主机以域管理员，并打开远程桌面许可诊断程序。 查找消息如下所示：

  - 在宽限期内的远程桌面会话主机服务器已过期，但尚未与任何许可证服务器配置的 RD 会话主机服务器。 除非许可证服务器配置为 RD 会话主机服务器将拒绝连接到 RD 会话主机服务器。
  - 许可证服务器\<计算机名\>不可用。 这可能引起网络连接问题、 远程桌面授权服务已停止在许可证服务器上，或在计算机上不再安装 RD 授权。

这些问题往往能够与以下用户消息相关联：

  - 远程会话已断开连接，因为此计算机没有远程桌面客户端访问许可证。
  - 由于没有可用于提供许可证没有远程桌面许可证服务器，远程会话已断开连接。

在这种情况下，[配置的 RD 授权服务](#configure-the-rd-licensing-service)。

如果远程桌面许可诊断程序列出其他问题，如"RDP 协议组件 X.224 协议流中检测到错误，并且已断开连接的客户端，"可能会影响许可证证书有问题。 此类问题往往与用户消息，如下所示：

由于安全错误，导致客户端无法连接到终端服务器。 确保你登录到网络之后, 再次尝试连接到服务器。

在这种情况下，[刷新 X509 证书的注册表项](#refresh-the-x509-certificate-registry-keys)。

### <a name="configure-the-rd-licensing-service"></a>配置 RD 授权服务

以下过程使用服务器管理器进行配置更改。 有关如何配置和使用服务器管理器的信息，请参阅[服务器管理器](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager)。

1. 打开服务器管理器，然后导航到**远程桌面服务**。
2. 上**部署概述**，选择**任务**，然后选择**编辑部署属性**。
3. 选择**RD 授权**，然后选择你的部署的适当授权模式 (**每个设备**或**每用户**)。
4. 输入您的远程桌面许可证服务器的完全限定的域名 (FQDN)，然后选择**添加**。
5. 如果你有多个远程桌面许可证服务器，每个服务器重复步骤 4。 
    ![远程桌面许可证服务器配置选项在服务器管理器。](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>刷新 X509 证书的注册表项

> [!IMPORTANT]  
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在出现问题时。

若要解决此问题，备份，然后删除 X509 证书的注册表项，重新启动计算机，然后重新激活 RD 授权服务器。 为此，请按照以下步骤操作。

> [!NOTE]  
> 每台 RDSH 服务器上执行以下过程。

1. 打开注册表编辑器并导航到**HKEY\_本地\_机\\系统\\CurrentControlSet\\控制\\终端服务器\\RCM**.
2. 在注册表菜单中，选择**导出注册表文件**。
3. 类型**导出证书**中**文件名**框，并选择**保存**。
4. 右键单击每个以下值，选择**删除**，然后选择**是**以确认删除：  
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. 退出注册表编辑器，然后重新 RDSH 服务器。

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>用户无法进行身份验证，或必须进行身份验证两次

有可能会导致影响用户身份验证的问题的几个问题。 本部分解决以下用例：

  - [用户被拒绝访问"受限的登录类型"的消息](#access-denied-restricted-type-of-logon)
  - [用户或应用程序被拒绝访问与事件 16965，"远程调用在 SAM 数据库已被拒绝"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [用户不能使用智能卡登录](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果远程 PC 处于锁定状态，用户需要进行两次输入密码](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [用户无法登录并收到"身份验证错误"和"CredSSP 加密 oracle 修正"消息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [更新客户端计算机后，某些用户需要进行两次登录](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [用户被拒绝使用 RemoteGuard 与多个 RD 连接代理的部署上的访问](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>访问被拒绝，受限制的登录类型

在此情况下，尝试连接到 Windows 10 或 Windows Server 2016 计算机的 Windows 10 用户被拒绝访问并显示以下消息：

> 远程桌面连接：  
> 系统管理员具有受限的登录类型 (网络或交互式)，可以使用。 要获得帮助，请与系统管理员或技术支持联系。

网络级别身份验证 (NLA) RDP 连接所必需的并且用户不是的成员时，会出现此问题**Remote Desktop Users**组。 如果它也会发生**Remote Desktop Users**尚未分配给组**从网络访问这台计算机**用户权限。

若要解决此问题，请执行以下步骤之一：

  - [修改用户的组成员身份或用户权限分配](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 关闭 NLA （不推荐）。
  - 使用 Windows 10 以外的远程桌面客户端。 例如，Windows 7 客户端不具有此问题。

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改用户的组成员身份或用户权限分配

如果此问题会影响单个用户，此问题的最简单解决方案是将用户添加到**Remote Desktop Users**组。

如果用户已经是此组的成员 （或如果多个组成员具有相同的问题），，检查远程 Windows 10 或 Windows Server 2016 计算机上的用户权限配置。

1. 打开组策略对象编辑器 (GPE) 并连接到远程计算机的本地策略。
2. 导航到**计算机配置\\Windows 设置\\安全设置\\本地策略\\用户权限分配**，右键单击**访问此从网络计算机**，然后选择**属性**。
3. 检查用户和组的列表**Remote Desktop Users** （或父组）。
4. 如果列表不包括**Remote Desktop Users** (或其父组，如**每个人都**) 需要将其添加到列表 （如果您在部署中的多个或两个计算机，使用组策略对象）.  
    例如的默认成员身份**从网络访问这台计算机**包括**每个人都**。 如果你的部署使用组策略对象以删除**每个人都**，可能需要通过更新要添加的组策略对象来恢复访问权限**Remote Desktop Users**。

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>访问被拒绝，到 SAM 数据库的远程调用已被拒绝

此行为是最有可能发生，如果你的域控制器运行 Windows Server 2016 或更高版本，以及用户尝试通过使用自定义的连接应用程序连接。 具体而言，应用程序来访问 Active Directory 中的用户的配置文件信息将被拒绝访问。

此行为进行更改将导致到 Windows。 在 Windows Server 2012 R2 及更早版本中，当用户登录到远程桌面，远程连接管理器 (RCM) 联系人域控制器 (DC) 来查询特定于远程桌面在 Active Directory 域的用户对象的配置服务 (AD DS)。 此信息显示在用户的对象属性中的 Active Directory 用户和计算机 MMC 管理单元中的远程桌面服务配置文件选项卡。

从 Windows Server 2016 开始，RCM 不再会查询 AD DS 中的用户对象。 如果您需要 RCM 查询 AD DS，因为正在使用远程桌面服务属性，必须手动启用以前的 RCM 行为。

> [!IMPORTANT]  
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)在出现问题时。

若要启用远程桌面会话主机服务器上的旧 RCM 行为，请配置以下注册表项，然后重新启动**远程桌面服务**服务：  
  - **HKEY\_本地\_MACHINE\\软件\\策略\\Microsoft\\Windows NT\\终端服务**
  - **HKEY\_本地\_MACHINE\\系统\\CurrentControlSet\\控制\\终端服务器\\WinStations\\\<Winstation 名称\>\\**  
      - 名称： **fQueryUserConfigFromDC**
      - 键入：**Reg\_DWORD**
      - 值：**1** （十进制）

若要启用旧版 RCM 行为以外的 RD 会话主机服务器的服务器上，配置以下注册表项和以下其他注册表项 （，然后重新启动服务）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

有关此行为的详细信息，请参阅 KB 3200967[更改为远程连接管理器在 Windows Server 中](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>用户不能使用智能卡登录

本文介绍三个常见的情况下在其中用户无法登录到远程桌面使用智能卡：  
  - [用户无法登录到分支机构使用只读域控制器 (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [用户无法登录到最近已更新的 Windows Server 2008 SP2 计算机](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [用户不能保持登录到已于最近更新，在 Windows 计算机和远程桌面服务服务变得无响应 （挂起）](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>不能使用与 RODC 分支机构中的智能卡登录

在使用 RODC 的分支站点将 RDSH 服务器的部署中会出现此问题。 在 RDSH 服务器承载根域中。 分支站点上的用户属于一个子域，并且使用智能卡进行身份验证。 RODC 配置为缓存用户密码 (在 RODC 所属**允许的 RODC 密码复制组**)。 当用户尝试登录到 RDSH 服务器上的会话时，他们可以接收消息，如"有人尝试的登录无效。 这是由于错误的用户名或身份验证信息。"

这是一个已知的问题的方式的根域控制器和 RODC 管理用户凭据进行加密。 根 DC 使用加密密钥来加密凭据，并在 RODC 向客户端提供的解密密钥。 但是，两个键不匹配。

若要解决此问题，请考虑更改你的 DC 拓扑关闭密码缓存在 RODC 上或通过将可写 DC 部署到分支站点。 一种替代方法是将 RDSH 服务器移动到同一个子域的用户。 另一种方法是允许用户不含智能卡登录。 任何这些解决方案可能需要折中的方案中性能或安全级别。

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>无法使用 Windows Server 2008 SP2 计算机到智能卡登录

当用户登录到已使用 KB4093227 更新的 Windows Server 2008 SP2 计算机时，会出现此问题 (2018.4B)。 当用户尝试使用智能卡登录时，在被拒绝访问消息，如"找到任何有效证书。 检查卡已正确插入和紧密。" 同时，Windows Server 计算机应用程序都会将事件记录"检索数字证书中插入的智能卡时出现错误。 无效的签名。"

若要解决此问题，请使用 2018.06 B 重新发布 KB 4093227 更新 Windows Server 计算机[Windows 远程桌面协议 (RDP) 拒绝服务漏洞在 Windows Server 2008 中的安全更新说明：2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)。

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>不能保持登录使用智能卡和远程桌面服务服务挂起状态

当用户登录到已使用 KB 4056446 更新的 Windows 或 Windows Server 计算机时，将出现此问题。 首先，用户可能能够通过使用智能卡登录到系统，但然后接收"放弃\_E\_否\_服务"错误消息。 在远程计算机可能无响应。

若要解决此问题，请重新启动远程计算机。

若要解决此问题，请使用相应的修补程序更新远程计算机：

  - Windows Server 2008 SP2:KB 4090928， [Windows 泄漏 lsm.exe 过程中的句柄和智能卡应用程序可能会显示"放弃\_E\_否\_服务"错误](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724， [2018 年 5 月 17，— KB4103724 （月度汇总预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10，版本 1607年:KB 4103720， [2018 年 5 月 17，— KB4103720 （OS 内部版本 14393.2273）](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>如果远程 PC 处于锁定状态，用户需要进行两次输入密码

当用户尝试连接到 RDP 连接不需要 NLA 部署中运行 Windows 10 版本 1709年的远程桌面时，可能出现此问题。 上述情况下，如果远程桌面已被锁定，用户需要在连接时两次输入其凭据。

若要解决此问题，请更新 Windows 10 版本 1709年计算机与 KB 4343893 [2018 年 8 月 30 日-KB4343893 (OS 生成 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893)。

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>用户无法登录并收到"身份验证错误"和"CredSSP 加密 oracle 修正"消息

当用户尝试登录使用任何版本的 Windows 从 Windows Vista SP2 和更高版本或 Windows Server 2008 SP2 和更高版本时，它们将被拒绝访问和接收消息如下所示：

  - 身份验证出错。 不支持所请求的函数。
  - 这可能是由于 CredSSP 加密 oracle 修正

"CredSSP 加密 oracle 修正"是指一组在年 3 月、 年 4 月和 2018 年 5 月发布的安全更新。 CredSSP 是处理其他应用程序的身份验证请求的身份验证提供程序。 2018 年 3 月 13，，"3B"以及后续更新解决了的攻击的攻击者可以在其中中继用户凭据在目标系统上执行代码。

初始更新添加了对新的组策略对象，支持**加密 Oracle 修正**，具有以下可能的设置：

  - **易受攻击**。 客户端应用程序使用 CredSSP 可以回退到不安全的版本中，但此行为会公开受到攻击的远程桌面。 使用 CredSSP 服务接受尚未更新的客户端。
  - **缓解**。 客户端应用程序使用 CredSSP 无法回退到不安全的版本中，但使用 CredSSP 服务接受尚未更新的客户端。
  - **强制更新客户端**。 客户端应用程序使用 CredSSP 无法回退到不安全的版本中，而使用 CredSSP 的服务不会接受未安装修补程序的客户端。 
    注意：直到所有远程主机支持的最新版本，则不应部署此设置。

8，2018 年 5 月更新更改了默认值**加密 Oracle 修正**设置从**易受攻击**到**缓解**。 此更改后，使用远程桌面客户端安装的更新无法连接到不具有 （或更新尚未重启的服务器） 的服务器。 有关效果的更新和在一直进行阻止的通信的类型的详细信息，请参阅 KB 4093492 [CredSSP 更新 CVE 2018 0886年](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解决此问题，请确保所有系统完全更新并重新启动。 有关更新和的漏洞相关的详细信息的完整列表，请参阅[CVE 2018 0886年 |CredSSP 远程代码执行漏洞](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

若要解决此问题，直到完成更新后，检查 KB 4093492 对于允许类型的连接。 如果不可行的替代方案可以考虑以下方法之一：

  - 对于受影响的客户端计算机中，设置**加密 Oracle 修正**策略回**易受攻击**。
  - 修改在以下策略**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\安全**组策略文件夹：  
      - **对远程 (RDP) 连接要求使用特定的安全层**： 设置为**已启用**，然后选择**RDP**。
      - **需要进行用户身份验证的远程连接使用网络级别身份验证**： 设置为**禁用**。
      > [!IMPORTANT]  
      > 这些修改降低你的部署的安全性。 只应临时的如果在所有使用。

有关使用组策略的详细信息，请参阅[修改阻止 GPO](#modifying-a-blocking-gpo)。

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>更新客户端计算机后，某些用户需要进行两次登录

某些 Windows 7 或 Windows 10 版本 1709年上的用户登录到远程桌面会话，并立即看到第二个登录屏幕。 如果客户端计算机收到了以下更新，会发生此问题：

  - Windows 7：KB 4103718， [，2018 年 5 月 8 日-KB4103718 （每月汇总）](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709年:KB 4103727， [，2018 年 5 月 8 日-KB4103727 （OS 内部版本 16299.431）](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解决此问题，请确保用户想要连接到 （以及 RDSH 或 RDVI 服务器） 的计算机都完全更新通过 2018 年 6 月。 这包括以下更新：

  - Windows Server 2016:KB 4284880， [2018 年 6 月 12 日 — KB4284880 （OS 内部版本 14393.2312）](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815， [2018 年 6 月 12 日 — KB4284815 （每月汇总）](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012:KB 4284855， [2018 年 6 月 12 日 — KB4284855 （每月汇总）](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826， [2018 年 6 月 12 日 — KB4284826 （每月汇总）](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2:KB4056564，[在 Windows Server 2008、 Windows Embedded POSReady 2009 和 Windows Embedded Standard 2009、windows CredSSP 远程代码执行漏洞的安全更新说明：2018 年 3 月 13日日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>用户被拒绝访问上使用远程 Credential Guard 与多个 RD 连接代理的部署

如果 Windows Defender 远程 Credential Guard 正在使用中，在高可用性部署中使用两个或多个远程桌面连接代理，将出现此问题。 用户无法登录到远程桌面。

因为远程 Credential Guard 使用 Kerberos 进行身份验证，并限制 NTLM，会出现此问题。 但是，在具有负载均衡的高可用性配置，RD 连接代理不能支持 Kerberos 操作。

如果您需要使用与负载平衡的 RD 连接代理高可用性配置，可以通过禁用远程 Credential Guard 来解决此问题。 有关如何管理 Windows Defender 远程 Credential Guard 的详细信息，请参阅[与 Windows Defender 远程 Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>在连接时，用户会收到"远程桌面服务是当前正忙"消息

若要确定相应的响应与此问题，请使用以下步骤：

1. 没有远程桌面服务服务无响应 （例如，远程桌面客户端显示为"挂起"在欢迎屏幕上）。  
      - 如果该服务变得无响应，请参阅[RDSH 服务器内存问题](#rdsh-server-memory-issue)。
      - 如果客户端似乎通常与服务交互，继续下一步。
2. 如果一个或多个用户断开连接其远程桌面会话，可以用户再次连接？  
   - 如果该服务将继续拒绝无论多少用户的连接断开其会话的连接，请参阅[RD 侦听器问题](#rd-listener-issue)。
   - 如果服务开始接受其会话已断开连接后的用户数[检查连接限制策略](#check-the-connection-limit-policy)。

### <a name="rdsh-server-memory-issue"></a>RDSH 服务器内存问题

在某些 Windows Server 2012 R2 RDSH 服务器上找到了内存泄漏。 随着时间推移，这些服务器开始拒绝远程桌面连接和本地控制台登录名与如下所示的消息：

> 因为远程桌面服务当前正忙，无法完成想要执行的任务。 请在几分钟后重试。 其他用户仍应能够登录。

远程桌面客户端尝试连接也会变得无响应。

若要解决此问题，请重新启动 RDSH 服务器。

若要解决此问题，请应用知识库 4093114 [2018 年 4 月 10 日-KB4093114 （每月汇总）] (c:file:///\\用户\\v jesits\\AppData\\本地\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\年 4 月 %2010，%202018 — KB4093114 %20\(每月 %20rollup\))，到 RDSH 服务器。

### <a name="rd-listener-issue"></a>RD 侦听器问题

已直接从 Windows Server 2008 R2 到 Windows Server 2012 R2 升级某些 RDSH 服务器或 Windows Server 2016 上记录了一个问题。 当远程桌面客户端连接到 RDSH 服务器时，RDSH 服务器创建为用户会话的 RD 侦听器。 受影响的服务器将保留数增加会导致用户连接时，RD 侦听器，但永远不会减少。

若要解决此问题，可以使用以下方法：

  - 重新启动 RDSH 服务器 RD 侦听器将计数重置
  - 修改连接限制策略，将其设置为太大的值。 有关管理连接限制策略的详细信息，请参阅[检查连接限制策略](#check-the-connection-limit-policy)。

若要解决此问题，请到 RDSH 服务器应用以下更新：

  - Windows Server 2012 R2：KB 4343891， [2018 年 8 月 30 日-KB4343891 （月度汇总预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016:KB 4343884， [2018 年 8 月 30 日-KB4343884 （OS 内部版本 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>检查连接限制策略

在单台计算机级别或通过配置组策略对象 (GPO)，可以同时进行的远程桌面连接数设置限制。 默认情况下，不设置限制。

若要检查的当前设置，并确定在 RDSH 服务器上的任何现有 Gpo，打开命令提示符窗口，以管理员身份，并输入以下命令：
  
```
gpresult /H c:\gpresult.html
```
   
此命令执行完毕后，打开 gpresult.html 后并在**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\连接**，找到**限制连接数**策略。

  - 如果为此策略设置为**禁用**，则组策略不受限制的 RDP 连接。
  - 如果为此策略设置为**已启用**，然后检查**入选 GPO**。 如果需要删除或更改连接限制，编辑此 GPO。

若要强制实施策略更改，请打开受影响的计算机上的命令提示符窗口并输入以下命令：
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>远程桌面客户端断开连接，并不能重新连接到同一个会话

远程桌面客户端将失去其连接到的远程桌面后，客户端不能立即重新连接。 用户会收到错误消息如下所示：

  - 由于安全错误，导致客户端无法连接到终端服务器。 确保你登录到网络之后, 再次尝试连接到服务器。
  - 远程桌面断开连接。 由于安全错误，导致客户端无法连接到远程计算机。 验证已登录到网络，然后再次尝试连接。

在更高版本时，远程桌面客户端重新连接到远程桌面。 但是，而不是客户端连接到原始会话，RDSH 服务器将客户端连接到的新会话。 正在检查 RDSH 服务器显示的原始会话没有输入断开连接的状态，但改为将保持活动状态。

若要解决此问题，可以启用**配置保持活动状态的连接时间间隔**中的策略**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\连接**组策略文件夹。 如果启用此策略，则必须输入保持活动状态的时间间隔。 保持活动状态的时间间隔确定何种频率，以分钟为单位，服务器将检查会话状态。

可以通过您的身份验证和配置设置的配置错误导致此问题。 在服务器级别，或通过使用 Gpo，您可以配置这些设置。 组策略设置位于**计算机配置\\管理模板\\Windows 组件\\远程桌面服务\\远程桌面会话主机\\安全**组策略文件夹。

1. 许可证服务器未正确配置为自动发现
2. 下**连接**，右键单击该连接的名称，然后单击**属性**。
3. 在中**属性**连接对话框上**常规**选项卡上，在**安全**层中，选择一种安全方法。
4. 在中**加密级别**，单击你想要的级别。 可以选择**低**，**客户端兼容**，**高**，或者**符合 FIPS**。

> [!NOTE]  
>  - 当客户端和 RD 会话主机服务器之间的通信需要最高级别的加密时，使用符合 FIPS 的加密。
>  - 在组策略中配置的任何加密级别设置重写使用远程桌面服务配置工具设置的配置。 此外，如果启用[系统加密：对加密、 哈希和签名使用 FIPS 兼容算法](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))策略，此设置将替代**设置客户端连接加密级别**策略。 系统加密策略处于**计算机配置\\Windows 设置\\安全设置\\本地策略\\安全选项**文件夹。
>  - 更改加密级别后，新的加密级别将在用户下一次登录时生效。 如果需要多个在一台服务器上的加密级别，多个网络适配器单独安装和配置每个适配器。
>  - 若要验证该证书具有相应的私钥，在远程桌面服务配置中，右键单击你想要查看证书，请选择的连接**常规**，选择**编辑**，选择你想要查看，然后选择的证书**查看证书**。 在底部**常规**选项卡上，该语句中，"您有对应于此证书的私钥"应显示。 此外可以通过使用证书管理单元中查看此信息。
>  - 符合 FIPS 标准的加密 (**系统加密：对加密、 哈希和签名使用 FIPS 兼容算法**策略或**符合 FIPS**在远程桌面服务器配置中设置) 进行加密和解密服务器与从客户端发送的数据服务器到客户端，使用联邦信息处理标准 (FIPS) 140-1 加密算法，使用 Microsoft 加密模块。 有关详细信息，请参阅[FIPS 140 验证](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation)。
>  - **高**设置从服务器到客户端和服务器到客户端使用发送的强 128 位加密的数据进行加密。
>  - **客户端兼容**设置对客户端和服务器上的客户端支持的最大密钥强度之间发送的数据进行加密。
>  - **低**设置对从客户端发送到服务器使用 56 位加密的数据进行加密。

## <a name="remote-laptop-disconnects-from-wireless-network"></a>远程便携式计算机断开与无线网络的连接

远程桌面客户端连接到便携式计算机使用 802.1x 无线网络时，可能出现此问题。 间歇性，便携式计算机断开与无线网络的连接并不会自动重新连接。

这是无线网络连接的网络身份验证设置时出现一个已知的问题**用户身份验证**。

若要解决此问题，请设置网络身份验证设置为**用户或计算机身份验证**或**计算机身份验证**。

 > [!NOTE]  
> 若要更改一台计算机上的网络身份验证设置，可能需要使用网络和共享中心控制面板以使用新的设置创建新的无线连接。

有关如何使用 Gpo 配置无线网络设置的完整说明，请参阅[配置无线网络 (IEEE 802.11) 策略](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="user-experiences-poor-performance-or-application-problems"></a>用户体验不佳的性能或应用程序问题

本部分讲述的用户在使用远程桌面功能时可能会遇到几个常见问题：

  - [间歇性地连接到 Microsoft Azure 的新虚拟机的用户体验问题](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [连接到 Windows 10 版本 1709年远程桌面用户可能会有播放视频问题](#video-playback-issues-on-windows-10-version-1709)。
  - [使用只读的用户配置文件连接到 Windows 10 的远程桌面的用户不能共享其桌面。](#desktop-sharing-issues-on-windows-10)
  - [NLA 处于禁用状态时，连接到 Windows 10 的远程桌面使用较旧版本的 Windows 10 上运行的客户端的用户体验不佳的性能](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [当用户在远程桌面会话中运行多个应用程序和断开连接并频繁地重新连接时，它们可能会遇到黑屏重新连接。](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>使用新的 Microsoft Azure 虚拟机的间歇性问题

此问题会影响已最近预配的虚拟机。 远程桌面会话在用户连接到虚拟机后，可能无法加载用户的所有设置正确。

若要解决此问题，断开与虚拟机的连接，等待至少 20 分钟，然后再次连接。

若要解决此问题，请为虚拟机，根据需要应用以下更新：

  - Windows 10 和 Windows Server 2016:KB 4343884， [2018 年 8 月 30 日-KB4343884 （OS 内部版本 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891， [2018 年 8 月 30 日-KB4343891 （月度汇总预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Windows 10 版本 1709年上的视频播放问题

当用户连接到运行 Windows 10，版本 1709年的远程计算机时，将出现此问题。 当这些用户播放视频使用 VMR9 编解码器 (视频混合使用呈现器 9)，播放机显示仅一个黑色的窗口。

这是 Windows 10 版本 1709年中的已知的问题。 Windows 10 版本 1703年中不会出现此问题。

### <a name="desktop-sharing-issues-on-windows-10"></a>桌面共享 Windows 10 上的问题

如果用户的只读的用户配置文件 （和相关的注册表配置单元） 中，如网亭方案中，将出现此问题。 当此类用户连接到远程计算机运行 Windows 10，版本 1803，它们不能共享其桌面。

若要解决此问题，请将 Windows 10 更新 4340917，应用[2018 年 7 月 24 日 — KB4340917 (OS 生成 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>如果禁用 NLA 混合版本的 Windows 10 时的性能问题

NLA 处于禁用状态，并运行 Windows 10 的远程桌面客户端计算机连接到运行不同版本的 Windows 10 的远程桌面时，会出现此问题。 在连接到运行 Windows 10，1803年版或更高版本的远程桌面时，运行 Windows 10，版本 1709年或更早版本的计算机上的远程桌面客户端用户遇到性能不佳。

这是因为在连接到 Windows 10、 1803年版或更高版本时，较旧的客户端计算机时 NLA 处于禁用状态，使用速度较慢的协议。

若要解决此问题，请应用知识库 4340917 [2018 年 7 月 24 日 — KB4340917 (OS 生成 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑色屏幕问题

在 Windows 8.0、 Windows 8.1、 Windows 10 RTM 和 Windows Server 2012 R2 中会出现此问题。 用户启动远程桌面中的多个应用程序，然后从会话断开连接。 我们会定期用户重新连接到远程桌面进行交互的应用程序，并再次然后断开的连接。 在某些时候，当用户重新连接，远程桌面会话只会显示黑色屏幕。 用户必须结束从远程计算机的控制台 （或 RDSH 服务器控制台） 的远程桌面会话。 此操作将停止应用程序。

若要解决此问题，应用根据以下更新：

  - Windows 8 和 Windows Server 2012:KB4103719， [2018 年 5 月 17，— KB4103719 （月度汇总预览）](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2:KB4103724， [2018 年 5 月 17，— KB4103724 （月度汇总预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)和 KB 4284863， [2018 年 6 月 21 日-KB4284863 （月度汇总预览）](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10：修复了在 KB4284860， [2018 年 6 月 12 日 — KB4284860 （OS 内部版本 10240.17889）](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
