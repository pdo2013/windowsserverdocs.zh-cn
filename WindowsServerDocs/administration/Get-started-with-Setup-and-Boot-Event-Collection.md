---
title: 安装和启动事件收集入门
description: 设置安装和启动事件收集的收集器和目标
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: e94659c62db574dc8779c8246d471ab401414ddb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435805"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>安装和启动事件收集入门

>适用于：Windows Server

  
## <a name="overview"></a>概述  
安装和启动事件收集是 Windows Server 2016 中的一项新功能，通过此功能，你可以指定一台“收集器”计算机，以在启动或完成安装过程时收集其他计算机上发生的各种重要事件。 然后，你可以使用事件查看器、消息分析器、Wevtutil 或 Windows PowerShell cmdlet 分析收集的事件。  
  
以前一直无法监控这些事件，因为在事先设置计算机之前，收集事件所需的基础架构不存在。 你可以监视的安装和启动事件的类型包括：  
  
-   加载内核模块和驱动程序  
  
-   枚举设备和初始化其驱动程序（包括“设备”，如 CPU 类型）  
  
-   验证和安装文件系统  
  
-   启动可执行文件  
  
-   启动和完成系统更新  
  
-   完成服务启动以及提供网络共享（当系统可以登录并与域控制器建立连接时）  
  
收集器计算机必须运行 Windows Server 2016（可以是具有桌面体验或 Server Core 模式的服务器）。 目标计算机必须运行 Windows 10 或 Windows Server 2016。 你还可以在**未**运行 Windows Server 2016 的计算机上所托管的虚拟机上运行此服务。 已知虚拟化收集器和目标计算机可采用以下组合：  
  
|虚拟化主机|收集器虚拟机|目标虚拟机|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|是|是|  
|Windows 10|是|是|  
|Windows Server 2016|是|是|  
|Windows Server 2012 R2|是|否|  
  
## <a name="installing-the-collector-service"></a>安装收集器服务  
从 Windows Server 2016 开始，事件收集器服务可用作一项可选功能。 在此版本中，你可以在提升的 Windows PowerShell 提示符下配合使用 DISM.exe 和以下命令来安装此服务：  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
此命令会创建一项叫做 BootEventCollector 的服务并用空配置文件启动它。  
  
通过检查 `get-service -displayname *boot*` 来确认安装是否成功。 启动事件收集器应该正在运行。 它通过网络服务帐户运行，并且会在 **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config** 中创建一个空配置文件 (Active.xml)。  
  
你还可以使用服务器管理器中的“添加角色和功能”向导来安装此安装和启动事件收集服务。  
  
## <a name="configuration"></a>配置  
你需要配置两个项目来收集安装和启动事件。  
  
-   在将发送事件的目标计算机（即，要监视其安装和启动的计算机）上，启用 KDNET/EVENT-NET 传输和事件转发。  
  
-   在收集器计算机上，指定要从哪些计算机接受事件以及要将事件保存在何处。  
  
> [!NOTE]  
> 你无法将计算机配置为将启动或引导事件发送给自身。 但是，如果你想要监视两台计算机，则可以将其配置为相互发送事件。  
  
### <a name="configuring-a-target-computer"></a>配置目标计算机  
在每个目标计算机上，首先启用 KDNET/EVENT-NET 传输，然后允许通过传输来发送 ETW 事件，并重启目标计算机。 EVENT-NET 是一项类似于 KDNET（内核调试程序协议）的内核内部传输协议。 EVENT-NET 只传输事件但不允许进行调试程序访问。 这两项协议相互排斥；一次只能启用两者之一。  
  
你可以远程（使用 Windows PowerShell）或在本地启用事件传输。  
  
##### <a name="to-enable-event-transport-remotely"></a>若要远程启用事件传输，请执行以下操作  
  
1.  如果已将 Windows PowerShell 远程处理设置为目标计算机，请跳至步骤 3。 如果未设置，则在目标计算机上打开命令提示符并运行以下命令：  
  
    winrm quickconfig  
  
2.  提示响应，然后重启目标计算机。 如果目标计算机与收集器计算机不在同一个域中，则可能需要将它们定义为受信任主机。 要实现此目的，请执行以下操作：  
  
3.  在收集器计算机上，运行以下命令之一：  
  
    -   在 Windows PowerShell 提示符下：`Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`后, 跟`Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true`其中\<target1 > 等的名称或 IP 地址的目标计算机。  
  
    -   或在命令提示符中： **winrm 设置 winrm/config/客户端 @{TrustedHosts ="\<target1 >，\<target2 >，...";AllowUnencrypted ="true"}**  
  
        > [!IMPORTANT]  
        > 这会设置未加密的通信，因此请勿在实验室环境之外执行此操作。  
  
4.  通过转到收集器计算机并运行以下 Windows PowerShell 命令之一来测试远程连接：  
  
    如果目标计算机是作为收集器计算机位于同一域中，运行 `New-PSSession -Computer <target> | Remove-PSSession`  
  
    如果目标计算机不在同一个域中，请运行 `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`，系统将提示你提供凭据。  
  
    如果该命令未返回任何内容，则远程处理成功完成。  
  
5.  在目标计算机上，打开一个提升的 Windows PowerShell 提示符并运行以下命令：  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    < Target_name > 下面是目标计算机的名称\<ip > 是收集器计算机的 IP 地址。 \<端口 > 是收集器将在其中运行的端口号。 密钥 <a.b.c.d> 是通信所需的加密密钥，它包括由点分隔的四个字母数字字符串。 收集器计算机上使用此相同的密钥。 如果你未输入密钥，则系统会生成一个随机密钥；你需要将此密钥用于收集器计算机，因此请记下此密钥。  
  
6.  如果已设置收集器计算机，请使用新目标计算机的信息更新收集器计算机上的配置文件。 请参阅“配置收集器计算机”部分以获取详细信息。  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>若要在目标计算机上本地启用事件传输，请执行以下操作  
  
1.  启动提升的命令提示符，然后运行以下命令：  
  
    **bcdedit /event 是**  
  
    **bcdedit /eventsettings net hostip:1.2.3.4 端口： 50000 key:a.b.c.d**  
  
    在此，“1.2.3.4”是一个示例；请将其替换为收集器计算机的 IP 地址。 此外，将“50000”替换为收集器运行所在的端口号，并将“a.b.c.d”替换为通信所需的加密密钥。 收集器计算机上使用此相同的密钥。 如果你未输入密钥，则系统会生成一个随机密钥；你需要将此密钥用于收集器计算机，因此请记下此密钥。  
  
2.  如果已设置收集器计算机，请使用新目标计算机的信息更新收集器计算机上的配置文件。 请参阅“配置收集器计算机”部分以获取详细信息。  
  
**现在，启用事件传输本身，必须使系统能够实际发送 ETW 事件，通过该传输。**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>要允许通过传输远程发送 ETW 事件，请执行以下操作  
  
1.  在收集器计算机上，打开提升的 Windows PowerShell 提示符。  
  
2.  运行 `Enable-SbecAutologger -ComputerName <target_name>`，其中 <target_name> 是目标计算机的名称。  
  
如果无法设置 Windows PowerShell 远程处理，则可以始终允许直接在目标计算机上发送事件。  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>若要允许在本地通过传输发送 ETW 事件，请执行以下操作  
  
1.  在目标计算机上，启动 Regedit.exe 并查找以下注册表项：  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**。 各个日志会话在此项下面列出为子项。 可以选择将**设置平台**、**NT 内核记录程序**和 **Microsoft-Windows-Setup** 以与“安装和启动事件收集”配合使用，但推荐使用 **EventLog-System**。 [配置和启动自动记录器会话](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx)中详细介绍了这些项。  
  
2.  在 EventLog-System 项中，将 **LogFileMode** 的值从 **0x10000180** 更改为 **0x10080180**。 有关这些设置的详细信息，请参阅[日志记录模式常量](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx)。  
  
3.  （可选）你还可以允许将 Bug 检查数据转发到收集器计算机。 若要执行此操作，请查找注册表项 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager，并使用 **0x1** 的值创建**调试打印筛选器**项。  
  
4.  重启目标计算机。  
  
### <a name="choosing-the-network-adapter"></a>选择网络适配器  
如果目标计算机具有多个网络适配器，KDNET 驱动程序将选择所列出的第一个受支持项。 你可以通过以下步骤指定用于转发安装事件的特定网络适配器：  
  
##### <a name="to-specify-a-network-adapter"></a>要指定网络适配器，请执行以下操作  
  
1.  在目标计算机上，打开设备管理器、展开**网络适配器**、查找想要使用的网络适配器并右键单击该网络适配器。  
  
2.  在打开的菜单，单击**属性**，然后单击**详细信息**选项卡。展开的菜单中**属性**字段中，滚动查找**位置信息**（列表可能不是按字母顺序），然后单击它。 值将为格式的字符串**PCI 总线 X，Y 设备、 函数 Z**。请记下的 X.Y.Z;这些是所需的以下命令总线参数。  
  
3.  运行以下命令之一：  
  
    从提升的 Windows PowerShell 提示符： `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    从提升的命令提示符下：**bcdedit /eventsettings  net hostip:aaa port:50000 key:bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>验证目标计算机配置  
要检查目标计算机上的设置，请打开提升的命令提示符，然后运行 **bcdedit /enum**。 完成后，请运行 **bcdedit /eventsettings**。 你可以仔细检查以下值：  
  
-   键  
  
-   Debugtype = NET  
  
-   主机 =\<收集器的 IP 地址 >  
  
-   端口 =\<收集器使用指定的端口的号 >  
  
-   DHCP = 是  
  
此外，检查是否启用了 **bcdedit /event**，因为 **/debug** 和 **/event** 互相排斥。 一次只能运行其中一项。 同样，不能将 /eventsettings 与 /debug 混用，也不能将 /dbgsettings 与 /event 混用。  
  
另外，请注意，如果设置为串行端口，则事件收集不起作用。  
  
### <a name="configuring-the-collector-computer"></a>配置收集器计算机  
收集器服务接收事件，并将其保存在 ETL 文件中。 然后，诸如事件查看器、消息分析器、Wevtutil 和 Windows PowerShell cmdlet 之类的其他工具可以读取这些 ETL 文件。  
  
由于 ETW 格式不允许你指定目标计算机名称，因此每台目标计算机的事件必须保存到单独的文件中。 显示工具可能会显示计算机名称，这将是运行工具的计算机的名称。  
  
更准确地说，每台目标计算机都分配有一连串的 ETL 文件。 每个文件名都包含一个介于 000 到所配置的最大值之间的索引（最高为 999）。 当文件达到最大配置大小时，它会将编写事件切换到下一个文件。 文件达到可能的最高索引之后，会切换回文件索引 000。 系统以这种方式自动回收文件，并限制使用磁盘空间。 你还可以设置其他外部保留策略以进一步限制磁盘的使用；例如，你可以删除早于设定天数的文件。  
  
通常，收集的 ETL 文件保留在 **c:\ProgramData\Microsoft\BootEventCollector\Etl** 目录（可能具有其他子目录）中。 你可以按上次修改时间进行排序来查找最新的日志文件。 另外，还有一个状态日志（通常在 **c:\ProgramData\Microsoft\BootEventCollector\Logs** 中)，其中记录了收集器将编写事件切换到新文件的时间。  
  
此外，还有一个收集器日志，其中记录了有关收集器本身的信息。 你可以按 ETW 格式（向 Windows 日志服务报告事件所采用的格式；这是默认格式）保留此日志；或将此日志保留在文件中（通常在 **c:\ProgramData\Microsoft\BootEventCollector\Logs** 中）。 当希望启用会产生大量数据的详细模式时，可以使用此类文件。 你还可以将日志设置为从命令行运行收集器以写入标准输出。  
  
**创建收集器配置文件**  
  
三个 XML 配置文件启用该服务时，会创建并存储在**c:\ProgramData\Microsoft\BootEventCollector\Config**:  
  
-   **Active.xml** 此文件包含收集器服务的当前活动配置。  安装后，该文件将与 Empty.xml 具有相同的内容。 设置新的收集器配置时，配置会保存到此文件。  
  
-   **Empty.xml** 该文件包含最少的已设置默认值的必要配置元素。 它未启用任何收集；它只允许收集器服务在空闲模式下启动。  
  
-   **Example.xml** 此文件提供可能的配置元素的示例和说明。  
  
**选择文件大小限制**  
  
你必须设置文件大小限值。 最佳文件大小限值取决于预期的事件和可用磁盘空间的数量。 文件越小，清理旧数据越方便。 但是，每个文件都带有 64KB 标头的开销，读取多个文件，以合并历史记录可能是不方便。 绝对最小文件大小限制为 256 KB。 合理的实用文件大小限值应为 1 MB 以上，10 MB 算是理想的典型值。 如果你希望保存更多事件，也可以设置更高的限值。  
  
关于配置文件，请记住以下一些详细信息：  
  
-   目标计算机地址。 你可以使用其 IPv4 地址、 MAC 地址或 SMBIOS GUID。 选择要使用的地址时，请牢记以下因素：  
  
    -   IPv4 地址最适用于 IP 地址静态分配。 但是，即使是静态 IP 地址也必须通过 DHCP 来提供。  
  
    -   如果预先知道 MAC 地址或 SMBIOS GUID 会很方便，但 IP 地址是动态分配的。  
  
    -   EVENT-NET 协议不支持 IPv6 地址。  
  
    -   可以指定多种标识计算机的方式。 例如，如果即将更换物理硬件，则可以输入旧的和新的 MAC 地址，系统将会接受其中一个。  
  
-   用于与收集器计算机通信的加密密钥  
  
-   目标计算机的名称。 你可以使用 IP 地址、主机名或任何其他名称作为计算机名称。  
  
-   要使用的 ETL 文件的名称及其圈大小配置  
  
##### <a name="to-create-the-configuration-file"></a>要创建配置文件，请执行以下操作  
  
1.  打开一个提升的 Windows PowerShell 提示符，并将目录更改为 %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config。  
  
2.  键入 `notepad .\newconfig.xml`，然后按 ENTER。  
  
3.  将此示例配置复制到记事本窗口中：  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > 根节点是\<收集器 >。 其属性指定配置文件语法的版本和状态日志文件的名称。  
    >   
    > \<常见 > 元素组合在一起指定通用的配置元素，非常像用户组可用于指定多个用户的常见权限的多个目标。  
    >   
    > \<Collectorport > 元素定义在收集器将侦听传入的数据的 UDP 端口号。 这是 Bcdedit 目标配置步骤中指定的相同端口。 收集器仅支持一个端口，并且所有目标都必须连接到同一个端口。  
    >   
    > \<转发器 > 元素指定如何将转发到目标计算机上从接收的 ETW 事件。 只有一类转发器会将事件写入 ETL 文件。 参数指定文件名模式、圈中每个文件的大小限值以及每台计算机的圈大小。 设置“toxml”指定接收 ETW 事件时将按二进制形式写入，不会转换为 XML。 有关决定是否将事件赋予 XML 的信息，请参阅“XML 事件转换”部分。 文件名模式包含以下替换：用 {computer} 替换计算机名，用 {#3} 替换圈中的文件索引。  
    >   
    > 在此示例文件中，两个目标计算机定义与\<目标 > 元素。 每个定义指定的 IP 地址\<ipv4 >，但也可以使用 MAC 地址 (例如，< mac 值 ="11:22:33:44:55:66"\/> 或 < mac value="11-22-33-44-55-66"\/>) 或 （对于 SMBIOS GUID示例中，< guid 值 ="{269076F9-4B77-46E1-B03B-CA5003775B88}"\/>) 来标识目标计算机。 另外，请记录加密密钥（与在目标计算机上使用 Bcdedit 指定或生成的密钥相同）以及计算机名称。  
  
4.  输入为一个单独的每台目标计算机的详细信息\<目标 > 元素中，在配置文件，然后保存 Newconfig.xml，然后关闭记事本。  
  
5.  使用 `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` 应用新配置。 系统应该会返回输出且“成功”字段为“true”。 如果系统返回了其他结果，请参阅本主题的“疑难解答”部分。  
  
始终可以用 `(Get-SbecActiveConfig).text` 检查当前活动配置。  
  
你可以使用 `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result` 对配置文件执行有效性检查。  
  
虽然应用新配置的 Windows PowerShell 命令会自动更新服务而不要求重启服务，但是你始终可以使用以下任一命令自行重启服务：  
  
-   使用 Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   在普通命令提示符下，使用：**sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>将 Nano Server 配置为目标计算机  
Nano Server 提供的最小接口有时可能会导致相关问题诊断困难。 你可以将 Nano Server 映像配置为自动参与安装和启动事件收集，并将诊断数据发送到收集器计算机，而无需你进一步干预。 要实现这一点，请执行下列操作：  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>将 Nano Server 配置为目标计算机  
  
1.  创建基本 Nano Server 映像。 有关详细信息，请参阅 [Nano Server 入门](https://technet.microsoft.com/library/mt126167.aspx)。  
  
2.  设置收集器计算机，如本主题的“配置收集器计算机”部分中所示。  
  
3.  添加自动记录器注册表项以允许发送诊断消息。 为此，应安装步骤 1 中所创建的 Nano Server VHD、加载注册表配置单元，然后添加某些注册表项。 在此示例中，Nano Server 映像位于 C:\NanoServer 中；你的路径可能会有所不同，因此请相应地调整这些步骤。  
  
    1.  在收集器计算机中，复制 **..\Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** 文件夹，并将其粘贴到要用于修改 Nano Server VHD 的计算机上的 **..\Windows\System32\WindowsPowerShell\v1.0\Modules** 目录中。  
  
    2.  以提升的权限启动 Windows PowerShell 控制台并运行 `Import-Module BootEventCollector`。  
  
    3.  更新 Nano Server VHD 注册表以启用自动记录器。 要执行此操作，请运行 `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`。 这会添加最典型的安装和启动事件的基本列表；你可以在[控制事件跟踪会话](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx)中研究其他项。  
  
4.  更新 Nano Server 映像中的 BCD 设置以启用事件标志，并设置收集器计算机以确保将诊断事件发送到正确的服务器。 请记录收集器计算机的 IPv4 地址、TCP 端口和你在收集器的 Active.XML 文件中配置的加密密钥（见本主题其他部分所述）。 在具有提升的权限的 Windows PowerShell 控制台中使用以下命令： `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  更新收集器计算机以接收 Nano Server 计算机发送的事件，方法是将 IPv4 地址范围、特定 IPv4 地址或 Nano Server 的 MAC 地址添加到收集器计算机上的 Active.XML 文件中（请参阅本主题的“配置收集器计算机”部分）。  
  
## <a name="starting-the-event-collector-service"></a>启动事件收集器服务  
在收集器计算机上保存有效的配置文件并配置目标计算机后，只要重启目标计算机，就会建立与收集器的连接并将收集事件。  
  
可以在 Microsoft-Windows-BootEvent-Collector/Admin 下找到收集器服务本身的日志（与服务收集的安装和启动数据不同）。 对于事件的图形接口，请使用事件查看器。 创建新视图；依次展开**应用程序和服务日志**和 **Microsoft**，然后选择 **Windows**。 查找 **BootEvent-Collector**、将其展开，并查找**管理员**。  

-   使用 Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   在普通命令提示符下，使用：**wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>疑难解答  
  
### <a name="troubleshooting-installation-of-the-feature"></a>功能安装疑难解答  
  
||错误|错误说明|症状|潜在问题|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|在此上下文中无法识别 feature-name 选项||-   如果拼错了功能名称，可能会出现这种情况。 确认拼写正确，然后重试。<br />-   确认你正在使用的操作系统版本提供此功能。 在 Windows PowerShell 中，运行 **dism /online /get-features &#124; ?{$_ -match "boot"}** 。 如果未返回匹配项，则表示正在运行的版本可能不支持此功能。|  
|Dism.exe|0x800f080c|功能\<名称 > 是未知的。||同上|  
  
### <a name="troubleshooting-the-collector"></a>收集器疑难解答  
  
**日志记录：**  
收集器将其自身的事件记录为 ETW 提供程序 Microsoft-Windows-BootEvent-Collector。 你应该首先在此处尝试解决收集器问题。 你可以在事件查看器中的“应用程序和服务日志”>“Microsoft”>“Windows”>“BootEvent-Collector”>“管理员”下面查找它们，也可以在命令窗口中使用以下任一命令读取它们：  
  
在普通命令提示符下，使用：**wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
在 Windows PowerShell 提示符下，使用：`Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`（你可以附加 `-Oldest` 以按时间顺序返回列表，最旧的事件排在第一位）  
  
你可以通过“警告”、“信息”（默认）、“详细”和“调试”来调整日志中“错误”的详细级别。 设置比“信息”更详细的级别有助于诊断目标计算机无法连接的问题，但这些级别可能会生成大量数据，因此请谨慎使用。  
  
设置最小日志级别中\<收集器 > 配置文件元素。 例如： < 收集器 configVersionMajor ="1"minlog\="详细">。  
  
详细级别记录了处理每个数据包时收到的数据包记录。 调试级别还会添加更多处理详细信息，并转储所有收到的 ETW 数据包的内容。  
  
在调试级别，你可以将日志写入文件，而非尝试在常用的日志记录系统中查看日志。 若要执行此操作，将添加中的其他元素\<收集器 > 元素的配置文件：  
  
<collector configVersionMajor="1" minlog="debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
 **故障排除，收集器建议的方法：**  
   
1. 首先，使用以下命令验证收集器是否收到了来自目标的连接（仅在目标开始发送消息时，收集器才会创建文件）   
   ```  
   Get-SbecForwarding  
   ```  
   如果它返回的结果表明此目标中存在连接，则问题可能出在自动记录器设置中。 如果未返回任何结果，则问题出在要首先进行的 KDNET 连接中。 要诊断 KDNET 连接问题，请尝试从两端（即收集器和目标）检查连接。  
  
2. 若要查看从收集器扩展的诊断，请将添加到\<收集器 > 元素的配置文件：  
   \<collector ... minlog="verbose">  
   这将启用有关每个收到的数据包的消息。  
3. 检查是否确实收到了任何数据包。 （可选）你可能希望以详细模式将日志直接写入文件，而不是通过 ETW 写入文件。 若要执行此操作，将添加到\<收集器 > 元素的配置文件：  
   \<collector ... minlog="verbose" log="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt">  
      
4. 检查事件日志中是否有关于收到的数据包的任何消息。 检查是否确实收到了任何数据包。 如果数据包已收到但不正确，请检查事件消息以获取详细信息。  
5. KDNET 会从目标端将一些诊断信息写入注册表。 查找范围   
   **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** 中查找消息。  
   成功时 KdInitStatus (DWORD) 将为 0，出错时会显示错误代码  
   KdInitErrorString = 错误说明（如果没有错误，还包含信息性消息）  
  
6. 在目标上运行 Ipconfig.exe 并检查它所报告的设备名称。 如果正确加载了 KDNET，设备名称应该类似于“kdnic”，而不是原始供应商的卡片名称。  
7. 检查是否为目标配置了 DHCP。 KDNET 绝对需要 DHCP。  
8. 确认收集器与目标在同一网络上。 如果不在同一网络上，请检查是否正确配置了路由，尤其是 DHCP 的默认网关设置。  
  
  
**连接状态**  
  
你可以使用 `Get-SbecForwarding` 检查已建立连接的当前列表以及有关在何处转发数据的信息。  
  
你还可以使用 `Get-SbecHistory` 获取连接状态更改的最新历史记录。  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>设置新配置疑难解答  
如果你使用 Windows PowerShell 命令 `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` 应用了配置，则变量 `$result` 将包含有关部署的信息。 你可以查询此变量以从其中获取不同的信息：  
  
使用 `$result.ErrorString` 获取与错误相关的信息。 如果此处报告了任何错误，则尚未应用新配置，旧配置将保持不变。  
  
使用 `$result.WarningString` 获取警告。  
  
使用 `$result.InfoString` 获取有关配置详情的信息。  
  
你可以使用 `$result | fl *` 获取完整结果。  
或者，如果不想将结果保存到变量中，则可以使用 `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`。  
  
### <a name="troubleshooting-target-computers"></a>目标计算机疑难解答  
  
||错误|错误说明|症状|潜在问题|  
|-|---------|---------------------|-----------|---------------------|  
|目标计算机||目标未连接到收集器||-   目标计算机在配置后未重启。 重启目标计算机。<br />-   目标计算机的 BCD 设置不正确。 检查“验证目标计算机设置”部分中的设置。 根据需要进行更正，然后重启目标计算机。<br />-   KDNET/EVENT-NET 驱动程序无法连接到网络适配器或连接到了错误的网络适配器。 在 Windows PowerShell 中，运行 `gwmi Win32_NetworkAdapter`，并检查输出中是否有 ServiceName 为 **kdnic** 的项。 如果选择了错误的网络适配器，请重新执行“指定网络适配器”中的步骤。 如果没有显示任何网络适配器，则可能是驱动程序不支持任何网络适配器。<br>**另请参阅**上面的“建议的收集器疑难解答方法”，尤其是步骤 5 到步骤 8。|  
|收集器||迁移托管我的收集器的虚拟机后，我无法再看到事件。||验证是否未更改收集器计算机的 IP 地址。 如果已更改，请查看“若要允许通过传输远程发送 ETW 事件，请执行以下操作”。|  
|收集器||未创建 ETL 文件。|`Get-SbecForwarding` 显示目标已连接，并且没有错误，但无法创建 ETL 文件。|目标计算机可能尚未发送任何数据；仅在收到数据时才会创建 ETL 文件。|  
|收集器||ETL 文件中未显示事件。|目标计算机已发送事件，但是使用消息分析器的事件查看器读取 ETL 文件时，事件不存在。|-   事件可能仍在缓冲区中。 在收集完整的 64 KB 缓冲区之前，或者在大约 10-15 秒的超时（超时前未发生新事件）发生之前，事件不会写入 ETL 文件。 等待超时到期，或者用 `Save-SbecInstance` 刷新缓冲区。<br />-   事件清单在收集器计算机或运行事件查看器或消息分析器的计算机上不可用。  在这种情况下，收集器可能无法处理事件（请查看收集器日志）或者查看器可能无法显示事件。  较好的做法是将所有清单安装在收集器计算机上，并在收集器计算机上安装更新，然后将更新安装到目标计算机上。|
