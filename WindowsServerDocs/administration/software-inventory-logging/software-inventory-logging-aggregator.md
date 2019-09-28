---
title: 软件清单日志记录聚合器
description: 描述如何安装和管理软件清单日志记录聚合器
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4230a75-6bcd-47d9-ba92-a052a90a6abc
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81dbfb89d2e72af57c070db8473fd3b0e521906c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382937"
---
# <a name="software-inventory-logging-aggregator"></a>软件清单日志记录聚合器

>适用于：Windows Server 2012 R2

## <a name="what-is-software-inventory-logging-aggregator"></a>什么是软件清单日志记录聚合器？
软件清单日志记录聚合器 (SILA) 将在数据中心接收、聚合和生成 Windows Server 上所安装 Microsoft 企业软件的数量和类型的基本报表。

SILA 是安装在 Windows Server 上的软件，但未包含在 Windows Server 安装中。 若要安装软件，请先从 Windows 下载中心免费下载软件：[Windows Server 的软件清单日志记录聚合器1。0](https://www.microsoft.com/en-us/download/details.aspx?id=49046)

软件清单日志记录框架旨在降低清点 IT 环境中众多服务器上所部署 Microsoft 软件的操作成本。 此框架由两个组件组成：此 SIL 聚合器和 Windows Server 功能，在 Windows Server 2012 R2 中引入，软件清单日志记录（SIL）。 此软件清单日志记录聚合器 1.0 将安装在一个服务器上并从配置为通过 SIL 向其转发数据的任何 Windows Server 接收清单数据。 此设计允许数据中心管理员在准备在其环境中进行广泛分发的主 Windows Server 映像中启用 SIL。  此软件程序包为目标点并且专供客户进行本地安装，以便随着时间的推移轻松记录清单数据。 此软件还允许在 Microsoft Excel 中定期创建基本清单报表。 软件清单日志记录聚合器 1.0 报表包含安装 Windows Server、System Center 和 SQL Server 的计数。

> [!IMPORTANT]
> 使用此软件时不会向 Microsoft 发送任何数据。

### <a name="data-sil-collects-over-time"></a>数据 SIL 随着时间的推移进行收集
正确部署后，可以在 SIL 聚合器查看下列数据：

-   唯一的 Windows Server 安装在数据中心中

-   FQDN

-   标识 GUID

-   物理处理器和内核的数目

-   虚拟处理器（如果是虚拟机）的数目

-   物理处理器的模型和类型

-   是否在物理处理器上启用超线程

-   底盘序列号

-   随时间推移在每个主机上同时运行的 Windows Server VM（或运行虚拟机监控程序的主机）的高水位线计数和标识

-   随着时间推移，在每个主机上同时运行托管\(系统中心\)代理的高水位线计数和主机名称

-   安装在以托管高\-水位线计数的 vm 上的 System Center 代理名称

-   只需 sku 和需要许可证的版本\(，在一段时间内 SQL Server 安装的计数和位置\)

-   在 "添加\//删除程序" 中安装的软件的列表

### <a name="who-will-use-sil"></a>哪些人将使用 SIL？

-   **IT 专业人员或数据中心管理员**，他们寻求在一段时间内自动收集有价值的软件清单数据的低成本方法。

-   **首席信息官和财务**总监，他们需要报告其组织 IT 部署中的 Microsoft 企业软件的使用情况。

## <a name="getting-started"></a>入门
**必备条件**

至少一台服务器上（在 VM 中或物理硬件上）具有软件清单日志记录聚合器（SIL 聚合器）以用于聚合和生成报表：

-   **Windows Server 2012 R2**（Standard 或 Datacenter Edition）

-   **IIS 服务器角色**，随 .Net Framework 4.5、WCF Services 和 HTTP 激活一起添加，全部位于**添加角色和功能向导**的同一选择树中。

-   **SQL Server** 2012 SP2 Standard Edition 或 SQL Server 2014 Standard Edition

-   **64 位 Microsoft Excel** 2013（可选择安装，但创建报表时需要）

-   可选：**VMware PowerCLI 5.5.0.5836**（在 VMware 环境中需要）

>[!Note]
>使用 Windows Management Framework 时，仅在 SIL 聚合器上存在与 WMF 版本5.1 有关的已知兼容性问题。  安装 SIL 聚合器的服务器上不需要超过 WMF 版本4.0。

安装了以下更新的 Windows Server 版本中提供软件清单日志记录 (SIL)：

-   **Windows Server 2016**或更高版本

-   **Windows Server 2012 R2**（Standard 或 Datacenter Edition）

    -   Windows Server 2012 R2 更新 **KB3000850**（2014 年 11 月）

    -   Windows Server 2012 R2 更新 **KB3060681**（2015 年 6 月）（可能在 Windows 更新上显示为可选更新）

### <a name="security-and-account-types"></a>安全和帐户类型
**证书要求**

SIL 和 SIL 聚合器依靠 SSL 证书进行经过身份验证的通信。 此操作的常见实现方法是使用一个证书（服务器名称和证书名称相匹配）安装 SIL 聚合器以便托管接收清单数据的 Web 服务。 然后，要使用 SIL 功能进行清点的 Windows Server 将使用不同的客户端证书将数据推送到 SIL 聚合器。 需要使用 PowerShell cmdlet （Set-silaggregator，更多详细信息）将证书指纹添加到聚合器将接受关联数据的已批准证书的 SIL 聚合器列表。 在使用证书对数据的每个负载进行身份验证后，SIL 聚合器将继续处理数据并插入其数据库。 有关其工作原理的更多具体细节，请参阅 **SIL 聚合器 Cmdlet 详细信息** 部分

### <a name="polling-account-setup"></a>轮询帐户设置
向 SIL 聚合器添加凭据来启用轮询操作时，你应当使用最低特权帐户方法。 另外，作为最佳安全方案，你不应为数据中心或其他 IT 部署中的所有或多个主机使用相同的凭据。

在 Windows Server 主机上，如果你想要对其进行设置以便通过 SIL 聚合器进行轮询，并避免使用管理员组中的用户，请按照以下步骤操作以便向用户帐户授予足够的访问权限：

##### <a name="to-setup-a-polling-account"></a>设置轮询帐户

1.  在你想要通过 SIL 聚合器轮询的 Windows Server Hyper-V 主机上，使用 Windows 中的“计算机管理”创建一个本地用户帐户（务必取消选中强制在首次登录时更改密码的复选框）。

2.  将此用户添加到“远程管理用户”组。

3.  将此用户添加到“Hyper-V 管理员”组。

4.  用 "**开始**->**运行**" 打开**wmimgmt.msc** 。

5.  单击“操作” 部分中的“更多操作” ，然后选择“属性”。

6.  单击“安全”。

7.  选择“命名空间”树视图中的“cimv2 命名空间”。

8.  单击“安全”（按钮）。

9. 以“计算机名\组名”格式添加“远程管理用户”组

10. 单击 **“确定”** 。

11. 返回“root\cimv2”窗口的“安全”中，选择“远程管理用户”。

12. 在底部的 "权限" 部分中，确保选中 "**远程启用**"。

13. 单击“应用” ，然后单击“确定”。

14. 单击“属性” 窗口中的“确定” 。

### <a name="installing-sil-aggregator"></a>安装 SIL 聚合器
在 Windows Server 上安装 SIL 聚合器之前，需要确保以下几项内容：

-   你具有要用于托管此软件的 web 服务的**有效 SSL 证书**。

    -   证书应采用 **.pfx** 格式

    -   Windows Server 名称和证书名称应匹配。

-   **SQL Server Standard Edition 已安装**，或者已在你想要与此软件结合使用的远程服务器上安装。

    -   SIL 聚合器可用于 SQL Server 2012 SP2 和 SQL Server 2014。 在 SQL Server 安装过程中无需进行特殊的选择。

    -   用于安装 SIL 聚合器的帐户必须是 SQL 上的 sysadmin 角色，以便可以在安装过程中创建数据库。

    -   安装 SIL 聚合器之前，用于安装 SIL 聚合器的帐户应添加为 SQL Analysis Services 上的管理员。

    -   安装完成后，SQL Server 代理应配置为自动运行。

-   **IIS 服务器**已随 .Net Framework 4.5、WCF Services 和 HTTP 激活一起添加，全部位于**添加角色和功能向导**的同一选择树中。

-   你 **已使用在服务器上拥有管理权限的帐户登录到服务器** 。

-   你 **已使用在 SQL Server 上拥有 sysadmin 权限的帐户登录到服务器**（如果需要 Windows 身份验证）。

    或者

    如果需要 SQL 身份验证，**则需要使用拥有 SQL 管理权限的帐户的密码**。

##### <a name="to-install-software-inventory-logging-aggregator"></a>安装软件清单日志记录聚合器

1.  双击“Setup.exe” 开始安装。

2.  在欢迎窗口上单击“下一步” 。

3.  如果你接受最终用户许可协议，则选中接受该协议的复选框，然后单击“下一步”。

4.  在“选择功能”中，选择“安装软件清单日志记录聚合器和报告模块”，然后单击“下一步”。

    有关仅安装报告模块的详细信息，请参阅 **SIL 聚合器 Cmdlet 详细信息**部分下的 `Publish-SilReport` 。

5.  验证完所有的先决条件后，单击“下一步”。

6.  在“选择帐户类型”中，根据你的偏好选择“本地用户” 或“gMSA”。

    选择本地用户帐户选项将创建本地用户，并具有自动生成的强密码。 此帐户将用于本地服务器上的所有 SIL 聚合器服务和任务操作。  如果聚合器是 Active Directory 域（Windows Server 2012 及更高版本）的一部分，则建议使用组托管服务帐户 (gMSA)。 有关 gMSA 的详细信息，请参阅：[组托管服务帐户概述](https://technet.microsoft.com/library/hh831782.aspx)

    -   如果你打算从 SIL 聚合器运行一个单独服务器上的 SQL Server 数据库，则必须使用 gMSA 帐户选项。

    -   在 Active Directory 中将计算机帐户添加到启用了 gMSA 的安全组后，不要忘记重新启动服务器。

7.  在“选择 SQL Server”中，输入 SQL Server（SQL 实例的安装位置）；如果 SQL 实例安装在本地服务器上，则输入“localhost”。

    每个 SQL 实例仅支持有一个 SIL 聚合器。

8.  选择身份验证类型，然后单击“验证 SQL”。

9. 单击“下一步”，然后在“Internet 信息服务服务器详细信息”中，选择一个端口号或保留默认值。

10. 浏览“.pfx” 文件位置，键入 .pfx 文件的密码，然后单击“下一步”。

11. 最后一个屏幕将显示安装进度。 成功完成后，单击“完成”。

### <a name="uninstalling-sil-aggregator"></a>卸载 SIL 聚合器

##### <a name="to-uninstall-software-inventory-logging-aggregator"></a>卸载软件清单日志记录聚合器

1.  以管理员身份打开“PowerShell” ，然后键入 `Stop-SilAggregator`。 当提示符返回时，SIL 聚合器已停止。

    按照设计，SIL 聚合器将在 20 分钟之后或接收 100 个文件之后处理文件。  在大规模环境中决不会发生这种情况，但在小规模环境中，可能仍需要处理某些文件才能停止聚合器。 如果不需要保留这些文件和数据，请使用 `–Force` 。

2.  转到“控制面板”，依次单击“程序和功能”、“卸载程序”、“软件清单日志记录聚合器”和“卸载”。

    软件清单日志记录聚合器将打开一个窗口，提示你在删除数据库中所有数据与保留数据库中所有数据之间进行选择。 默认选择是保留（如果需要重新安装，你可以附加到现有数据库，以便从聚合器停止的地方开始）。

3.  选择“保留”或“删除”，然后单击“下一步”。

4.  进度栏完成后，单击“完成”。

### <a name="start-using-sil-and-the-sil-aggregator"></a>开始使用 SIL 和 SIL 聚合器

#### <a name="introduction-to-sil-aggregator-powershell-cmdlets"></a>SIL 聚合器 PowerShell cmdlet 简介
可以通过管理员身份从 Windows PowerShell 控制台运行以下命令。

|Windows PowerShell Cmdlet|Functions|
|-----------------------------|------------|
|`Start-SilAggregator`|启动所有软件清单日志记录聚合器服务和任务。 这是聚合器通过 HTTPS 从启用 SIL 日志记录的服务器接收数据的必需步骤。|
|`Stop-SilAggregator`|停止所有软件清单日志记录聚合器服务和任务。 如果任务或服务正在运行中，可能会延迟完成此命令。|
|`Set-SilAggregator`|允许管理员对软件清单日志记录聚合器进行配置更改。|
|`Add-SilVmHost`|用于添加特定主机名或主机名的数组，按固定间隔\(默认值为一小时的间隔。\)|
|`Remove-SilVmHost`|用于删除要每隔一定时间进行轮询的特定主机名或主机名的数组。|
|`Get-SilVMHost`|用于检索物理主机的列表，其中软件清单日志记录聚合器配置为针对正在运行状态数据的 VM 进行轮询。|
|`Get-SILAggregatorData`|用于检索从数据库到 PowerShell 控制台的数据。|
|`Publish-SilReport`|用于从软件清单日志记录数据的数据库创建报告。 **注意：** 每天在聚合器上进行一次多维数据集处理。 因此，在聚合器上捕获的数据直到第二天才会显示。|

#### <a name="suggested-order-to-start"></a>建议的启动顺序
在服务器上安装软件清单日志记录聚合器后，请以管理员身份打开 PowerShell。

-   在 SIL 聚合器上：

    -   运行 `Start-SilAggregator`

        这是必需的步骤，以便聚合器主动接收通过 HTTPS 从已（或将要）设置为要进行清点的服务器转发给它的数据。 请注意，即使你已首先将服务器设置为转发到此聚合器也没有问题，因为服务器将在本地缓存数据负载长达 30 天。 聚合器启动并运行后，所有缓存数据将一次转发到聚合器，并且所有数据都将进行处理。

    -   运行 `Add-SilVMHost`

        示例： `add-silvmhost –vmhostname contoso1 –hostcredential get-credential`

        -   在此示例中，**contoso1** 是你希望聚合器向其轮询以进行定期更新（对于哪些 VM 在其中运行以便在一段时间内跟踪此数据）的物理主机服务器的网络名称（或 IP 地址）。 Get-Credential 将提示已登录用户输入一个用于在此之后轮询该主机的帐户。 在同一主机上运行相同的命令将允许你随时更新所使用的帐户。 请谨防帐户密码在一段时间后发生更改以及过期。 如果凭据发生更改或过期，将无法在主机上进行轮询。

        -   默认情况下，轮询将每小时开始一次，在 `Start-SilAggregator` 运行一小时后或在轮询列表中新增主机后一小时开始。  使用 `Set-SilAggregator cmdlet` 可以更改轮询间隔。

        -   此 cmdlet 将自动从预设的选项列表中进行检测（请参阅 **SIL 聚合器 Cmdlet 详细信息**部分），其中 HostType 和 HyperVisorType 均符合你要添加的主机。 如果无法识别这些选项或提供的凭据不正确，将显示一条提示。 如果你接受“Y”条目，将添加主机并将其列为“未知”，但不会对其进行轮询。

    -   运行`Set-SilAggregator –AddCertificateThumbprint` "你的客户端证书的指纹"

        这是通过 HTTPS 从启用 SIL 日志记录的 Windows Server 接收数据必需的步骤。 指纹将添加到 SIL 聚合器将从其中接受数据的指纹列表中。 SIL 聚合器旨在接受有效的企业客户端身份验证证书。 使用的证书需要安装在转发数据的服务器上的 **\\localmachine\MY （本地计算机 > 个人**）存储中。

-   在要清点的 Windows Server 上，以管理员身份打开 PowerShell 并运行以下命令：

    -   运行 `Set-SilLogging –TargetUri "https://contososilaggregator" –CertificateThumbprint "your client certificate's thumbprint"`

        -   这将告知 Windows Server 中的 SIL 在何处发送清单数据以及使用哪个证书进行身份验证。

            > [!IMPORTANT]
            > 请确保 "https://" 是 TargetUri 值。

        -   具有此指纹的企业客户端证书需要安装在 **\localmachine\MY** 中或使用 **certmgr.msc** 将证书安装在“本地计算机”->“个人”存储中。

            > [!IMPORTANT]
            > 如果这些值不正确，或者如果证书未安装在正确的存储中（或无效），启动 SIL 日志记录时将无法转发到目标。 数据将在本地缓存长达 30 天。

    -   运行 `Start-SilLogging`

        这将启动 SIL 日志记录。 在每个小时内，SIL 会不定时地将其清单数据转发到使用 `–targeturi` 参数指定的聚合器。 首次转发的将是一个完整的数据集。 接下来，每次转发的都是 "检测信号"，只是标识未更改的数据。 如果对该数据集进行了任何更改，将转发另一个完整的数据集。

    -   运行 `Publish-SilData`

        -   首次启用 SIL 日志记录时，此步骤为可选步骤。

        -   这便是手动一次性转发一个完整的数据集。

        -   如果 SIL 日志记录已启动了一段时间并使用 `Set-SilLogging`指定了新的 SIL 聚合器，则需要运行此 cmdlet（仅一次）才能将完整的数据集发送到新的聚合器。

在按照这些步骤添加运行虚拟 Windows Server 计算机的物理主机后，并且在这些 Windows Server 内部启用软件清单日志记录（或 SIL 日志记录）后，可以在 SIL 聚合器上随时运行 `Publish-SilReport –OpenReport` （需要 Excel 2013）。 但是请注意，SQL Server Analysis Services 多维数据集每天处理一次，因此在同一天报表中不会提供数据。

## <a name="architectural-overview"></a>体系结构概述
SIL 在推送和拉取模式下工作，并且由两个并行工作的组件组成：Windows Server 中的软件清单日志记录（SIL）功能，以及软件清单日志记录聚合器（SILA）可下载的 MSI。 要清点的服务器会将软件清单数据通过 HTTPS 使用 SIL 推送到 SIL 聚合器（每小时内不定时推送）。 反过来，聚合器每小时都会轮询或查询物理虚拟机监控程序主机来拉取硬件清单数据。 需要对推送和拉取进行正确配置才能启用 SIL 的完整功能。 可以按任意顺序进行配置。 但是，聚合器上的多维数据集处理每天进行一次，因此在聚合器上捕获的数据（通过推送或拉取）在第二天才会显示。

![](../media/software-inventory-logging/SILA_Architecture.png)

> [!IMPORTANT]
> 使用此软件时不会向 Microsoft 发送任何数据。

## <a name="enable-sil-on-multiple-servers"></a>在多个服务器上启用 SIL
有多种方法可以在分布式服务器基础结构中（如在虚拟机的私有云中）启用 SIL。  下面举例说明一种对 Windows Server 进行设置的方式，以便当它们首次在网络上启动时自动向 SIL 聚合器发送清单数据。

在每台装有 Windows Server（请参阅 **先决条件** 部分）且运行 VM 的设备或物理计算机/设备上，以管理员身份在 PowerShell 控制台中运行以下 cmdlet：

将需要 .pfx 格式的有效客户端 SSL 证书才能使用这些步骤。  需要使用 `Set-SILAggregator –AddCertificateThumbprint` cmdlet 将此证书的指纹添加到 SIL 聚合器。 此客户端证书不需要与 SIL 聚合器的名称匹配。

-   `$secpasswd = ConvertTo-SecureString "`**<password for the account with permissions to the network location holding your client pfx file>**`" -AsPlainText –Force`

-   `$mycreds = New-Object System.Management.Automation.PSCredential ("`**<user account with permissions to the network location holding your client  pfx file>**`", $secpasswd)`

-   `$driveLetters = ([int][char]'C')..([int][char]'Z') | % {[char]$_}`

-   `$occupiedDriveLetters = Get-Volume | % DriveLetter`

-   `$availableDriveLetters = $driveLetters | ? {$occupiedDriveLetters -notcontains $_}`

-   `$firstAvailableDriveLetter = $availableDriveLetters[0]`

-   `New-PSDrive -Name $firstAvailableDriveLetter -PSProvider filesystem -root`**server\path 要共享的文件，其中保存了 pfx 证书文件 > < \\** `-credential $mycreds`

-   `Copy-Item ${firstAvailableDriveLetter}:\` **< 新驱动器目录中的 certificatename 文件 > c：\<你选择的位置 >**

-   `Remove-PSDrive –Name $firstAvailableDriveLetter`

-   `$mypwd = ConvertTo-SecureString -String "`**<password for the certificate pfx file>**`" -Force –AsPlainText`

-   `Import-PfxCertificate -FilePath c:\` **< location\\certificatename >** `cert:\localMachine\my -Password $mypwd`

-   `Set-sillogging –targeturi "https://` **<machinename of your SIL Aggregator>** `–certificatethumbprint`

> [!NOTE] 
> 使用客户端 pfx 文件中的证书指纹，并使用**Set-silaggregator AddCertificateThumbprint** cmdlet 将其添加到 SIL 聚合器。

-   `Start-sillogging`

每当不能到达 SIL 聚合器时，SIL 清单数据将以本地方式缓存在 Windows 服务器上长达 30 天。 成功推送到聚合器后，将转发所有缓存的数据。

如果在成功推送到旧的聚合器后，要向新的 SIL 聚合器推送 SIL 数据，请向上述列表添加 `Publish-SilData` （这将发送 SIL 数据的完整补集，新的聚合器需要将此用于该计算机）。

## <a name="software-inventory-logging-aggregator-reports"></a>软件清单日志记录聚合器报告
![](../media/software-inventory-logging/SILA_Report.png)

### <a name="cube-processing"></a>多维数据集处理
在软件清单日志记录聚合器上，SQL Server Analysis Services 多维数据集将每天处理一次，时间为本地系统时间凌晨 3:00:00。 在此之前，报表将显示所有数据，但在此之后（同一天），不会显示任何内容。

### <a name="high-water-mark"></a>高水位线
软件清单日志记录聚合器报告的一个基本方面是捕获通常称为 "高水位线" 的同时运行的 Windows 服务器。 这适用于这些报告中的 Windows Server 和 System Center 计数。 对于 Windows Server，每个物理主机（无论该主机上的操作系统是何种类型）在一个月之内都有一个大多数 Windows Server VM 同时运行的时间点。 这就是月高水位线。 此外，对于 System Center，每个物理主机在一个月之内都有一个大多数托管 Windows Server 同时运行的时间点（当存在一个或多个 System Center 代理时，将标识一个托管服务器）。 对于任何物理主机，报告中仅显示其最新的高水位线。 不会显示高水位线之后的任何数据。 并且可以假定 Windows Server VM（WS 选项卡）或托管 Windows Server VM（SC 选项卡）数量已低于在该点之后的高水位线。 这种跟踪和表示使用情况的方式旨在帮助对这些产品进行容量规划并与其许可模型匹配。

在报表中的 SQL 相关选项卡上，累积计数 SQL Server 安装;不是由 hig-水位线标记。 总数为 SQL Server 安装的运行计数。

> [!NOTE]
> 使用软件清单日志记录不会替换对适用的许可条款下的 Microsoft 软件使用情况进行准确报告的义务。

### <a name="poll-date-time"></a>轮询日期时间
使用软件清单日志记录聚合器时，请务必了解高水位线计数的聚合器是否靠轮询支持。 换而言之，只有基础物理主机的轮询才能捕获到高水位线。 因此，高水位线计数与相应的 "轮询日期时间" 直接关联。 尽管可调整轮询间隔，但如果使用较高的间隔值，捕获的高水位线的保真度将受到影响。 间隔越高，越无法表示数据的实际使用情况。

### <a name="reports-are-month-by-month"></a>按月提供报告
所有报告（即使是年度报告）都要以按月报告表示。 高水位线、总计以及计算机数据在每个公历月之初重置。

因转到新月份而受影响的报告数据包括：

-   所有主机的所有高水位线都会在新月份之初重置。

-   如果聚合器从 VM 收到至少一个完整负载（通过 HTTPS），但停止接收检测信号，则在该月份内，基础主机的所有轮询会将主机、VM 和 VM 数据之间的关联假定为该 VM 在整个月份中都处于运行或停止状态。 在新月份之初，将在从该 VM 收到完整负载或检测信号之前清除此关联。

### <a name="additional-notes-on-report-behavior"></a>有关报告行为的其他说明

-   摘要选项卡旨在提供清单的快速参考列表。 主机及其 VM 在同一列中列出。

-   忽略灰暗部分的所有值。这些是从 SSAS 多维数据集创建报表产生的项目。

-   如果某个 VM 列出了 "未知操作系统"，则意味着聚合器尚未通过 SIL 通过 HTTPS 从该 VM 收到完整的数据负载。

-   "未知主机" 下列出的 Vm 是 Windows Server Vm 已成功将清单数据通过 HTTPS 转发到聚合器，但聚合器不会主动或成功地轮询该 VM 的基础主机。 由于基础主机未知，这些条目的计数将始终为 0。 将 `Add-SilVMHost` cmdlet 与正确的凭据一起使用，从而将该主机（或所有主机）添加到 SIL 聚合器进行轮询。 轮询成功后，VM 数据和主机数据将在随后的报告中相关联。

-   所有日期和时间均为 SIL 聚合器系统本地的时间和区域设置。 这包括通过 HTTPS 从支持 SIL 的系统中收到的清单数据。 当处理这些文件时（收到后不超过 20 分钟），该数据将插入到使用本地系统时间的数据库中。

-   "SIL 聚合器" 将在已安装软件清单日志记录聚合器的任何服务器计算机上表示。

-   如果物理主机更改了处理器数量或物理内存量，新行与旧行将一起显示在报告中。 轮询更新将在旧行上停止，并在新行上继续进行，犹如它是新添加的主机一样。

-   在“摘要” 和“详细信息” 选项卡上，在同时运行的 Windows Server 或托管的 Windows Server 列中列出的总数可指示以下所有主机的所有高水位线总和。 其中包括不是虚拟机监控程序主机且没有运行任何 Vm 的 Windows 服务器，以及可能运行了 Vm 但 "未知" 的 vm 的服务器，因为未通过 HTTPS 从 SIL 接收到 VM 中的数据。 为方便起见，这些服务器总计在一起。

-   在“仪表板” 选项卡的“SQL Server” 部分中，SQL Server 安装的总计数是仪表板上所有版本的总和。  如果一台服务器上安装了多个版本的 SQL，可能会导致在“SQL 详细信息”选项卡上发现的总数之间出现差异。  仪表板会在每台服务器上分别计算这些版本的数量，而“详细信息” 选项卡则不会。  根据许可条款，安装在一台 Windows Server 上的多个 SQL 版本始终算做一个计数。

-   在“仪表板” 选项卡的“Windows Server” 部分中，“其他虚拟机监控程序主机” 行和“总虚拟机监控程序主机” 行包括可能运行或未运行 Hyper-V 的物理 Windows Server 主机。

### <a name="column-descriptions"></a>列描述
以下是有关每列的说明，这些列位于 SIL 聚合器基于 Excel 创建的报告的“Windows Server 详细信息”选项卡上。 其他数据选项卡或是相同，或是这些列的子集。 "SQL Server" 选项卡上的 "安装计数" 是一个例外情况（请参阅**高水位**线部分）。

|列标题|描述|
|-----------------|---------------|
|公历月|报告中的数据按月份分组，首先列出最近的月份。 一个月内的数据没有特定的列出顺序。|
|主机名|SIL 聚合器成功轮询的物理主机的网络名称或 FQDN。<br /><br />使用 Get-SilVMHost cmdlet 来查找已添加但未成功轮询或不再轮询的主机。 将显示上次成功的轮询。|
|主机类型|物理主机上的操作系统制造商。|
|虚拟机监控程序类型|物理主机上的虚拟机监控程序制造商。|
|处理器制造商|物理主机处理器的处理器制造商。|
|处理器型号|物理主机处理器的处理器型号。|
|是否已启用超线程？|显示为 True 或 False，具体取决于物理主机的处理器上是否已启用超线程。|
|VM 名称|Windows Server 虚拟机的网络名称或 FQDN。 如果聚合器未通过 HTTPS 从此计算机收到数据，将列出虚拟机监控程序中 VM 的友好名称。|
|由主机同时运行的 Windows Server VM|在主机上同时运行的 Windows Server VM 的计数。 该主机在一个月中的最高编号是在该时间点列出和捕获的高水位线计数。<br /><br />请参阅本文档的 **高水位线** 部分。<br /><br />已安装 Windows Server 或已安装 Windows Server 但未运行任何已知 Windows Server VM 的物理主机将始终有一个计数。 如果至少有一个已知 Windows Server VM 在该主机上运行，并且 Windows Server 在该主机本身上运行，主机操作系统则不属于计数的一部分。|
|物理处理器计数|安装在物理主机上的物理处理器数目。|
|物理核心计数|安装在物理主机上的物理处理器核心数目。|
|虚拟处理器计数|Windows 可在 VM 内部识别的虚拟处理器数目。 此值仅来自通过 HTTPS 在 Windows Server 中使用 SIL 转发的数据。|
|轮询日期时间|在该物理主机上同时运行的 Windows Server VM 最新高水位线点的日期和时间。<br /><br />请参阅本文档的 **轮询日期时间** 部分。|
|上次发现 VM 的日期时间|聚合器上次通过 HTTPS 从此 Windows Server VM 中接收数据清单的日期和时间。|
|上次发现主机的日期时间|聚合器上次通过 HTTPS 从此 Windows Server 物理主机中接收数据清单的日期和时间。<br /><br />支持运行 Windows Server 和 HyperV 的物理主机启用 SIL 并通过 HTTPS 向 SIL 聚合器转发清单数据。|

## <a name="sil-aggregator-cmdlets-detail"></a>SIL 聚合器 Cmdlet 详细信息
以下是 SIL 聚合器 cmdlet 的详细信息。 有关完整的 cmdlet 文档，请参阅：[SIL 聚合器 PowerShell cmdlet](https://technet.microsoft.com/library/mt548455.aspx)

### <a name="publish-silreport"></a>Publish-SilReport

-   按原样使用此 cmdlet 将创建一个软件清单日志记录报告并将其放置在已登录用户的文档目录中（在运行该 cmdlet 的计算机上需要 Excel 2013）。

-   与 `–OpenReport` 参数结合使用时，它将创建该报告并使用 Excel 打开进行查看。

-   你将注意到在安装 SIL 聚合器时，有一个仅安装报告模块的选项。 可以在 Windows 客户端操作系统上（如 Windows 8.1 或 Windows 10）安装报告模块。 从而允许瘦客户端（如笔记本电脑或平板电脑）连接到 SIL 聚合器数据库服务器以便直接发布 SIL 报告。

    -   以下示例将提示输入凭据以使用并连接到名为 SILContoso 的 SIL 聚合器数据库服务器，然后在本地计算机上创建和打开 SIL 报告。

        `Publish-SilReport -DBServerName "SILContoso" -DBServerCredential Get-Credential –OpenReport`

    -   在进行首次连接之前，通常需要在 SIL 聚合器数据库服务器上打开防火墙中的一个端口以允许连接。 IT 专业人员需要事先对此进行设置，以便财务总监或其他清单管理人员能够创建他们自己的报告。 有关执行此操作的步骤，请查看以下链接。 SQL Server Analysis Services 的典型默认端口为 2383。

### <a name="add-silvmhost"></a>Add-SilVMHost
当使用 `Add-SilVMHost` cmdlet 时，将支持以下主机类型和虚拟机监控程序版本。 请注意，不需要指定这些内容。 `Add-SilVMHost` cmdlet 将自动检测受支持的组合。 如果无法检测，或者提供的凭据不正确，将显示一条提示。 如果用户接受 "Y" 条目，将添加主机，但不会对其进行轮询。 它将添加为 "未知"。

|虚拟机监控程序版本|SIL 聚合器         HostType 值|SIL 聚合器 HypervisorType 值|
|----------------------|-----------------------------------------|---------------------------------------|
|Windows Server 2012 R2|Windows|HyperV|
|VMware 5.5|VMware|Esxi|
|Xen 4.x|Ubuntu、OpenSuse、或 CentOS|Xen|
|XenServer 6.2|Citrix|XenServer|
|KVM|Ubuntu、OpenSuse、或 CentOS|KVM|

这些虚拟机监控程序平台和类型的其他版本也许可以正常工作。  SIL 聚合器附带下面的 sshnet 版本。  这用于与基于 Linux 的虚拟化平台进行通信。

<pre>sshnet 2014.4.6-beta1
https://sshnet.codeplex.com/releases/view/120504
Copyright (c) 2010, RENCI</pre>

### <a name="get-silaggregator"></a>Get-SilAggregator
`Get-SilAggregator` 会为你的软件清单日志记录应用程序提供配置信息。 以下示例输出显示：

-   应用程序正在运行

-   轮询间隔为每小时（可以通过小时增量对其进行更改）

-   轮询开始时间

-   其他计算机应进行设置以便向此聚合器转发数据的目标 URI

-   此聚合器从中接受 SIL 数据的证书指纹

-   安装时指定的帐户类型

    `PS C:\Windows\system32> Get-SilAggregator`

    ``

    `State          : Running HostPollIntervalInHours : Every 1 Hour(s)`

    `PollStartTime      : 8/24/2015 5:07:33 AM`

    `TargetURI        : https://SilContoso`

    `CertificateThumbprint  : 3efc6b8ce7d5eefba5107ede9d1caca550417452, 2dc4ea8bfb64b1246a8c1ffa1b701cd1042a3412`

    `UserProfile       : Local`

### <a name="set-silaggregator"></a>Set-SilAggregator
使用 `Set-SilAggregator` cmdlet，你可以执行以下操作：

-   更改发生轮询的小时间隔。

-   更改轮询的开始日期和时间，以便以指定的间隔进行轮询。

-   添加或删除 SIL 聚合器从中接受数据或接受与之关联数据的证书指纹。

### <a name="get-aggregatordata"></a>Get-AggregatorData

-   当单独使用时，此 cmdlet 将显示 SIL 聚合器 Excel 报告的“Windows Server 详细信息”选项卡的内容。

-   与参数一起使用时，此 cmdlet 将直接从协助自定义使用 SIL 整体解决方案的数据库中检索数据。

-   请注意， `–StartTime` 和 `–Endtime` 参数将显示从开始日期的第一个月到结束日期的最后一个月的报告数据。

![](../media/software-inventory-logging/SILA_Get-SILAggregator.png)

### <a name="get-silvmhost"></a>Get-SilVMHost

-   此 cmdlet 将输出配置 SIL 聚合器后要轮询的物理主机、最新的成功轮询日期和时间、HostType（或操作系统制造商）以及 HypervisorType（虚拟机监控程序制造商）的列表。 有关 HostType 和 HypervisorType 的详细信息，请参阅“Add-SilVMHost 详细信息”。

    如果主机没有轮询日期和时间，但具有受支持的 HostType 和 HypervisorType，则意味着轮询尚未开始或尚未成功。

-   此 cmdlet 还将列出已通过来自 VM 本身的数据添加的主机名称（如果可从 VM 中获取数据）。 这些名称将显示在列表中，但不含有任何 HostType 或 HypervisorType。 此数据有助于协调可能未进行轮询设置的 VM 和主机。

-   使用 `–StartTime` 和`–EndTime` 参数有助于了解主机首次添加或最后一次轮询的时间。

### <a name="remove-silvmhost"></a>Remove-SilVMHost

-   此 cmdlet 将从要轮询的主机列表中删除任何主机。 如果删除了某个主机，该主机上的 VM 可能会将其重新添加到列表中，但不会使用正确的凭据（即使用 `Add-SilVMHost` cmdlet 指定的凭据）对该主机进行轮询。

-   如果删除了某个主机，将免于对其进行轮询，但不会从报告中将其删除。 由于轮询将停止，因此主机将不会显示在随后数月的报告中。

-   分别使用 `–StartTime` 和`–EndTime` 参数有助于删除多组在某个日期之前或从某个日期开始成功轮询的主机。

## <a name="avoid-these-errors-and-issues-with-sil-and-sil-aggregator-troubleshooting-guide"></a>避免 SIL 和 SIL 聚合器的这些错误和问题（疑难解答指南）

-   用于检查 `SilLogging` 或 `Publish-Sildata` cmdlet 是否失败或错误的事项：

    -   确保“targeturi” 在条目中含有“https://” 。

    -   确保已安装 Windows Server 的所有必需更新（请参阅 SIL 的先决条件）。  一种快速的检查方法是使用以下 cmdlet 查找这些内容：`Get-SilWindowsUpdate *3060*, *3000*`

    -   确保用于对聚合器进行身份验证的证书安装在要使用 SilLogging 进行清点的本地服务器的正确存储中（请参阅“快速入门”部分）。

    -   在 SIL 聚合器上，确保已使用 `Set-SilAggregator –AddCertificateThumbprint` cmdlet 将证书（用于对聚合器进行身份验证）的证书指纹添加到列表（请参阅“快速入门”部分）。

    -   如果使用企业证书，请检查启用 SIL 的服务器是否已加入为之而创建证书的域，或是否可通过根证书颁发机构进行验证。 如果证书在尝试向聚合器转发/推送数据的本地计算机上不受信任，此操作将失败并返回错误。

    -   如果已检查上述所有事项，可以检查用于安装 SIL 聚合器的证书是否处于正常状态，以及是否与 SIL 聚合器服务器本身的名称相匹配（如果其他计算机已成功转发到同一 SIL 聚合器，则无需执行此步骤）。

    -   你可以在尝试转发/推送 \Windows\System32\\日志\\文件 SIL 的服务器上查看缓存的 SIL 文件所在的位置。 如果 `SilLogging` 已启动并运行超过 1 小时，或者 `Publish-SilData` 最近已运行而此目录中没有任何文件，则表明已成功登录到聚合器。

-   确认已登录的用户拥有 SQL 数据库和 Analysis Services 访问权限。

    -   安装 SIL 聚合器时需要执行此步骤。

    -   当远程使用 PowerShell 来管理 SIL 聚合器时，这是必需的。

-   通过客户端桌面操作系统发布 SIL 聚合器报告。

    -   使用仅将报告模块安装在 Windows 客户端 (8.1/10) 上的选项。

    -   如果在远程使用 powershell 尝试创建报告时遇到问题，你可能需要在你想要连接到的 SIL 聚合器上打开防火墙端口（请参阅“SIL 聚合器 Cmdlet 详细信息”部分下的 `Publish-SilReport` Cmdlet）。

-   当使用 gMSA 选项时：

    -   在 Active Directory 中将服务器加入启用了 gMSA 的计算机组后，不要忘记重新启动服务器。

    -   在安装过程中，在输入 domain\user 时不要使用完全限定的域 例如，使用**mydomain\gmsaaccount**。 请勿输入**mydomain。<i> </i>com\gmsaaccount**。

-   在您的环境中使用 Windows Management Framework 时：

    -   确保安装了 SILA 的服务器未安装 WMF 5.1。  可能会在事件日志中命中有关 DLL **"mpunits"** 的错误。  这会阻止操作正常。  SILA 仅需要 WMF 4.0。

## <a name="managing-sil-over-time"></a>随着时间推移管理 SIL

### <a name="uninstallreinstall-sil-aggregator"></a>卸载/重新安装 SIL 聚合器
如果需要卸载并重新安装 SIL 聚合器，你可以执行此操作，而不会丢失现有和历史清单数据。 简单地进行卸载（按照本文档中提供的步骤操作），并选择保留软件清单日志记录数据库的选项。 然后重新安装 SIL 聚合器（按照本文档中提供的步骤操作），并选择附加到现有数据库的选项。

在执行此操作之后，需要在 SIL 聚合器之前轮询的所有主机上使用 `Add-SilVMHost` cmdlet 更新凭据（假设需要继续从这些主机中收集数据）。 此外，为了避免在报告中重复出现同一主机的条目，需要使用与最初添加的地址相同的网络地址重新添加主机进行轮询。 以下三种受支持的 vmhostname 类型可用于通过 `Add-SilVMHost` cmdlet 添加主机：

-   IP 地址

-   完全限定的域名 (FQDN)

-   NetBIOS 名称

### <a name="changing-sil-aggregators"></a>更改 SIL 聚合器
如果你想要使用不同的 SIL 聚合器开始清点环境中的服务器，只需在这些服务器上使用 SIL cmdlet 更改 targeturi（如有必要还需更改证书指纹）、`Set-SilLogging –TargetUri` 即可。 请注意在执行此操作后，需要至少使用一次 `Publish-SilData` ，以便向新指定的 SIL 聚合器转发一份完整的清单。

### <a name="changing-or-updating-certificates"></a>更改或更新证书
**避免数据丢失的重要步骤：** 如果需要更改服务器用于将数据转发到 SIL 聚合器的证书，但目标聚合器保持不变，请使用以下步骤以避免在向聚合器传输数据时可能发生的数据丢失：

-   在 SIL 聚合器上，使用 `Set-SilAggregator –AddCertificateThumbprint` cmdlet 向 SIL 聚合器添加新的指纹。

-   在所有转发数据的服务器上，使用你喜欢的方法安装要用于 **\LOCALMACHINE\MY** 的新证书。

-   在所有转发数据的服务器上，使用 `Set-SilLogging –CertificateThumbprint` cmdlet 更新到新证书的指纹。

-   **来说只有在所有转发数据的服务器都更新后，才能使用** `Set-SilAggregator –RemoveCertificateThumbprint` cmdlet 从 SIL 聚合器中删除旧指纹。 如果转发数据的服务器继续使用已从 SIL 聚合器中删除的旧证书进行转发， **数据将丢失** 并且不会插入到聚合器上的数据库中。 这只会影响以下情况：服务器之前已成功地将数据转发到 SIL 聚合器，然后从 SIL 聚合器的指纹列表中删除该证书以接受来自的数据。

## <a name="release-notes"></a>发行说明

-   存在 SIL 聚合器将不会处理的已知问题并报告已安装 SQL Server Standard Edition。  下面是更正此问题的步骤：

    1.  打开 SIL 聚合器上的 SQL Server Management Studio。

    2.  连接到数据库引擎。

    3.  在选择树中依次展开 SoftwareInventoryLogging 数据库和表。

    4.  右键单击 " **dbo"。Dbo.sqlserveredition**，然后选择 "**编辑前200行**"。

    5.  将 "Standard Edition" 旁边的 PropertyNumValue 更改为**2760240536** （从-1534726760）。

    6.  关闭查询以保存更改。

    7.  对于运行已将数据记录到此聚合器的 SIL 的任何服务器，可能有必要运行 `Publish-SilData` Cmdlet 一次以便聚合器正确处理存在 SQL Server Standard Edition 的问题。

-   在 SIL 生成的报告中，如果在物理服务器上启用了超线程，所有处理器核心计数都将包含线程计数。  若要在已启用超线程的服务器上获取实际物理核心计数，需要将这些计数减半。

-   对于 Windows Server 和 System Center 标记的行（在 "**仪表板**" 选项卡上）和列（**在 "** **摘要" 和 "详细信息**" 选项卡上）的合计在两个位置之间并不完全匹配。 在 "**仪表板**" 选项卡上，需要将 "**Windows Server 设备（无已知 vm**）" 值添加到 "**同时运行**..."在 "**摘要" 和 "详细信息**" 选项卡上等于此数字的值。

-   当更改或更新证书时，请参阅本文档 **随着时间推移管理 SIL** 部分下的 **避免数据丢失的重要步骤** 。

-   尽管可以将 Windows Server 2008 R2 和 Windows Server 2012 主机添加到轮询主机列表中，但对于基于 Windows/Hyper-V 的主机，此版本 (1.0) 的 SIL 聚合器仅支持轮询 Windows Server 2012 R2，以便成功使用所有特性和功能。  特别是，已知当轮询 Windows Server 2008 R2 主机时，虚拟机和主机可能在 SIL 聚合器报告中不匹配。

## <a name="see-also"></a>请参阅
[Windows Server 的软件清单日志记录聚合器1。0](https://www.microsoft.com/en-us/download/details.aspx?id=49046)<br>
[SIL 聚合器 PowerShell cmdlet](https://technet.microsoft.com/library/mt548455.aspx)<br>
[SIL PowerShell cmdlet](https://technet.microsoft.com/library/dn283390.aspx)<br>
[SIL 概述](https://technet.microsoft.com/library/dn268301.aspx)<br>
[管理 SIL](https://technet.microsoft.com/library/dn383584.aspx)

