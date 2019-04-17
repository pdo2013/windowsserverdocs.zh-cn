---
title: 远程桌面连接疑难解答
description: 按症状排列的疑难解答步骤
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297396"
---
# 远程桌面连接疑难解答
有关的多个最常见的远程桌面服务 (RDS) 问题的简短说明，请参阅[有关远程桌面客户端的常见问题](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq)。 本文介绍了多个更高级的方法解决连接问题。 这些过程的许多应用是否正在解决简单的配置，例如一个物理计算机连接到另一台物理计算机或更复杂的配置。 某些过程解决这些问题仅在更复杂的多用户场景中出现。 有关远程桌面组件以及它们如何协同工作的详细信息，请参阅[远程桌面服务体系结构](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)。

> [!NOTE]  
> 多个本文中描述的过程需要访问多台计算机，其中一些你可能需要远程访问。 有关远程管理工具和配置方式的详细信息，请参阅[远程服务器管理工具 (RSAT) 的 Windows 操作系统](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems)。

在以下列表中，标识的症状遇到你 （或你的用户） 的类型。

- [远程桌面客户端无法连接到远程桌面，但没有特定的症状或消息 （一般疑难解答步骤）](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [远程桌面客户端无法连接到远程桌面，并收到"没有注册类"消息](#client-cannot-connect-class-not-registered)
- [远程桌面客户端无法连接到远程桌面，并接收"没有可用许可证"或"安全错误"消息](#client-cannot-connect-no-licenses-available)
- [用户会收到"拒绝访问"消息，或必须两次提供凭据](#user-cannot-authenticate-or-must-authenticate-twice)
- [在连接时，收到"远程桌面服务正忙"消息](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [远程桌面客户端将断开连接，不能重新连接到同一个会话](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [用户连接到无线网络时，通过远程便携式计算机和笔记本电脑然后将与网络断开连接](#remote-laptop-disconnects-from-wireless-network)。
- [用户体验不佳的性能或与远程应用程序问题](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> 有关当前 RDP 断开连接代码的列表，请参阅[ExtendedDisconnectReasonCode 枚举](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode)。 

## 没有特定的症状或消息 （一般疑难解答步骤）

当远程桌面客户端无法连接到远程桌面，但不提供消息或其他症状可帮助我们确定的原因，请使用这些步骤。 若要解决许多此类问题的最常见原因，请使用以下方法：

- [检查 RDP 协议的状态](#check-the-status-of-the-rdp-protocol)
- [检查 RDP 服务的状态](#check-the-status-of-the-rdp-services)
- [检查 RDP 侦听器正常工作](#check-that-the-rdp-listener-is-functioning)
- [检查 RDP 侦听器端口](#check-the-rdp-listener-port)

### 检查 RDP 协议的状态

#### 检查本地计算机上的 RDP 协议的状态

若要检查并更改本地计算机上 RDP 协议的状态，请参阅[如何启用远程桌面](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop)。

> [!NOTE]  
> 如果远程桌面选项不可用，请参阅[检查组策略对象是否阻止 RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer)。

#### 检查远程计算机上的 RDP 协议的状态

> [!IMPORTANT]  
> 请遵循此部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 之前你对其进行修改，[备份还原的注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)中的情况下会发生问题。

若要检查并更改远程计算机上 RDP 协议的状态，请使用网络注册表连接：

1. 选择**开始菜单**，选择**运行**，然后输入**regedt32**。
2. 在注册表编辑器中，选择**文件**，然后选择**连接网络注册表**。
3. 在**选择计算机**对话框中，输入远程计算机的名称，选择**检查名称**，然后选择**确定**。
4. 导航到**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal 服务器**。  
   ![注册表编辑器中，显示 fDenyTSConnections 条目](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - 如果**fDenyTSConnections**密钥的值为**0**，那么将启用 RDP
   - 如果**fDenyTSConnections**密钥的值为**1**，则会禁用 RDP
5. 若要启用 RDP，请将从**1**的**fDenyTSConnections**的值更改为**0**。

#### 检查组策略对象 (GPO) 是否在本地计算机上阻止 RDP

如果你不能打开 RDP 在用户界面或已更改后， **fDenyTSConnections**的值会恢复为**1** ，GPO 可能重写计算机级别的设置。

若要查看本地计算机上的组策略配置，打开命令提示符窗口作为管理员，并输入以下命令：
```
gpresult /H c:\gpresult.html
```
此命令完成后，打开 gpresult.html。 在**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Connections**，找到**允许用户使用远程桌面服务的远程连接**策略。

- 此策略设置是否**已启用**，组策略不会阻止 RDP 连接。
- 如果**禁用**此策略设置，检查**获胜 GPO**。 这是，阻止 RDP 连接的 GPO。
![Gpresult.html，在其中示例类别域级 GPO * * 块 RDP * * 禁用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Gpresult.html，在其中示例类别 * * 本地组策略 * * 禁用 RDP。](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### 查看 GPO 是否在远程计算机上阻止 RDP

若要检查远程计算机上的组策略配置，该命令为本地计算机几乎相同：
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
此命令将生成的文件 (**gpresult-\<computer name\>.html**) 作为本地计算机版本 (**gpresult.html**) 使用使用相同的信息格式。

#### 修改阻止 GPO

你可以修改这些设置的组策略对象编辑器 (GPE) 和组策略管理控制台 (使用 GPM) 中。 有关如何使用组策略的详细信息，请参阅[高级组策略管理](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/)。

若要修改阻止策略，请使用以下方法之一：

- 在 GPE，访问 （例如本地或域），相应的 GPO 级别，然后导航至**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Connections**\\**允许用户可以通过使用远程桌面服务远程连接**。  
   1. 将该策略设置为**启用**或**未配置**。
   2. 受影响的计算机上打开命令提示符窗口作为管理员，并运行**gpupdate /force**命令。
- 在使用 GPM，导航到阻止策略应用到受影响的计算机中，OU 并从 OU 中删除该策略。

### 检查 RDP 服务的状态

在本地 （客户端） 计算机和远程 （目标） 计算机上，应运行以下服务：

- 远程桌面服务 (TermService)
- 远程桌面服务用户模式端口重定向程序 (UmRdpService)

你可以使用服务 mmc 管理单元本地或远程管理的服务。 你还可以使用 PowerShell，本地或远程 （如果远程计算机配置为接受远程 PowerShell 命令）。

![在服务 MMC 管理单元中的远程桌面服务。 未修改的默认服务设置。](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

任何一种在计算机上，如果未运行一个或两个服务，启动它们。

> [!NOTE]  
> 如果启动远程桌面服务服务，请单击**是**自动重新启动远程桌面服务用户模式端口重定向程序服务。

### 检查 RDP 侦听器正常工作

> [!IMPORTANT]  
> 请遵循此部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 之前你对其进行修改，[备份还原的注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)中的情况下会发生问题。

#### 检查 RDP 侦听程序的状态

此过程中，使用具有管理权限的 PowerShell 实例。 对于本地计算机，你还可以使用具有管理权限在命令提示符。 但是，此过程使用 PowerShell，因为的相同命令工作都本地和远程。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入**Enter-pssession-ComputerName \<computer name\>**。
2. 输入**qwinsta**。 
    ![Qwinsta 命令列出了在计算机的端口上侦听的进程。](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. 如果列表包括**rdp tcp**状态为**侦听**，工作 RDP 侦听器。 进入[检查 RDP 侦听器端口](#check-the-rdp-listener-port)。 否则，请继续在步骤 4。
4. 将工作计算机中导出 RDP 侦听器配置。
    1. 登录到具有受影响的计算机，如具有相同的操作系统版本的计算机和访问该计算机的注册表 （例如，通过使用注册表编辑器）。
    2. 导航到以下注册表项：  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP Tcp**
    3. 将此条目导出到.reg 文件。 例如，在注册表编辑器中，右键单击条目，选择**导出**，然后输入文件名导出的设置。
    4. 导出的.reg 文件复制到受影响的计算机。
5. 若要导入 RDP 侦听器配置、 打开 PowerShell 窗口受影响的计算机上具有管理权限 （或打开 PowerShell 窗口和远程连接到受影响的计算机）。
   1. 若要备份现有的注册表项，输入以下命令：  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. 若要删除现有的注册表项，输入以下命令：  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. 若要导入新的注册表项，然后重新启动该服务，输入以下命令：  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      其中 \<filename\> 是导出的.reg 文件的名称。
6. 通过尝试远程桌面连接再次测试配置。 如果仍无法连接，重新启动受影响的计算机。
7. 如果你仍无法连接，[检查 RDP 自签名证书的状态](#check-the-status-of-the-rdp-self-signed-certificate)。

#### 检查 RDP 自签名证书的状态

1. 如果仍无法连接，打开证书 mmc 管理单元。 当您选择要管理，选择**计算机帐户**，然后依次选择受影响的计算机的证书存储。
2. 在**远程桌面**下的**证书**文件夹中，删除 RDP 自签名的证书。 
    ![MMC 证书管理单元中的远程桌面证书。](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. 在受影响的计算机上重新启动远程桌面服务服务。
4. 刷新证书管理单元中。
5. 如果 RDP 自签名的证书不被重新创建，[检查 MachineKeys 文件夹的权限](#check-the-permissions-of-the-machinekeys-folder)。

#### 检查 MachineKeys 文件夹的权限

1. 在受影响的计算机中，打开资源管理器，然后导航到**C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**。
2. 右键单击**MachineKeys**，选择**属性**、 选择**安全**，然后依次选择**高级**。
3. 请确保配置了以下权限：
      - Builtin\\Administrators： 完全控制
      - 每个人都： 读取、 写入

### 检查 RDP 侦听器端口

在本地 （客户端） 计算机和远程 （目标） 计算机上，应在 RDP 侦听器侦听端口 3389 上。 没有其他应用程序应使用此端口。

> [!IMPORTANT]  
> 请遵循此部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 之前你对其进行修改，[备份还原的注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)中的情况下会发生问题。

若要检查或更改 RDP 端口，使用注册表编辑器：

1. 选择开始菜单，选择**运行**，然后输入**regedt32**。
      - 若要连接到远程计算机，选择**文件**，然后依次选择**连接网络注册表**。
      - 在**选择计算机**对话框中，输入远程计算机的名称，选择**检查名称**，然后选择**确定**。
2. 打开注册表并导航到**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**。 
    ![RDP 协议端口号子项。](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. 如果**端口号** **3389**以外的值，则将其更改为**3389**。 
   > [!IMPORTANT]  
    > 你可以运行远程桌面服务使用另一个端口。 但是，我们不建议执行此操作。 此类配置疑难解答已超出本文的范围。
4. 更改的端口号后，重启远程桌面服务服务。

#### 检查另一个应用程序不尝试使用相同的端口

此过程中，使用具有管理权限的 PowerShell 实例。 对于本地计算机，你还可以使用具有管理权限在命令提示符。 但是，此过程使用 PowerShell，由于本地和远程的工作原理相同的命令。

1. 打开 PowerShell 窗口。 若要连接到远程计算机，请输入**Enter-pssession-ComputerName \<computer name\>**。
2. 输入以下命令：  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Netstat 命令将生成端口和收听它们的服务的列表。](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. 寻找状态为**侦听**TCP 端口 3389 的条目 （或分配的 RDP 端口）。 
    > [!NOTE]  
   > 进程或服务使用该端口的 PID （进程标识符） 显示在 PID 列下。
1. 若要确定哪些应用程序使用端口 3389 （或分配的 RDP 端口），输入以下命令：  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Tasklist 命令报告特定进程的详细的信息。](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. 查找 PID 数 （从**netstat**输出） 的端口与相关联的条目。 服务或进程与该 PID 相关联的显示在右侧。
1. 如果应用程序或远程桌面服务 (TermServ.exe) 以外的服务使用的端口，你可以通过使用以下方法之一来解决冲突：
      - 配置另一个应用程序或服务使用其他端口 （推荐）。
      - 卸载的其他应用程序或服务。
      - 配置 RDP 使用不同的端口，并重新启动 （不推荐） 远程桌面服务服务。

#### 检查是否防火墙阻止 RDP 端口

使用**psping**工具来测试是否可以通过使用端口 3389 访问受影响的计算机。

1. 在计算机上不同于受影响的计算机，下载**psping**从<https://live.sysinternals.com/psping.exe>。
2. 打开以管理员身份命令提示符窗口、 更改为安装**psping**的目录，然后输入以下命令：  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. 检查结果如下所示的**psping**命令的输出：  
      - **连接到 \<computer IP\>**： 远程计算机是否可以访问。
      - **（0%丢失）**： 所有连接尝试成功。
      - **远程计算机拒绝网络连接**： 远程计算机不可访问。
      - **（100%丢失）**： 所有连接尝试失败。
1. 在多台计算机来测试它们能够连接到受影响的计算机上运行**psping** 。
1. 请注意受影响的计算机是否阻止来自所有其他计算机、 一些其他计算机或其他只有一台计算机的连接。
1. 建议在后续步骤：
      - 咨询网络管理员以验证网络允许 RDP 通信到受影响的计算机。
      - 调查源计算机和受影响的计算机 （包括在受影响的计算机上的 Windows 防火墙） 之间的任何防火墙的配置以确定是否防火墙阻止 RDP 端口。

## 客户端无法连接，"没有注册类"

当你尝试使用运行 Windows 10 版本 1709年的客户端连接到远程计算机或更高版本，客户端可能无法连接到远程桌面会话主机服务器报告一条消息时包含"类未注册 (0x80040154) 中"错误代码。

如果尝试连接的用户具有强制性用户配置文件，将出现此问题。 若要解决此问题，请安装[2018 年 7 月 24 日-KB4338817 (操作系统生成 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) Windows 10 更新。

## 客户端无法连接，没有可用的许可证

这种情况下适用于包含 RDSH 服务器和远程桌面授权服务器的部署。

确定哪种类型的用户所看到的行为：

- 由于没有许可证有或没有许可证服务器可以会话被中断
- 由于安全错误访问被拒绝

登录到 RD 会话主机作为域管理员，并打开 RD 许可证诊断程序。 查看以下信息：

  - 远程宽限期已到期桌面会话主机服务器，但尚未配置的任何许可证服务器 RD 会话主机服务器。 向 RD 会话主机服务器的连接将被拒绝，除非许可证服务器配置为 RD 会话主机服务器。
  - 许可证服务器 \<computer name\> 不可用。 这可能由网络连接问题、 远程桌面授权服务已停止在许可证服务器上，或在计算机上不再安装 RD 授权。

这些问题倾向于与以下用户消息相关联：

  - 由于不有这台计算机的任何远程桌面客户端访问许可证，远程会话被中断。
  - 由于没有可用于提供许可证远程桌面许可证服务器，远程会话被中断。

在此情况下[配置 RD 授权服务](#configure-the-rd-licensing-service)。

如果 RD 许可证诊断程序列出了其他问题，如"RDP 协议组件 X.224 协议流中检测到错误，并且已断开连接的客户端"那里可能会影响许可证证书出现问题。 此类问题倾向于与用户消息，如下所示：

由于安全错误，客户端无法连接到终端服务器。 确保之后，登录到网络，请尝试重新连接到服务器。

在此情况下，[刷新 X509 证书注册表项](#refresh-the-x509-certificate-registry-keys)。

### 将 RD 授权服务配置

以下过程使用服务器管理器进行配置更改。 有关如何配置和使用服务器管理器的信息，请参阅[服务器管理器](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager)。

1. 打开服务器管理器，然后导航到**远程桌面服务**。
2. 在**部署概述**，选择**任务**，然后选择**编辑部署属性**。
3. 选择**RD 授权**，，然后选择你的部署 （**每个设备**或**每用户**） 的相应许可模式。
4. 输入你 RD 许可证服务器的完全限定的域名 (FQDN)，然后选择**添加**。
5. 如果你有多个 RD 许可证服务器，请为每个服务器重复步骤 4。 
    ![RD 许可证服务器配置选项在服务器管理器。](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### 刷新 X509 证书注册表项

> [!IMPORTANT]  
> 请遵循此部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 之前你对其进行修改，[备份还原的注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)中的情况下会发生问题。

若要解决此问题，备份，然后删除 X509 证书注册表项，重启计算机，然后重新激活 RD 许可服务器。 若要执行此操作，请遵循以下步骤。

> [!NOTE]  
> 在每个 RDSH 服务器上执行以下过程。

1. 打开注册表编辑器，然后导航到**HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**。
2. 在注册表菜单上，选择**导出注册表文件**。
3. 在**文件名称**框中，键入**导出证书**，然后选择**保存**。
4. 右键单击每个下列值，选中**删除**，然后选择**** 以确认删除:  
      - **证书**
      - **X509 证书**
      - **X509 证书 ID**
      - **X509 Certificate2**
5. 退出注册表编辑器，然后重启 RDSH 服务器。

## 用户无法进行身份验证或必须进行身份验证两次

有几个问题可能会导致问题，会影响用户身份验证。 本部分可解决以下情况：

  - [用户将被拒绝访问"限制的登录类型"消息](#access-denied-restricted-type-of-logon)
  - [用户或应用程序被拒绝访问与事件 16965，"远程调用 SAM 数据库已被拒绝"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [用户无法登录使用智能卡](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [如果远程电脑锁定时，用户需要输入密码两次](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [用户无法登录，并接收"身份验证错误"和"CredSSP 加密 oracle 修正"消息](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [某些用户更新客户端计算机后需要两次登录](#after-you-update-client-computers-some-users-need-to-sign-in-twice)。
  - [用户被拒绝访问使用具有多个 RD 连接代理 RemoteGuard 部署](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### 访问被拒绝，受限的登录类型

在此情况下，Windows 10 用户尝试连接到 Windows 10 或 Windows Server 2016 的计算机被拒绝访问以下消息：

> 远程桌面连接：  
> 系统管理员已限制的登录类型 (网络或交互式)，你可以使用。 以寻求帮助，请联系你的系统管理员或技术支持。

当网络级别身份验证 (NLA) 需要对于 RDP 连接，并且用户不是**远程桌面用户**组的成员，将出现此问题。 如果尚未**从网络访问此计算机**用户权限分配**远程桌面用户**组，也可能发生此问题。

若要解决此问题，请按照以下步骤之一：

  - [修改用户的组成员身份或用户权限分配](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 关闭 NLA （不推荐）。
  - 使用 Windows 10 以外的远程桌面客户端。 例如，Windows 7 客户端没有此问题。

#### 修改用户的组成员身份或用户权限分配

如果此问题影响单个用户，此问题的最简单解决方案是将用户添加到**远程桌面用户**组。

如果用户已经是此组的成员 （或如果多个组成员具有相同问题），请检查远程 Windows 10 或 Windows Server 2016 计算机上的用户权限配置。

1. 打开组策略对象编辑器 (GPE) 并连接到远程计算机的本地策略。
2. 导航到**计算机 Configuration\\Windows Settings\\Security Settings\\Local Policies\\User 权限分配**，右键单击**从网络访问此计算机**，然后依次选择**属性**。
3. 检查**远程桌面用户**（或父组） 的用户和组的列表。
4. 如果该列表不包括**远程桌面用户**（或父组，如**每个人都**） 你需要将其添加到列表 （如果你有多个或两个计算机部署中，使用组策略对象）。  
    例如，**从网络访问此计算机**的默认成员身份包括**每位**。 如果你的部署使用组策略对象来删除**所有人**，你可能需要通过更新的组策略对象，以添加**远程桌面用户**还原的访问权限。

### 访问被拒绝，远程调用 SAM 数据库已被拒绝

此行为是最有可能在域控制器运行 Windows Server 2016 或更高版本，且用户尝试使用自定义的连接应用连接时发生。 特别是，访问 Active Directory 中的用户的配置文件信息的应用程序将被拒绝访问。

Windows 使用更改产生此行为。 在 Windows Server 2012 R2 及更早版本的版本中，当用户登录到远程桌面的远程连接管理器 (RCM) 联系人的域控制器 (DC) 来查询特定于远程桌面 Active Directory 域中的用户对象的配置服务 (AD DS)。 此信息会显示在用户的对象属性的 Active Directory 用户和计算机 mmc 管理单元中的远程桌面服务配置文件选项卡。

从 Windows Server 2016 开始，RCM 不再查询 AD DS 中的用户对象。 如果你需要 RCM 查询 AD DS，因为你正在使用的远程桌面服务属性，你必须手动启用以前的 RCM 行为。

> [!IMPORTANT]  
> 请遵循此部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 之前你对其进行修改，[备份还原的注册表](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22)中的情况下会发生问题。

若要启用 RD 会话主机服务器上的旧 RCM 行为，配置以下注册表项，并重新启动**远程桌面服务**服务：  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal 服务**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - 名称： **fQueryUserConfigFromDC**
      - 类型： **Reg\_DWORD**
      - 值： **1** （十进制）

若要启用旧版 RCM 行为以外 RD 会话主机服务器的服务器上，配置这些注册表项和以下其他注册表项 （，然后重启服务）：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal 服务器**

有关此行为的详细信息，请参阅 KB 3200967[到远程连接管理器在 Windows Server 中的更改](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

### 用户无法登录使用智能卡

本文介绍三个常见的情况下，用户无法登录到远程桌面使用智能卡：  
  - [用户无法登录到分支机构使用只读域控制器 (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [用户无法登录到最近已更新的 Windows Server 2008 SP2 计算机](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [用户不能保持登录到最近更新后，Windows 计算机和远程桌面服务将不响应 （挂起）](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### 不能使用在分支机构 RODC 与智能卡登录

包括使用 RODC 分支站点 RDSH 服务器的部署中出现此问题。 RDSH 服务器托管在根域中。 分支站点上的用户属于子域，并使用智能卡进行身份验证。 RODC 配置为缓存用户密码 （RODC 所属**允许 RODC 密码复制组**）。 当用户尝试登录到 RDSH 服务器上的会话时，它们可以接收消息，如"登录尝试无效。 这可以是由于错误的用户名或身份验证信息。"

这是一个已知的问题的方式的根域控制器和 RODC 管理的用户凭据的加密。 根 DC 使用加密密钥的凭据进行加密并 RODC 到客户端提供的解密密钥。 但是，这两个密钥不匹配。

若要解决此问题，请考虑更改 DC 拓扑通过关闭密码缓存在 RODC 上或通过将可写 DC 部署到分支站点。 一种方法是将 RDSH 服务器移动到与用户位于同一子域。 另一个方法是允许用户登录而无需智能卡。 任何这些解决方案可能需要在性能或的安全级别的威胁。

#### 无法登录到 Windows Server 2008 SP2 计算机智能卡

当用户登录到 KB4093227 已更新的 Windows Server 2008 SP2 计算机时，会出现此问题 (2018.4B)。 当用户尝试使用智能卡登录时，他们访问被拒绝消息，如"未找到有效证书。 检查卡已正确插入和紧密。" 同时，Windows Server 计算机将应用程序事件记录"从插入的智能卡检索数字证书时出现错误。 无效的签名。"

若要解决此问题，使用 2018.06Bre 更新与 Windows Server 计算机的版本的 KB 4093227 [Windows 远程桌面协议 (RDP) 拒绝服务漏洞，Windows Server 2008 中的安全更新的说明： 2018 年 4 月 10 日](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008)。

#### 不能保持使用智能卡和远程桌面服务服务挂起签名

在用户登录到 KB 4056446 已更新的 Windows 或 Windows Server 计算机上时，将出现此问题。 首先，用户可能无法登录到系统使用智能卡，但然后接收"SCARD\_E\_NO\_SERVICE"错误消息。 远程计算机可能停止响应。

若要解决此问题，请重新启动远程计算机。

若要解决此问题，请使用相应的修补程序更新远程计算机：

  - Windows Server 2008 SP2: KB 4090928， [Windows 泄漏 lsm.exe 过程中的句柄和智能卡应用程序可能会显示"SCARD\_E\_NO\_SERVICE"错误](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724， [2018 年 5 月 17 日-KB4103724 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 和 Windows 10 版本 1607年: KB 4103720 [2018 年 5 月 17 日-KB4103720 (操作系统生成 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### 如果远程电脑锁定时，用户需要输入密码两次

当用户尝试连接到远程桌面部署 RDP 连接不需要 NLA 中运行 Windows 10 版本 1709年，可能发生此问题。 在这些情况下远程桌面已被锁定，如果用户需要在两次连接时输入其凭据。

若要解决此问题，请使用 KB 4343893 更新 Windows 10 版本 1709年计算机[2018 年 8 月 30 日-KB4343893 (操作系统生成 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893)。

### 用户无法登录，并接收"身份验证错误"和"CredSSP 加密 oracle 修正"消息

当用户尝试登录使用任何从 Windows Vista SP2 和更高版本或 Windows Server 2008 SP2 的 Windows 版本和更高版本时，它们被拒绝访问，并且收到消息如下所示：

  - 身份验证错误。 不支持请求的函数。
  - 这可能是由于 CredSSP 加密 oracle 修正

"CredSSP 加密 oracle 修正"指的是一组在 3 月，4 月和 2018 年 5 月发布的安全更新。 CredSSP 是处理其他应用程序的身份验证请求身份验证提供程序。 13，2018 年 3 月，"3B"和后续更新解决的攻击的攻击者可能在其中中继要在目标系统上执行代码的用户凭据。

初始更新添加了对新组策略对象，**加密 Oracle 修正**，具有以下可能的设置的支持：

  - **易受攻击**。 客户端应用程序使用 CredSSP 可以回退到不安全的版本中，但此行为公开对攻击的远程桌面。 使用 CredSSP 服务接受尚未更新的客户端。
  - **缓解**。 客户端应用程序使用 credssp 都无法回退到不安全的版本中，但使用 CredSSP 服务接受尚未更新的客户端。
  - **强制更新客户端**。 客户端应用程序使用 credssp 都无法回退到不安全的版本中，并使用 CredSSP 的服务不会接受未安装修补程序客户端。 
    注意： 此设置不应部署直到所有远程主机支持的最新版本。

2018 年 5 月 8 日，更新变为默认**加密 Oracle 修正**设置从**易受攻击****缓解**。 通过这项更改位置中，已更新的远程桌面客户端无法连接到服务器没有它们 （或更新没有已重新启动的服务器）。 有关更新和类型的通信，便会阻止效果的详细信息，请参阅 KB 4093492， [CVE 2018 0886 CredSSP 更新](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解决此问题，请确保所有系统的完全更新和重启。 有关漏洞的详细信息和更新的完整列表，请参阅[CVE 2018 0886年 |CredSSP 远程代码执行漏洞](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886)。

若要解决此问题，直到完成更新后，检查 KB 4093492 对允许类型的连接。 如果有任何可行的替代项，你可能会考虑以下方法之一：

  - 对于受影响的客户端计算机中，将**加密 Oracle 修正**策略设置回**易受攻击**。
  - 修改**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Security**组策略文件夹中的以下策略：  
      - **要求使用的远程 (RDP) 连接的特定的安全层**： 设置为**已启用**，然后选择**RDP**。
      - **需要通过使用网络级别身份验证的远程连接的用户身份验证**： 将设置为**禁用**。
      > [!IMPORTANT]  
      > 这些修改减少部署的安全性。 它们仅应临时的如果你在所有使用它们。

有关使用组策略的详细信息，请参阅[修改阻止 GPO](#modifying-a-blocking-gpo)。

### 更新客户端计算机后，某些用户需要两次登录

某些 Windows 7 或 Windows 10 版本 1709年上的用户登录到远程桌面会话，并立即查看第二个登录屏幕。 如果客户端计算机已收到的以下更新，会出现此问题：

  - Windows 7: KB 4103718， [2018 年 5 月 8 日-KB4103718 （每月汇总）](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 版本 1709年: KB 4103727， [2018 年 5 月 8 日-KB4103727 （操作系统内部 16299.431）](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

若要解决此问题，请确保，通过 2018 年 6 月，完全更新用户想要连接到 （以及 RDSH 或 RDVI 服务器） 的计算机。 这包括下列更新：

  - Windows Server 2016: KB 4284880， [2018 年 6 月 12 日-KB4284880 （操作系统内部 14393.2312）](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815， [2018 年 6 月 12 日-KB4284815 （每月汇总）](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855， [2018 年 6 月 12 日-KB4284855 （每月汇总）](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826， [2018 年 6 月 12 日-KB4284826 （每月汇总）](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564[在 Windows Server 2008、 Windows Embedded POSReady 2009 和 Windows Embedded 标准 2009年 CredSSP 远程代码执行漏洞的安全更新的说明： 2018 年 3 月 13 日](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### 用户被拒绝访问上的部署中使用远程 Credential Guard 具有多个 RD 连接代理

使用 Windows Defender Remote Credential Guard 是否在使用两个或多个远程桌面连接代理高可用性部署中出现此问题。 用户无法登录到远程桌面。

因为远程 Credential Guard 使用 Kerberos 进行身份验证，并限制 NTLM 出现此问题。 但是，在具有负载平衡的高可用性配置，RD 连接代理无法支持 Kerberos 操作。

如果你需要使用负载平衡 RD 连接代理高可用性配置，可以通过禁用远程 Credential Guard 来解决此问题。 有关如何管理 Windows Defender Remote Credential Guard 的详细信息，请参阅[与 Windows Defender Remote Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。

## 有关连接，用户会收到"远程桌面服务正忙"消息

若要确定适当的响应此问题，请使用以下步骤：

1. 没有远程桌面服务服务意外响应 （例如，远程桌面客户端显示在欢迎屏幕"挂起"）。  
      - 如果该服务意外响应，请参阅[RDSH 服务器内存问题](#rdsh-server-memory-issue)。
      - 如果客户端似乎正常情况下与服务交互，则继续下一步。
2. 如果一个或多个用户断开连接的远程桌面会话，用户再次连接？  
   - 如果该服务将继续拒绝无论多少用户断开连接的会话的连接，请参阅[RD 侦听器问题](#rd-listener-issue)。
   - 如果该服务开始接受连接的用户数后再次断开其会话，[检查连接限制策略](#check-the-connection-limit-policy)。

### RDSH 服务器内存问题

在某些 Windows Server 2012 R2 RDSH 服务器上发现内存泄漏。 随着时间的推移这些服务器开始拒绝远程桌面连接和本地控制台登录使用以下信息：

> 由于远程桌面服务当前正在无法完成尝试执行的任务。 请在几分钟后重试。 其他用户仍应能够登录。

远程桌面客户端尝试连接还没有响应。

若要解决此问题，重启 RDSH 服务器。

若要解决此问题，应用 KB 4093114 [2018 年 4 月 10 日-KB4093114 （每月汇总）] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly%20Rollup\))，到 RDSH 服务器。

### RD 侦听器问题

已直接从 Windows Server 2008 R2 至 Windows Server 2012 R2 升级一些 RDSH 服务器或 Windows Server 2016 上已注意到问题。 当远程桌面客户端连接到 RDSH 服务器时，RDSH 服务器为用户会话创建 RD 侦听器。 受影响的服务器保留用户连接，随着 RD 侦听器但永远不会减少的计数。

若要解决此问题，你可以使用以下方法：

  - 重启 RDSH 服务器重置的 RD 侦听器计数
  - 修改连接限制策略，将其设置为一个非常大的值。 有关管理连接限制策略的详细信息，请参阅[检查连接限制策略](#check-the-connection-limit-policy)。

若要解决此问题，请到 RDSH 服务器应用下列更新：

  - Windows Server 2012 R2: KB 4343891， [2018 年 8 月 30 日-KB4343891 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884， [2018 年 8 月 30 日-KB4343884 （操作系统内部 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### 检查连接限制策略

你可以在单个计算机级别或通过配置组策略对象 (GPO) 上同时远程桌面连接数设置限制。 默认情况下，未设置限制。

若要检查的当前设置并确定 RDSH 服务器上的任何现有 Gpo，打开以管理员身份命令提示符窗口并输入以下命令：
  
```
gpresult /H c:\gpresult.html
```
   
此命令完成后，打开 gpresult.html，然后在**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Connections**，查找**限制连接数**策略。

  - 如果**禁用**此策略设置，组策略将不会限制 RDP 连接。
  - 此策略设置是否**已启用**，然后检查**获胜 GPO**。 如果你需要删除或更改连接限制，编辑此 GPO。

若要强制执行策略更改，打开命令提示符窗口上受影响的计算机，并输入以下命令：
  
```
gpupdate /force
```
  
## RD 客户端将断开连接，不能重新连接到同一个会话

远程桌面客户端失去与远程桌面的连接后，客户端无法立即重新连接。 用户将收到错误消息如下所示：

  - 由于安全错误，客户端无法连接到终端服务器。 确保之后，登录到网络，请尝试重新连接到服务器。
  - 断开连接的远程桌面。 由于安全错误，客户端无法连接到远程计算机。 验证登录到网络，然后尝试重新连接。

在更高版本时，远程桌面客户端重新连接到远程桌面。 但是，而不是连接到原始会话的客户端，RDSH 服务器客户端连接到新的会话。 检查 RDSH 服务器可以发现的原始会话未输入断开连接的状态，但改为将保持活动状态。

若要解决此问题，你可以启用**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Connections 中的**配置保持连接时间间隔**策略**组策略文件夹。 如果启用此策略，你必须输入保持活动状态的时间间隔。 保持活动状态的时间间隔确定频率，以分钟为单位，服务器检查会话状态。

此问题可能引起的错误配置的身份验证和配置设置。 在服务器级别，或通过使用 Gpo，你可以配置这些设置。 组策略设置可在**计算机配置 \\ 管理模板 \\windows Components\\Remote 桌面 Services\\Remote 桌面会话 Host\\Security**组策略文件夹中。

1. 在 RD 会话主机服务器上，打开远程桌面会话主机配置。
2. 在**连接**中，右键单击连接的名称，然后单击**属性**。
3. 在**常规**选项卡**安全**层上的连接，**属性**对话框中选择安全方法。
4. 在**加密级别**上，依次单击所需的级别。 你可以选择**低**、**客户端兼容**、**高**，或**符合 FIPS**。

> [!NOTE]  
>  - 当客户端和 RD 会话主机服务器之间的通信需要最高级别的加密时，使用符合 FIPS 加密。
>  - 配置组策略中的任何加密级别设置将覆盖你使用的远程桌面服务配置工具设置的配置。 此外，如果你启用[系统加密： 使用 FIPS 兼容算法用于加密、 哈希和签名](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\))策略，此设置将覆盖**设置客户端连接加密级别**策略。 系统加密策略是在**计算机 Configuration\\Windows Settings\\Security Settings\\Local Policies\\Security 选项**文件夹中。
>  - 更改加密级别时，新的加密级别将在用户登录下一步时生效。 如果你需要多个级别的一台服务器上的加密，安装多个网络适配器并分别配置每个适配器。
>  - 若要验证该证书具有相应的私钥，远程桌面服务配置中，右键单击你想要查看的证书，选择**常规**、 选择**编辑**，选择你希望的证书的连接查看，，然后选择**查看证书**。 在**常规**选项卡的底部，语句，"你有一个私钥对应于此证书"应显示。 你还可以通过使用证书管理单元查看此信息。
>  - 符合 FIPS 的加密 (**系统加密： 使用 FIPS 兼容算法用于加密、 哈希和签名**远程桌面服务器配置中的**符合 FIPS**设置或策略) 进行加密和解密发送的数据客户端服务器和客户端，与联邦信息处理标准 (FIPS) 140-1 加密算法，使用 Microsoft 加密模块服务器。 有关详细信息，请参阅[FIPS 140 验证](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation)。
>  - **高**设置加密数据使用的强 128 位加密发送到服务器客户端从和到客户端服务器。
>  - **客户端兼容**设置对客户端和客户端支持最大密钥强度的服务器之间发送的数据进行加密。
>  - **低**设置对从客户端发送到服务器使用 56 位加密的数据进行加密。

## 远程便携式计算机将与无线网络断开连接

远程桌面客户端连接到一台笔记本电脑使用 802.1 x 无线网络时，可能发生此问题。 间歇性，笔记本电脑无线网络断开连接，并且不会自动重新连接。

这是一个已知的问题，时会出现的无线网络连接的网络身份验证设置为**用户身份验证**。

若要解决此问题，请对**用户或计算机身份验证**或**计算机身份验证**设置网络身份验证设置。

 > [!NOTE]  
> 若要更改一台计算机上的网络身份验证设置，你可能需要使用网络和共享中心控制面板使用新的设置创建新的无线连接。

有关如何配置使用 Gpo 的无线网络设置的完整说明，请参阅[配置无线网络 (IEEE 802.11) 策略](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## 用户体验不佳的性能或应用程序问题

本部分可解决多个用户使用远程桌面的功能时可能遇到的常见问题：

  - [连接到新的虚拟机间歇性 Microsoft Azure 的用户遇到问题](#intermittent-problems-with-new-microsoft-azure-virtual-machines)。
  - [连接到 Windows 10 版本 1709年远程桌面版的用户可能会遇到问题播放视频](#video-playback-issues-on-windows-10-version-1709)。
  - [具有只读的用户配置文件连接到 Windows 10 远程桌面版的用户无法共享其桌面。](#desktop-sharing-issues-on-windows-10)
  - [使用较旧版本的 Windows 10 运行的客户端的 Windows 10 远程桌面连接的用户体验不佳的性能时 NLA 处于禁用状态](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [当用户在远程桌面会话中运行多个应用程序和断开连接并重新连接频繁时，它们可能会遇到上重新连接黑屏。](#black-screen-issue)

### 与新的 Microsoft Azure 虚拟机经常出现问题

此问题影响最近已预配的虚拟机。 远程桌面会话用户连接到虚拟机后，可能无法加载用户的所有设置正确。

若要解决此问题，从虚拟机断开连接，等待至少 20 分钟，然后重新连接。

若要解决此问题，请到的虚拟机，根据需要应用的以下更新：

  - Windows 10 和 Windows Server 2016: KB 4343884， [2018 年 8 月 30 日-KB4343884 （操作系统内部 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891， [2018 年 8 月 30 日-KB4343891 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### Windows 10 版本 1709年上的视频播放问题

当用户连接到正在运行 Windows 10 版本 1709年的远程计算机时，将出现此问题。 当这些用户播放视频使用 VMR9 (视频 MixingRenderer9) 编解码器，玩家显示仅一个黑色窗口。

这是 Windows 10 版本 1709年中的已知的问题。 在 Windows 10 版本 1703年中不会出现问题。

### 桌面共享 Windows 10 上的问题

当用户具有一个只读的用户配置文件 （和关联的注册表配置单元），例如在网亭方案中，将出现此问题。 当此类用户连接到远程计算机的运行的 Windows 10，版本 1803，它们不能共享他们的桌面。

若要解决此问题，将 Windows 10 更新 4340917，应用[2018 年 7 月 24 日-KB4340917 (操作系统生成 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### 如果禁用 NLA 混合版本的 Windows 10 时的性能问题

NLA 处于禁用状态，并运行 Windows 10 的远程桌面客户端计算机连接到运行不同版本的 Windows 10 的远程桌面时，将出现此问题。 在连接到运行 Windows 10 版本 1803年或更高版本的远程桌面时，运行 Windows 10，版本 1709年或更早版本的计算机上的远程桌面客户端的用户体验性能不佳。

这是因为，在连接到 Windows 10 版本 1803年或更高版本时，较旧的客户端计算机禁用 NLA 后，使用较慢的协议。

若要解决此问题，应用 KB 4340917 [2018 年 7 月 24 日-KB4340917 (操作系统生成 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917)。

### 黑屏问题

Windows 8.0、 Windows 8.1、 Windows 10 RTM 和 Windows Server 2012 R2 中出现此问题。 用户启动远程桌面中的多个应用程序，然后从该会话将断开连接。 定期，用户重新连接到远程桌面进行交互的应用程序，并再次断开连接。 某些时候，当用户重新连接时，远程桌面会话就会显示黑屏。 用户必须结束从远程计算机的主机 （或 RDSH 服务器控制台） 的远程桌面会话。 此操作将停止应用程序。

若要解决此问题，应用为相应的以下更新：

  - Windows 8 和 Windows Server 2012: KB4103719， [2018 年 5 月 17 日-KB4103719 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2: KB4103724， [2018 年 5 月 17 日-KB4103724 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)和 KB 4284863， [2018 年 6 月 21 日-KB4284863 （每月汇总的预览）](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10： 中 KB4284860，修复[2018 年 6 月 12 日-KB4284860 （操作系统内部 10240.17889）](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
