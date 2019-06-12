---
title: 管理 Server Core
description: 了解如何管理 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 50fa737db5862132c1dde5cb6eb6b83674b3f02e
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811384"
---
# <a name="administer-a-server-core-server"></a>管理服务器核心服务器

>适用于：Windows Server （半年频道） 和 Windows Server 2016

因为服务器核心不具有 UI，您需要使用 Windows PowerShell cmdlet、 命令行工具或远程工具来执行基本管理任务。 以下部分概述的 PowerShell cmdlet 和命令用于执行基本任务。 此外可以使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)，目前处于公共预览状态，来管理您的安装一个统一的管理门户。 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>使用 PowerShell cmdlet 的管理任务
使用以下信息来执行基本管理任务使用 Windows PowerShell cmdlet。

### <a name="set-a-static-ip-address"></a>设置静态 IP 地址
在安装服务器核心服务器时，默认情况下它具有 DHCP 地址。 如果需要静态 IP 地址，可以将其使用以下步骤。

若要查看当前的网络配置，请使用**Get NetIPConfiguration**。

若要查看你已在使用的 IP 地址，请使用**Get NetIPAddress**。

若要设置静态 IP 地址，请执行以下操作： 

1. 运行**Get-netipinterface**。 
2. 请注意中的编号**IfIndex**你的 IP 接口的列或**InterfaceDescription**字符串。 如果有多个网络适配器，请注意数字或你想要设置的静态 IP 地址的接口对应的字符串。
3. 运行以下 cmdlet 设置静态 IP 地址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**的值**IfIndex**步骤 2 中。 （在本例中为 12）
   - **IPAddress**是你想要设置的静态 IP 地址。 （在本例中为 191.0.2.2）
   - **PrefixLength**是你设置的 IP 地址前缀长度 （子网掩码的另一个窗体）。 （对于我们示例中是 24）
   - **默认网关**是默认网关的 IP 地址。 （对于我们示例中是 192.0.2.1）
4. 运行以下 cmdlet 以设置服务器地址的 DNS 客户端： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex** IfIndex 步骤 2 中的值。
   - **ServerAddresses**是你的 DNS 服务器的 IP 地址。
5. 若要添加多个 DNS 服务器，请运行以下 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   在此示例中，位置**192.0.2.4**和**192.0.2.5 都**是这两个 IP 地址的 DNS 服务器。

如果需要转换成使用 DHCP，运行**集 DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### <a name="join-a-domain"></a>加入一个域
使用以下 cmdlet 将计算机加入到域。

1. 运行**添加计算机**。 系统会提示的凭据加入域的域名。
2. 如果需要将域用户帐户添加到本地管理员组，（不在 PowerShell 窗口中） 的命令提示符处运行以下命令：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新启动计算机。 您可以执行此操作通过运行**重新启动计算机**。

### <a name="rename-the-server"></a>重命名服务器
使用以下步骤将服务器重命名。

1. 使用 **hostname** 或 **ipconfig** 命令确定服务器的当前名称。
2. 运行**重命名计算机-ComputerName \<new_name\>** 。
3. 重新启动计算机。

### <a name="activate-the-server"></a>激活服务器

运行**slmgr.vbs-ipk\<productkey\>** 。 然后运行**slmgr.vbs – ato**。 如果激活成功，不会获得一条消息。

> [!NOTE]
> 您还可以激活的服务器使用的电话[密钥管理服务 (KMS) 服务器](../../get-started/server-2016-activation.md)，或远程。 若要远程激活，请从远程计算机运行以下 cmdlet: 
> 
> ```powershell
> **cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
> ```
 
### <a name="configure-windows-firewall"></a>配置 Windows 防火墙

你可以使用 Windows PowerShell cmdlet 和脚本在服务器核心计算机上本地配置 Windows 防火墙。 请参阅[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) cmdlet 可用于配置 Windows 防火墙。

### <a name="enable-windows-powershell-remoting"></a>启用 Windows PowerShell 远程处理

你可以启用 Windows PowerShell 远程处理，在一台计算机上的 Windows PowerShell 中键入的命令在另一台计算机上运行。 启用 Windows PowerShell 远程处理**Enable-psremoting**。

有关详细信息，请参阅[关于远程 FAQ](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)。

## <a name="administrative-tasks-from-the-command-line"></a>从命令行管理任务
使用以下参考信息从命令行执行管理任务。

### <a name="configuration-and-installation"></a>配置和安装

|                             任务                              |                                                                                                                                                                                                                 Command                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             设置本地管理密码             |                                                                                                                                                                                                      **最终用户管理员** \*                                                                                                                                                                                                      |
|                  将计算机加入到域                  |                                                                                                                                                       **netdom join %computername%** **/domain:\<域\>/userd:\<域\\用户名\>/passwordd:** \* <br> 重新启动计算机。                                                                                                                                                        |
|              确认域已更改。              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                从域中删除计算机                |                                                                                                                                                                                                   **netdom 删除\<computername\>**                                                                                                                                                                                                    |
|         将用户添加到本地管理员组          |                                                                                                                                                                                       **net localgroup 管理员 /add\<域\\用户名\>**                                                                                                                                                                                       |
|       从本地管理员组删除用户       |                                                                                                                                                                                     **net localgroup 管理员 /delete\<域\\用户名\>**                                                                                                                                                                                      |
|               向本地计算机添加用户                |                                                                                                                                                                                                **最终用户\<域 \ 用户名\> \* / 添加**                                                                                                                                                                                                 |
|               向本地计算机添加组               |                                                                                                                                                                                                 **net localgroup\<组名称\>/ 添加**                                                                                                                                                                                                  |
|          更改加入域的计算机的名称          |                                                                                                                                                           **netdom renamecomputer %computername%了 /NewName:\<新的计算机名称\>/userd:\<域\\用户名\>/passwordd:** \*                                                                                                                                                            |
|                 确认新计算机名称                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         更改工作组中的计算机的名称         |                                                                                                                                                                **netdom renamecomputer \<currentcomputername\>了 /NewName:\<newcomputername\>** <br>重新启动计算机。                                                                                                                                                                 |
|                禁用页面文件管理                 |                                                                                                                                                                        **wmic computersystem where name="\<computername\>" set AutomaticManagedPagefile=False**                                                                                                                                                                         |
|                   配置页面文件                   |                                                            **wmic pagefileset where name=”\<path/filename\>” set InitialSize=\<initialsize\>,MaximumSize=\<maxsize\>** <br>其中*路径/文件名*是路径和分页文件的名称*initialsize*是分页文件，以字节为单位的起始大小和*maxsize*是最大大小页面文件，以字节为单位。                                                             |
|                 更改静态 IP 地址                 | **ipconfig /all** <br>记录的相关信息或将其重定向到文本文件 (**ipconfig /all > ipconfig.txt**)。<br>**netsh 接口 ipv4 显示接口**<br>验证存在接口列表。<br>**netsh interface ipv4 设置地址名称\<接口列表中的 ID\>源 = 静态地址 =\<首选的 IP 地址\>网关 =\<网关地址\>**<br>运行**ipconfig /all**若要验证是否已启用 DHCP 设置为**否**。 |
|                   设置静态 DNS 地址。                   |   <strong>netsh interface ipv4 添加 dnsserver 名称 =\<网络接口卡的名称或 ID\>地址 =\<的主 DNS 服务器的 IP 地址\>索引 = 1 <br></strong>netsh interface ipv4 添加 dnsserver 名称 =\<辅助 DNS 服务器的名称\>地址 =\<辅助 DNS 服务器 IP 地址\>索引 = 2\*\* <br> 根据需要添加更多服务器重复。<br>运行**ipconfig /all**若要验证的地址是否正确。   |
| 从静态 IP 地址更改为 DHCP 提供的 IP 地址 |                                                                                                                                      **netsh interface ipv4 设置地址名称 =\<的本地系统的 IP 地址\>源 = DHCP** <br>运行**ipconfig /all**若要验证是否已启用的 DCHP 设置为**是**。                                                                                                                                      |
|                      输入产品密钥                      |                                                                                                                                                                                                   **slmgr.vbs –ipk \<product key\>**                                                                                                                                                                                                    |
|                  本地激活服务器                  |                                                                                                                                                                                                           **slmgr.vbs -ato**                                                                                                                                                                                                            |
|                 远程激活服务器                  |                                            **cscript slmgr.vbs-ipk\<产品密钥\>\<服务器名称\>\<用户名\>\<密码\>** <br>**cscript slmgr.vbs -ato \<servername\> \<username\> \<password\>** <br>通过运行获取计算机的 GUID **cscript slmgr.vbs-未** <br> 运行**cscript slmgr.vbs-dli \<GUID\>** <br>验证许可证状态设置为 **（激活） 的已授权**。                                             |

### <a name="networking-and-firewall"></a>网络与防火墙

|任务|Command| 
|----|-------|
|将服务器配置为使用代理服务器|**netsh Winhttp 设置代理\<servername\>:\<端口号\>** <br>**注意：** 服务器核心安装不能通过需要密码以允许连接的代理访问 Internet。|
|将服务器配置为绕过代理访问 Internet 地址|**netsh winttp set proxy \<servername\>:\<port number\> bypass-list="\<local\>"**| 
|显示或修改 IPSEC 配置|**netsh ipsec**| 
|显示或修改 NAP 配置|**netsh nap**| 
|显示或修改物理地址转换的 IP|**arp**| 
|显示或配置本地路由表|**route**| 
|查看或配置的 DNS 服务器设置|**nslookup**| 
|显示协议统计和当前 TCP/IP 网络连接|**netstat**| 
|显示协议统计信息和当前使用 NetBIOS 通过 TCP/IP (NBT) 的 TCP/IP 连接|**nbtstat**| 
|显示网络连接的跃点|**pathping**| 
|跟踪网络连接的跃点|**tracert**| 
|显示多播路由器的配置|**mrinfo**| 
|启用防火墙的远程管理|**netsh advfirewall 防火墙设置的规则组 ="Windows 防火墙远程管理"新 enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>更新、 错误报告和反馈

|                               任务                                |                                                                                                                               Command                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         安装更新                         |                                                                                                                    **wusa\<更新\>.msu /quiet**                                                                                                                    |
|                      列出安装的更新                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         删除更新                          |                                 **展开 /f:\* \<更新\>.msu c:\test** <br>导航到 c:\test\ 并打开\<更新\>在文本编辑器中的.xml。<br>替换**安装**与**删除**并保存该文件。<br>**pkgmgr /n:\<update\>.xml**                                 |
|                    配置自动更新                    |          若要验证当前设置： **cscript %systemroot%\system32\scregedit.wsf /AU /v \* \*<br>以启用自动更新： \* \*cscript scregedit.wsf /AU 4** <br>若要禁用自动更新： **cscript %systemroot%\system32\scregedit.wsf /AU 1**          |
|                      启用错误报告                       | 若要验证当前设置： **serverWerOptin /query** <br>若要自动发送详细的报告： **serverWerOptin / 详细** <br>若要自动发送摘要报告： **serverWerOptin /summary** <br>若要禁用错误报告： **serverWerOptin /disable** |
| 参与客户体验改善计划 (CEIP) |                                                     若要验证当前设置： **serverCEIPOptin /query** <br>若要启用 CEIP: **serverCEIPOptin /enable** <br>若要禁用 CEIP: **serverCEIPOptin /disable**                                                      |

### <a name="services-processes-and-performance"></a>服务、进程和性能

|                               任务                               |                                                                                                                                                                                                             Command                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    列出正在运行的服务                     |                                                                                                                                                                                                  **sc 查询**或**net start**                                                                                                                                                                                                   |
|                         启动服务                          |                                                                                                                                                                                 **sc start\<服务名称\>** 或**net start\<服务名称\>**                                                                                                                                                                                  |
|                          停止服务                          |                                                                                                                                                                                  **sc stop\<服务名称\>** 或**net stop\<服务名称\>**                                                                                                                                                                                   |
| 检索正在运行的应用程序和相关进程的列表 |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        启动任务管理器                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    创建和管理事件跟踪会话和性能日志    | 若要创建计数器、 跟踪、 配置数据收集或 API: **logman 请** <br>查询数据收集器属性： **logman 查询** <br>若要启动或停止数据收集： **logman 开始\|停止** <br>若要删除收集器： **logman 删除** <br> 若要更新收集器的属性： **logman update** <br>若要从 XML 文件导入数据收集器集或将其导出到 XML 文件： **logman 导入\|导出** |

### <a name="event-logs"></a>事件日志

|任务|Command| 
|----|-------|
|列出事件日志|**wevtutil el**| 
|查询中指定的日志的事件|**wevtutil qe /f:text \<log name\>**| 
|导出事件日志|**wevtutil epl\<日志名称\>**| 
|清除事件日志|**wevtutil cl\<日志名称\>**| 


### <a name="disk-and-file-system"></a>磁盘和文件系统

|                   任务                   |                        Command                        |
|------------------------------------------|-------------------------------------------------------|
|          管理磁盘分区          | 有关命令的完整列表，运行**diskpart /？**  |
|           管理软件 RAID           | 有关命令的完整列表，运行**diskraid /？**  |
|        管理卷装入点        | 有关命令的完整列表，运行**mountvol /？**  |
|           卷碎片整理            |  有关命令的完整列表，运行**碎片整理 /？**   |
| 将卷转换成 NTFS 文件系统 |        **将转换\<卷号\>/fs: ntfs**         |
|              压缩文件              |  有关命令的完整列表，运行**压缩 /？**  |
|          管理打开的文件           | 有关命令的完整列表，运行**openfiles /？** |
|          管理 VSS 文件夹          | 有关命令的完整列表，运行**vssadmin /？**  |
|        管理文件系统        |  有关命令的完整列表，运行**fsutil /？**   |
|    取得文件或文件夹的所有权    |  有关命令的完整列表，运行**icacls /？**   |
 
### <a name="hardware"></a>硬件

|任务|Command| 
|----|-------|
|添加新硬件设备的驱动程序|将驱动程序复制到位于 %homedrive%\\\<驱动程序文件夹\>。 运行**pnputil-i-a %homedrive%\\\<驱动程序文件夹\>\\\<驱动程序\>.inf**|
|删除硬件设备的驱动程序|有关加载的驱动程序列表，运行**sc 查询类型 = 驱动程序**。 然后运行**sc delete \<service_name\>**|
