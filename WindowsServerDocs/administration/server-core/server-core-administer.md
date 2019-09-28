---
title: 管理服务器核心
description: 了解如何管理 Windows Server 的服务器核心安装
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: bcc4bf7b3fbdbff1aed2c8dd07b90346fe9eebab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383430"
---
# <a name="administer-a-server-core-server"></a>管理服务器核心服务器

>适用于：Windows Server 2019、Windows Server 2016 和 Windows Server （半年频道）

由于服务器核心没有 UI，因此需要使用 Windows PowerShell cmdlet、命令行工具或远程工具来执行基本的管理任务。 以下部分概述了用于基本任务的 PowerShell cmdlet 和命令。 你还可以使用[Windows 管理中心](../../manage/windows-admin-center/overview.md)（目前为公共预览版的统一管理门户）来管理你的安装。 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>使用 PowerShell cmdlet 管理任务
使用以下信息通过 Windows PowerShell cmdlet 执行基本的管理任务。

### <a name="set-a-static-ip-address"></a>设置静态 IP 地址
安装服务器核心服务器时，默认情况下它具有 DHCP 地址。 如果需要静态 IP 地址，可以使用以下步骤进行设置。

若要查看当前的网络配置，请使用**NetIPConfiguration**。

若要查看已在使用的 IP 地址，请使用**new-netipaddress**。

若要设置静态 IP 地址，请执行以下操作： 

1. 运行**get-netipinterface**。 
2. 请注意 IP 接口的**IfIndex**列中的数字或**InterfaceDescription**字符串。 如果有多个网络适配器，请记下要为其设置静态 IP 地址的接口对应的数字或字符串。
3. 运行以下 cmdlet 以设置静态 IP 地址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**是步骤2中**IfIndex**的值。 （在我们的示例中为12）
   - **IPAddress**是要设置的静态 IP 地址。 （在我们的示例中，191.0.2.2）
   - **PrefixLength**是所设置的 IP 地址的前缀长度（子网掩码的另一种形式）。 （对于我们的示例，为24）
   - **DefaultGateway**是默认网关的 IP 地址。 （在我们的示例中，192.0.2.1）
4. 运行以下 cmdlet 以设置 DNS 客户端服务器地址： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex**是步骤2中 IfIndex 的值。
   - **ServerAddresses**是 DNS 服务器的 IP 地址。
5. 若要添加多个 DNS 服务器，请运行以下 cmdlet： 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   其中，在此示例中， **192.0.2.4 和**和**192.0.2.5 都**都是 DNS 服务器的 IP 地址。

如果需要切换到使用 DHCP，请运行**DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### <a name="join-a-domain"></a>加入一个域
使用以下 cmdlet 将计算机加入域。

1. 运行 "**外接程序**"。 系统将提示你输入加入域和域名的凭据。
2. 如果需要将域用户帐户添加到本地管理员组，请在命令提示符处运行以下命令（不在 PowerShell 窗口中）：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新启动计算机。 为此，可以运行**重新启动计算机**。

### <a name="rename-the-server"></a>重命名服务器
使用以下步骤重命名服务器。

1. 使用 **hostname** 或 **ipconfig** 命令确定服务器的当前名称。
2. 运行**1new_name @ no__t-2 @no__t**。
3. 重新启动计算机。

### <a name="activate-the-server"></a>激活服务器

运行**slmgr.vbs – ipk @ no__t-1productkey @ no__t**。 然后运行**slmgr.vbs – ato**。 如果激活成功，则不会收到消息。

> [!NOTE]
> 还可以通过电话、使用[密钥管理服务（KMS）服务器](../../get-started/server-2016-activation.md)或者远程激活服务器。 若要远程激活，请从远程计算机运行以下 cmdlet： 
> 
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```
 
### <a name="configure-windows-firewall"></a>配置 Windows 防火墙

你可以使用 Windows PowerShell cmdlet 和脚本在服务器核心计算机上本地配置 Windows 防火墙。 有关可用于配置 Windows 防火墙的 cmdlet，请参阅[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) 。

### <a name="enable-windows-powershell-remoting"></a>启用 Windows PowerShell 远程处理

你可以启用 Windows PowerShell 远程处理，在一台计算机上的 Windows PowerShell 中键入的命令在另一台计算机上运行。 使用**enable-psremoting**启用 Windows PowerShell 远程处理。

有关详细信息，请参阅[关于远程常见问题解答](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)。

## <a name="administrative-tasks-from-the-command-line"></a>命令行中的管理任务
使用以下参考信息从命令行执行管理任务。

### <a name="configuration-and-installation"></a>配置和安装

|                             任务                              |                                                                                                                                                                                                                 Command                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             设置本地管理密码             |                                                                                                                                                                                                      **net 用户管理员**\*                                                                                                                                                                                                      |
|                  将计算机加入到域                  |                                                                                                                                                       **netdom join% computername%** **/domain： \<domain @ no__t-3/userd： \<domain @ no__t-5username @ no__t-6/passwordd：** \* <br> 重新启动计算机。                                                                                                                                                        |
|              确认域已更改。              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                从域中删除计算机                |                                                                                                                                                                                                   **netdom remove \<computername @ no__t-2**                                                                                                                                                                                                    |
|         将用户添加到本地管理员组          |                                                                                                                                                                                       **net localgroup Administrators/add \<domain @ no__t-2username @ no__t-3**                                                                                                                                                                                       |
|       从本地管理员组删除用户       |                                                                                                                                                                                     **net localgroup Administrators/delete \<domain @ no__t-2username @ no__t-3**                                                                                                                                                                                      |
|               向本地计算机添加用户                |                                                                                                                                                                                                **net user \<domain \ 用户名 @ no__t-2 \*/add**                                                                                                                                                                                                 |
|               向本地计算机添加组               |                                                                                                                                                                                                 **net localgroup \<group name @ no__t-2/add**                                                                                                                                                                                                  |
|          更改加入域的计算机的名称          |                                                                                                                                                           **netdom renamecomputer% computername% 了/newname： \<new computer name @ no__t-2/userd： \<domain @ no__t-4username @ no__t-5/passwordd：** \*                                                                                                                                                            |
|                 确认新计算机名称                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         更改工作组中的计算机的名称         |                                                                                                                                                                **netdom renamecomputer \<currentcomputername @ no__t-2 了/newname： \<newcomputername @ no__t-4** <br>重新启动计算机。                                                                                                                                                                 |
|                禁用页面文件管理                 |                                                                                                                                                                        **wmic 程序，其中 name = "\<computername @ no__t-2" set AutomaticManagedPagefile = False**                                                                                                                                                                         |
|                   配置页面文件                   |                                                            **wmic pagefileset，其中 name = "\<path/filename @ no__t-2" set InitialSize = \<initialsize @ no__t-4，MaximumSize = \<maxsize @ no__t-6** <br>其中*path/filename*是页面文件的路径和名称， *initialsize*是页面文件的起始大小（以字节为单位）， *maxsize*是页面文件的最大大小（以字节为单位）。                                                             |
|                 更改静态 IP 地址                 | **ipconfig/all** <br>记录相关信息或将其重定向到文本文件（**ipconfig/all > ipconfig .txt**）。<br>**netsh interface ipv4 show interface**<br>验证是否有接口列表。<br>**netsh interface ipv4 set address name \<ID from interface list @ no__t-2 source = static address = \<preferred IP address @ no__t-4 gateway = \<gateway address @ no__t-6**<br>运行**ipconfig/all** ，验证 DHCP 是否已启用设置为 "**否**"。 |
|                   设置静态 DNS 地址。                   |   @no__t 0netsh 接口 ipv4 add dnsserver name = \<name 或网络接口卡的 ID @ no__t-2 address = 主 DNS 服务器 @ 3IP 索引的 @no__t-NO__T 地址 = 1 <br>@no__t 0netsh 接口 ipv4 add dnsserver name = \<name of 辅助 DNS 服务器 @ no__t-2 address = 辅助 DNS 服务器的 \<IP 地址 = 2 @ no__t-4 @ no__t-6 <br> 根据需要重复此步骤以添加其他服务器。<br>运行**ipconfig/all**以验证地址是否正确。   |
| 从静态 IP 地址更改为 DHCP 提供的 IP 地址 |                                                                                                                                      **netsh interface ipv4 set address name = \<IP address of local system @ no__t-2 source = DHCP** <br>运行**ipconfig/all** ，验证是否已将 DCHP 设置为 **"是"** 。                                                                                                                                      |
|                      输入产品密钥                      |                                                                                                                                                                                                   **slmgr.vbs – ipk \<product key @ no__t-2**                                                                                                                                                                                                    |
|                  本地激活服务器                  |                                                                                                                                                                                                           **slmgr.vbs-ato**                                                                                                                                                                                                            |
|                 远程激活服务器                  |                                            **cscript slmgr.vbs-ipk \<product key @ no__t-2 @ no__t-3server name @ no__t-4 @ no__t-5username @ no__t-6 @ no__t-7password @ no__t-8** <br>**cscript slmgr.vbs-ato \<servername @ no__t-2 \<username @ no__t-4 \<password @ no__t-6** <br>通过运行**cscript slmgr.vbs**获取计算机的 GUID-已 <br> 运行**cscript slmgr.vbs-dli \<GUID @ no__t-2** <br>验证 "许可证状态" 是否设置为 "已**许可（已激活）** "。                                             |

### <a name="networking-and-firewall"></a>网络与防火墙

|任务|Command| 
|----|-------|
|将服务器配置为使用代理服务器|**netsh Winhttp set proxy \<servername @ no__t-2： \<port 号 @ no__t-4** <br>**注意：** 服务器核心安装无法通过需要密码以允许连接的代理来访问 Internet。|
|将服务器配置为绕过 Internet 地址的代理|**netsh winhttp set proxy \<servername @ no__t-2： \<port number @ no__t-4 旁路-list = "\<local @ no__t-6"**| 
|显示或修改 IPSEC 配置|**netsh ipsec**| 
|显示或修改 NAP 配置|**netsh nap**| 
|显示或修改 IP 到物理地址的转换|**arp**| 
|显示或配置本地路由表|**路由**| 
|查看或配置 DNS 服务器设置|**nslookup**| 
|显示协议统计和当前 TCP/IP 网络连接|**netstat**| 
|使用 TCP/IP 上的 NetBIOS （NBT）显示协议统计信息和当前 TCP/IP 连接|**nbtstat**| 
|显示网络连接的跃点|**pathping**| 
|跟踪网络连接的跃点|**tracert**| 
|显示多播路由器的配置|**mrinfo**| 
|启用防火墙的远程管理|**netsh advfirewall 防火墙设置规则组 = "Windows Defender 防火墙远程管理" 新建 enable = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>更新、错误报告和反馈

|                               任务                                |                                                                                                                               Command                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         安装更新                         |                                                                                                                    **wusa \<update\>.msu/quiet**                                                                                                                    |
|                      列出安装的更新                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         删除更新                          |                                 **展开/f： @no__t \<update\>.msu c:\test** <br>导航到 c:\test\，在文本编辑器中打开 \<update\>.xml "。<br>将**Install**替换为**Remove** ，并保存该文件。<br>**pkgmgr.exe/n： \<update\>.xml**                                 |
|                    配置自动更新                    |          验证当前设置： **cscript%SYSTEMROOT%\SYSTEM32\SCREGEDIT.WSF/AU/v \* @ no__t-2 @ no__t-3To 启用自动更新： \* @ no__t-5cscript scregedit.wsf. .WSF/AU 4** <br>禁用自动更新： **cscript%SYSTEMROOT%\SYSTEM32\SCREGEDIT.WSF/AU 1**          |
|                      启用错误报告                       | 验证当前设置： **serverWerOptin/query** <br>自动发送详细报告： **serverWerOptin/detailed** <br>自动发送摘要报告： **serverWerOptin/summary** <br>禁用错误报告： **serverWerOptin/disable** |
| 参与客户体验改善计划 (CEIP) |                                                     验证当前设置： **serverCEIPOptin/query** <br>若要启用 CEIP： **serverCEIPOptin/enable** <br>禁用 CEIP： **serverCEIPOptin/disable**                                                      |

### <a name="services-processes-and-performance"></a>服务、进程和性能

|                               任务                               |                                                                                                                                                                                                             Command                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    列出正在运行的服务                     |                                                                                                                                                                                                  **sc 查询**或**net start**                                                                                                                                                                                                   |
|                         启动服务                          |                                                                                                                                                                                 **sc start \<service name @ no__t-2**或**net start \<service name @ no__t-5**                                                                                                                                                                                  |
|                          停止服务                          |                                                                                                                                                                                  **sc stop \<service name @ no__t-2**或**net stop \<service name @ no__t-5**                                                                                                                                                                                   |
| 检索正在运行的应用程序和相关进程的列表 |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        启动任务管理器                        |                                                                                                                                                                                                           **任务管理器**                                                                                                                                                                                                            |
|    创建和管理事件跟踪会话和性能日志    | 创建计数器、跟踪、配置数据收集或 API： **logman 个** <br>查询数据收集器属性： **logman 查询** <br>开始或停止数据收集： **logman start @ no__t-1stop** <br>删除收集器： **logman 删除** <br> 更新收集器的属性： **logman 更新** <br>若要从 XML 文件导入数据收集器集或将其导出到 XML 文件： **logman import @ no__t-1export** |

### <a name="event-logs"></a>事件日志

|任务|Command| 
|----|-------|
|列出事件日志|**wevtutil el**| 
|查询指定日志中的事件|**wevtutil qe/f： text \<log name @ no__t-2**| 
|导出事件日志|**wevtutil epl \<log name @ no__t-2**| 
|清除事件日志|**wevtutil cl \<log name @ no__t-2**| 


### <a name="disk-and-file-system"></a>磁盘和文件系统

|                   任务                   |                        Command                        |
|------------------------------------------|-------------------------------------------------------|
|          管理磁盘分区          | 有关完整的命令列表，请运行**diskpart/？**  |
|           管理软件 RAID           | 有关完整的命令列表，请运行**diskraid/？**  |
|        管理卷装入点        | 有关完整的命令列表，请运行**mountvol/？**  |
|           卷碎片整理            |  有关完整的命令列表，请运行**defrag/？**   |
| 将卷转换成 NTFS 文件系统 |        **convert @no__t 1volume 字母 @ no__t-2/FS： NTFS**         |
|              压缩文件              |  有关完整的命令列表，请运行**compact/？**  |
|          管理打开的文件           | 有关完整的命令列表，请运行**openfiles/？** |
|          管理 VSS 文件夹          | 有关完整的命令列表，请运行**vssadmin/？**  |
|        管理文件系统        |  有关完整的命令列表，请运行**fsutil/？**   |
|    取得文件或文件夹的所有权    |  有关完整的命令列表，请运行**icacls/？**   |
 
### <a name="hardware"></a>硬件

|任务|Command| 
|----|-------|
|添加新硬件设备的驱动程序|将驱动程序复制到% homedrive% \\ @ no__t-1driver 文件夹 @ no__t-2 文件夹中的文件夹。 运行**pnputil-i-a% homedrive% \\ @ no__t-2driver 文件夹 @ no__t-3 @ no__t-4 @ no__t-5driver\>.inf**|
|删除硬件设备的驱动程序|有关已加载的驱动程序的列表，请运行**sc query type = driver**。 然后运行**sc delete \<service_name @ no__t-2**|
