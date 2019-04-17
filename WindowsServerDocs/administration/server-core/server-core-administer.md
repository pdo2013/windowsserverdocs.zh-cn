---
title: 管理服务器核心
description: 了解如何管理 Windows Server 的服务器核心安装
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "8977994"
---
# 管理服务器核心服务器

>适用于：Windows Server（半年频道）和 Windows Server 2016

服务器核心不具有 UI，因为你需要使用 Windows PowerShell cmdlet、 命令行工具或远程工具执行基本管理任务。 以下部分概述了显示 PowerShell cmdlet 和用于基本任务的命令。 你还可以使用[Windows Admin Center](../../manage/windows-admin-center/overview.md)，当前在公共预览版的统一的管理门户管理你的安装。 

## 使用 PowerShell cmdlet 的管理任务
使用以下信息来执行基本管理任务通过 Windows PowerShell cmdlet。

### 设置静态 IP 地址
当安装服务器核心服务器时，默认情况下它具有 DHCP 地址。 如果你需要静态 IP 地址，则可以将其使用以下步骤。

若要查看你当前的网络配置，请使用**Get NetIPConfiguration**。

若要查看你已使用的 IP 地址，请使用**Get-netipaddress**。

若要设置静态 IP 地址，执行以下操作： 

1. 运行**Get NetIPInterface**。 
2. 请注意 IP 接口或**InterfaceDescription**字符串的**IfIndex**列中的数字。 如果你有多个网络适配器，请注意数字或字符串对应于想要设置的静态 IP 地址的接口。
3. 运行以下 cmdlet 来设置静态 IP 地址：

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   其中：
   - **InterfaceIndex**是**IfIndex**步骤 2 中的值。 （在我们的示例，12）
   - **Ip 地址**是你想要设置的静态 IP 地址。 （在我们的示例，191.0.2.2）
   - **PrefixLength**是要设置的 IP 地址的前缀长度 （其他形式的子网掩码）。 （对于我们的示例，24）
   - **默认网关**是到默认网关 IP 地址。 （对于我们的示例，192.0.2.1）
4. 若要设置 DNS 客户端服务器地址，请运行以下 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   其中：
   - **InterfaceIndex**是 IfIndex 步骤 2 中的值。
   - **ServerAddresses**是你的 DNS 服务器的 IP 地址。
5. 若要添加多个 DNS 服务器，请运行以下 cmdlet: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   其中，在此示例中， **192.0.2.4**和**192.0.2.5**在这两个 IP 地址的 DNS 服务器。

如果你需要切换到使用 DHCP，运行**集 DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**。

### 加入域
使用以下 cmdlet 将计算机加入到域。

1. 运行**添加计算机**。 你将会提示输入这两个凭据来加入域和域名。
2. 如果你需要将域用户帐户添加到本地管理员组，在命令提示符 （而不是在 PowerShell 窗口） 中运行以下命令：

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. 重新启动计算机。 你可以通过运行**重新启动计算机**来执行此操作。

### 重命名该服务器
使用以下步骤重命名服务器。

1. 确定当前的**主机名**或**ipconfig**命令服务器的名称。
2. 运行**重命名计算机-ComputerName \<new_name\ >**。
3. 重新启动计算机。

### 激活服务器

运行**slmgr.vbs – ipk\<productkey\ >**。 然后运行**slmgr.vbs – ato**。 如果成功激活，将不会收到一条消息。

> [!NOTE]
> 你也可以激活服务器通过电话，使用[密钥管理服务 (KMS) 服务器](../../get-started/server-2016-activation.md)，或远程。 若要激活远程，从远程计算机上运行以下 cmdlet: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### 配置 Windows 防火墙

你可以使用 Windows PowerShell cmdlet 和脚本的服务器核心计算机上本地配置 Windows 防火墙。 有关可用于配置 Windows 防火墙的 cmdlet，请参阅[NetSecurity](/powershell/module/netsecurity/?view=win10-ps) 。

### 启用 Windows PowerShell 远程处理

你可以启用 Windows PowerShell 远程处理，在其中命令键入 Windows PowerShell 中运行另一台计算机上的一台计算机。 启用 Windows PowerShell 远程处理**Enable-psremoting**。

有关详细信息，请参阅[有关远程常见问题解答](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## 从命令行管理任务
使用以下参考信息从命令行执行管理任务。

### 配置和安装
|任务 | 命令 |
|-----|-------|
|设置的本地管理密码| **净用户管理员** \* |
|将计算机加入到域| **netdom 加入 %computername%****/domain:\<domain\ > /userd:\<domain\\username\ > /passwordd:**\* <br> 重新启动计算机。|
|确认已发生更改，域| **set** |
|从域中删除计算机|**netdom 删除 \ < computername\ >**| 
|将用户添加到本地管理员组|**净本地组管理员 / 添加 \ < domain\\username\ >** |
|从本地管理员组中删除用户|**净本地组管理员 /delete \ < domain\\username\ >** |
|将用户添加到本地计算机|**净用户 \ < domain\username\ > * / 添加** |
|将某个组添加到本地计算机|**净本地组 \ < 组名称 \ > / 添加**|
|更改已加入域的计算机的名称|**netdom renamecomputer %computername%/NewName:\<new 计算机名称 \ > /userd:\<domain\\username\ > /passwordd:** * |
|确认新的计算机名称|**set**| 
|更改的工作组中的计算机的名称|**netdom renamecomputer \ < currentcomputername\ > /NewName: \ < newcomputername\ >** <br>重新启动计算机。|
|禁用分页文件管理|**wmic 计算机系统名称 ="< computername\ >"设置 AutomaticManagedPagefile = False**| 
|配置页面文件|**wmic pagefileset 名称 ="< 路径/filename\ >"设置初始化 = \ < initialsize\ > 大化 = \ < maxsize\ >** <br>*路径/文件名*所在的路径和文件的名称分页，*初始化*分页文件，以字节为单位，起始大小，并且*maxsize*页面文件，以字节为单位的最大大小。|
|更改为静态 IP 地址|**ipconfig/所有** <br>记录的相关信息或将其重定向到文本文件 (**ipconfig/所有 > ipconfig.txt**)。<br>**netsh 界面 ipv4 显示接口**<br>验证接口列表。<br>**netsh 界面 ipv4 设置地址名称 \ < 接口 list\ ID > 源 = 静态地址 = \ < 首选的 IP address\ > 网关 = \ < 网关 address\ >**<br>运行**ipconfig/所有**到 vierfy DHCP 已启用设置为**否**。|
|设置静态 DNS 地址。|**netsh 界面 ipv4 添加 dnsserver 名称 = \<name 或网络接口 card\ ID > 地址 = \<IP 地址的主 DNS server\ > 索引 = 1 <br> **netsh 界面 ipv4 添加 dnsserver 名称 = 辅助 DNS server\ \<name >地址 = 辅助 DNS server\ \<IP 地址 > 索引 = 2 * * <br> 重复根据需要添加其他服务器。<br>运行**ipconfig/所有**验证地址正确无误。|
|从静态 IP 地址更改为 DHCP 提供的 IP 地址|**netsh 界面 ipv4 设置地址名称 = \ < IP 地址的本地 system\ > 源 = DHCP** <br>运行**Ipconfig/所有**验证 DCHP 启用是否设置为**是**。|
|输入产品密钥|**slmgr.vbs – ipk \ < 产品 k e y >**| 
|激活本地服务器|**slmgr.vbs ato**| 
|远程激活服务器|**cscript slmgr.vbs – ipk \ < 产品 k e y > \ < 服务器名称 \ > \ < username\ > \ < password\ >** <br>**cscript slmgr.vbs ato \ < servername\ > \ < username\ > \ < password\ >** <br>通过运行来获取计算机的 GUID **cscript slmgr.vbs-未** <br> 运行**cscript slmgr.vbs-dli \<GUID\ >** <br>验证许可状态已设置为**已授权 （激活）**。


### 网络和防火墙

|任务|命令| 
|----|-------|
|你将服务器配置为使用代理服务器|**netsh Winhttp 设置代理 \ < servername\ >: \ < 端口 \<sequence >** <br>**注意：** 服务器核心安装无法通过要求输入密码以允许连接的代理访问 Internet。|
|配置服务器绕过 Internet 地址的代理服务器|**netsh winttp 设置代理 \ < servername\ >: \ < 端口 \<sequence > 绕过列表 ="< 到 >"**| 
|显示或修改 IPSEC 配置|**netsh ipsec**| 
|显示或修改 NAP 配置|**netsh nap**| 
|显示或修改为物理地址转换的 IP|**arp**| 
|显示或配置本地路由表|**路由**| 
|查看或配置 DNS 服务器设置|**nslookup**| 
|显示协议统计信息和当前的 TCP/IP 网络连接|**netstat**| 
|显示协议统计信息和当前的 TCP/IP 连接使用通过 TCP/IP (NBT) 的 NetBIOS|**nbtstat**| 
|显示网络连接跃的点数|**pathping**| 
|网络连接的跟踪跃点|**tracert**| 
|显示多播路由器的配置|**mrinfo**| 
|启用远程管理的防火墙|**netsh advfirewall 防火墙设置规则组 ="Windows 防火墙远程管理"新 enable = yes**| 
 

### 更新、 错误报告和反馈

|任务|命令| 
|----|-------|
|安装更新|**wusa \ < update\ >.msu /quiet**| 
|列出已安装更新|**systeminfo**| 
|删除更新|**展开 /f:\* \ < update\ >.msu c:\test** <br>导航到 c:\test\ 并打开 \ < update\ >.xml 在文本编辑器中的。<br>替换**安装****删除**并保存该文件。<br>**pkgmgr /n:\ < update\ >.xml**|
|配置自动更新|若要验证当前设置: * * cscript %systemroot%\system32\scregedit.wsf /AU /v * *<br>若要启用自动更新： **cscript scregedit.wsf /AU 4** <br>若要禁用自动更新： **cscript %systemroot%\system32\scregedit.wsf /AU 1**| 
|启用错误报告|若要验证当前设置： **serverWerOptin /query** <br>若要自动发送详细的报告： **serverWerOptin / 详细** <br>若要自动发送摘要报告： **serverWerOptin /summary** <br>若要禁用错误报告： **serverWerOptin /disable**|
|参与客户体验改善计划 (CEIP)|若要验证当前设置： **serverCEIPOptin /query** <br>若要启用 CEIP: **serverCEIPOptin 后** <br>若要禁用 CEIP: **serverCEIPOptin /disable**|

### 服务、 流程和性能

|任务|命令| 
|----|-------|
|列出正在运行的服务|**sc 查询**或**网络开始菜单**| 
|启动服务|**sc 开始菜单 \<service name\ >** 或**net 开始菜单 \<service name\ >**| 
|停止服务|**sc stop \<service name\ >** 或**net stop \<service name\ >**| 
|检索运行应用程序和关联的进程的列表|**tasklist**||强制停止进程|运行**tasklist**检索进程 ID (PID)，然后运行**taskkill /PID \<process ID\ >**|
|开始菜单任务管理器|**taskmgr**| 
|创建和管理事件跟踪会话和性能日志|若要创建计数器、 跟踪、 配置数据集合或 API: **logman ceate** <br>查询数据收集器属性： **logman 查询** <br>若要开始或停止数据集合： **logman start\ | 停止** <br>若要删除收集器： **logman delete** <br> 若要更新收集器的属性： **logman 更新** <br>若要从 XML 文件导入的数据收集器集或将其导出到 XML 文件： **logman import\ | 导出**|

### 事件日志

|任务|命令| 
|----|-------|
|列表事件日志|**wevtutil el**| 
|查询中指定的日志的事件|**wevtutil qe /f:text \ < 登录名称 \ >**| 
|导出事件日志|**wevtutil epl \ < 登录名称 \ >**| 
|清除事件日志|**wevtutil cl \ < 登录名称 \ >**| 


### 磁盘和文件系统

|任务|命令|
|----|-------|
|管理磁盘分区|有关命令的完整列表，运行**diskpart /？**|  
|管理软件 RAID|有关命令的完整列表，运行**diskraid /？**|  
|管理卷装入点|有关命令的完整列表，运行**mountvol /？**| 
|整理卷|有关命令的完整列表，运行**碎片整理 /？**|  
|转换为 NTFS 文件系统卷|**将转换 \ < 卷 letter\ > /FS:NTFS**| 
|压缩的文件|有关命令的完整列表，运行**压缩 /？**|  
|管理打开的文件|有关命令的完整列表，运行**openfiles /？**|  
|管理 VSS 文件夹|有关命令的完整列表，运行**vssadmin /？**| 
|管理文件系统|有关命令的完整列表，运行**fsutil /？**||验证文件签名|**sigverif /？**| 
|取得文件或文件夹的所有权|有关命令的完整列表，运行**icacls /？**| 
 
### 硬件

|任务|命令| 
|----|-------|
|添加新的硬件设备的驱动程序|将驱动程序复制到 %homedrive%\\\ < 驱动程序 folder\ > 文件夹。 运行**pnputil-i-%homedrive%\\\<driver folder\ > \\\<driver\ >.inf**|
|删除硬件设备的驱动程序|有关已加载的驱动程序的列表，运行**sc 查询类型 = 驱动程序**。 然后运行**sc 删除 \<service_name\ >**|
