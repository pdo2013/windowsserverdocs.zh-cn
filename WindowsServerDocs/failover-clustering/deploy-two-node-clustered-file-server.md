---
title: 部署双节点群集文件服务器
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: 本文介绍如何创建双节点文件服务器群集
ms.openlocfilehash: 03e78495b3fc85449d3d383706fb82541dd10372
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361305"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>部署双节点群集文件服务器

> 适用于：Windows Server 2019、Windows Server 2016

故障转移群集是一组独立的计算机，这些计算机相互协作以提高应用程序和服务的可用性。 多台群集服务器（称为节点）通过物理电缆和软件连接。 如果其中一个群集节点出现故障，另一个节点便会开始提供服务（此过程称为故障转移）。 用户最少遇到服务中断的情况。

本指南介绍了安装和配置具有两个节点的常规用途文件服务器故障转移群集的步骤。 通过在本指南中创建配置，可以了解故障转移群集并熟悉 Windows Server 2019 或 Windows Server 2016 中的故障转移群集管理管理单元接口。

## <a name="overview-for-a-two-node-file-server-cluster"></a>双节点文件服务器群集概述

故障转移群集中的服务器可以在各种角色（包括文件服务器、Hyper-v 服务器或数据库服务器的角色）中运行，并且可以为各种其他服务和应用程序提供高可用性。 本指南介绍如何配置双节点文件服务器群集。

故障转移群集通常包含一个物理连接到群集中的所有服务器的存储单元，但存储中的任何给定卷一次只能由一个服务器访问。 下图显示了连接到存储单元的双节点故障转移群集。

![双节点群集](media/Cluster-File-Server/Cluster-FS-Overview.png)

公开给群集中节点的存储卷或逻辑单元号（Lun）不得向其他服务器（包括另一群集中的服务器）公开。 下图对此进行了说明。

![存储中的 Lun](media/Cluster-File-Server/Cluster-FS-LUNs.png)

请注意，为了获得任何服务器的最高可用性，请务必遵循服务器管理的最佳做法，例如，仔细管理服务器的物理环境、在完全实现软件更改之前对其进行测试，以及仔细在所有群集服务器上跟踪软件更新和配置更改。

以下方案描述了如何配置文件服务器故障转移群集。 共享的文件位于群集存储上，而群集服务器可充当共享这些文件的文件服务器。

## <a name="shared-folders-in-a-failover-cluster"></a>故障转移群集中的共享文件夹

以下列表描述了集成到故障转移群集中的共享文件夹配置功能：

- 显示范围仅限于群集共享文件夹（不与非群集共享文件夹混合）：当用户通过指定群集文件服务器的路径查看共享文件夹时，该显示将仅包含属于特定文件服务器角色的共享文件夹。 它将排除非群集共享文件夹，并共享发生在群集节点上的独立文件服务器角色的一部分。

- 基于访问权限的枚举：您可以使用基于访问权限的枚举从用户的视图中隐藏指定的文件夹。 您可以选择防止用户查看文件夹，而不是允许用户查看文件夹，而不是访问它。 您可以使用与非群集共享文件夹相同的方式为群集共享文件夹配置基于访问权限的枚举。

- 脱机访问：你可以使用与非群集共享文件夹相同的方式为群集共享文件夹配置脱机访问（缓存）。

- 群集磁盘始终被识别为群集的一部分：无论你使用故障转移群集接口、Windows 资源管理器，还是共享和存储管理管理单元，Windows 都会识别磁盘是否已指定为位于群集存储中。 如果已在故障转移群集管理中将此类磁盘配置为群集文件服务器的一部分，则可以使用前面提到的任何接口在该磁盘上创建共享。 如果没有将此类磁盘配置为群集文件服务器的一部分，则无法在其上错误地创建共享。 相反，错误表示必须先将磁盘配置为群集文件服务器的一部分，然后才能共享该磁盘。

- 网络文件系统服务的集成：Windows Server 中的文件服务器角色包括称为网络文件系统（NFS）服务的可选角色服务。 通过安装角色服务并配置包含 NFS 服务的共享文件夹，你可以创建支持基于 UNIX 的客户端的群集文件服务器。

## <a name="requirements-for-a-two-node-failover-cluster"></a>双节点故障转移群集的要求

对于 Windows Server 2016 或 Windows Server 2019 中的故障转移群集，若要将其视为受 Microsoft 支持的解决方案，该解决方案必须符合以下条件。

- 所有硬件和软件组件必须符合相应徽标的资格要求。 对于 Windows Server 2016，这是 "针对 Windows Server 2016 进行了认证" 徽标。 对于 Windows Server 2019，这是 "针对 Windows Server 2019 进行了认证" 徽标。 有关哪些硬件和软件系统经过认证的详细信息，请访问 Microsoft [Windows Server 目录](https://www.windowsservercatalog.com/default.aspx)站点。

- 完全配置的解决方案（服务器、网络和存储）必须通过验证向导中的所有测试，这是 "故障转移群集" 管理单元的一部分。

双节点故障转移群集需要以下各项。

- **服务器**建议使用具有相同或相似组件的匹配计算机。  双节点故障转移群集的服务器必须运行同一版本的 Windows Server。 它们还应该具有相同的软件更新（修补程序）。

- **网络适配器和电缆：** 与故障转移群集解决方案中的其他组件一样，网络硬件必须与 Windows Server 2016 或 Windows Server 2019 兼容。 如果使用 iSCSI，则必须将网络适配器专用于网络通信或 iSCSI，而不能同时用于两者。 在连接群集节点的网络基础结构中，应避免出现单点故障。 有多种方法可以实现这一点。 可以通过多个不同的网络来连接群集节点。 或者，还可以使用一个由以下项组成的网络来连接群集节点：成组网络适配器、冗余交换机、冗余路由器或消除了单一故障点的类似硬件。

   > [!NOTE]
   > 如果群集节点已连接到单个网络，则网络将通过 "验证配置向导" 中的冗余要求。  但是，报表将包含一条警告，指出网络不应具有单点故障。

- **用于存储的设备控制器或相应适配器：**
    - **串行连接 SCSI 或光纤通道：** 如果使用串行连接 SCSI 或光纤通道，则在所有群集服务器中，存储堆栈的所有组件都应相同。 需要多路径 i/o （MPIO）软件和设备特定模块（DSM）软件组件是相同的。  建议连接到群集存储的大容量存储设备控制器—即主机总线适配器 (HBA)、HBA 驱动程序以及 HBA 固件都相同。 如果使用不同的 HBA，则应向存储供应商验证你是否采用了其支持或推荐的配置。
    - **/Iscsi**如果使用的是 iSCSI，则每台群集服务器必须具有一个或多个专用于 ISCSI 存储的网络适配器或主机总线适配器。 用于 iSCSI 的网络不能用于网络通信。 在所有群集服务器中，用于连接 iSCSI 存储目标的网络适配器都应相同，建议使用千兆或更高速度的以太网。  

- **储存**必须使用针对 Windows Server 2016 或 Windows Server 2019 认证的共享存储。
  
    对于双节点故障转移群集，如果使用见证磁盘进行仲裁，存储应至少包含两个单独的卷（Lun）。 见证磁盘是群集存储中的一个磁盘，它被指定用于保存群集配置数据库的副本。 对于这两节点群集示例，仲裁配置将是 "节点和磁盘多数"。 节点和磁盘多数意味着节点和见证磁盘都包含群集配置的副本，而群集具有仲裁，但前提是这些副本的多数（共三个）可用。 其他卷（LUN）将包含用户正在共享的文件。

    存储要求如下：

    - 若要使用故障转移群集中包含的本机磁盘支持，请使用基本磁盘（而非动态磁盘）。
    - 建议你将分区格式化为 NTFS （对于见证磁盘，分区必须为 NTFS）。
    - 对于磁盘分区形式，可使用主启动记录 (MBR) 或 GUID 分区表 (GPT)。
    - 存储必须正确响应特定 SCSI 命令，存储必须遵循称为 SCSI 主要命令3（SPC-3）的标准。 特别是，存储必须支持 SPC-3 标准中指定的持久保留。 
    -  用于存储的微型端口驱动程序必须适用于 Microsoft Storport 存储驱动程序。

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>使用故障转移群集部署存储区域网络

使用故障转移群集部署存储区域网络（SAN）时，应遵循以下准则。

- **确认存储的证书：** 使用[Windows Server 目录](https://www.windowsservercatalog.com/default.aspx)站点，确认供应商的存储（包括驱动程序、固件和软件）是否经过 Windows server 2016 或 windows server 2019 的认证。

- **隔离存储设备，每个设备一个群集：** 来自不同群集的服务器不能访问同一存储设备。 在大多数情况下，用于一组群集服务器的 LUN 应通过 LUN 屏蔽或分区与所有其他服务器隔离。

- **请考虑使用多路径 i/o 软件：** 在高度可用的存储构造中，你可以使用多路径 i/o 软件来部署带有多个主机总线适配器的故障转移群集。 这可以提供最高级别的冗余和可用性。 多路径解决方案必须基于 Microsoft 多路径 i/o （MPIO）。 存储硬件供应商可以为硬件提供 MPIO 设备特定模块（DSM），尽管 Windows Server 2016 和 Windows Server 2019 包含一个或多个 Dsm 作为操作系统的一部分。

## <a name="network-infrastructure-and-domain-account-requirements"></a>网络基础结构和域帐户要求

你将需要以下双节点故障转移群集的网络基础结构和具有以下域权限的管理帐户：

- **网络设置和 IP 地址：** 为网络使用相同的网络适配器时，还可以在这些适配器上使用相同的通信设置（例如，速度、双工模式、流控制和媒体类型）。 此外，还要比较网络适配器和它连接到的交换机之间的设置，并确保设置不冲突。

    如果你有还未路由到网络基础结构其余部分的专用网络，请确保每个专用网络都使用唯一的子网。 即使你为每个网络适配器提供了唯一的 IP 地址，此操作也是必需的。 例如，如果你在使用一个物理网络的中央办公室中有群集节点并在使用单独物理网络的分支机构中有另一个节点，不要为这两个网络指定 10.0.0.0/24，即使你为每个适配器提供了唯一的 IP 地址。

    有关网络适配器的详细信息，请参阅本指南前面的双节点故障转移群集的硬件要求。

- **DNS**群集中的服务器必须使用域名系统（DNS）来进行名称解析。 可以使用 DNS 动态更新协议。

- **域角色：** 群集中的所有服务器必须位于同一个 Active Directory 域中。 作为最佳做法，所有群集服务器都应具有相同的域角色（成员服务器或域控制器）。 推荐的角色为成员服务器。

- **域控制器：** 建议将群集服务器作为成员服务器。 如果是，则需要在包含故障转移群集的域中充当域控制器的其他服务器。

- **客户端**根据测试的需要，你可以将一个或多个联网的客户端连接到你创建的故障转移群集，并在将群集文件服务器从一个群集节点移动或故障转移到另一个群集节点时，观察客户端上的影响。

- **用于管理群集的帐户：** 首次创建群集或向其中添加服务器时，必须使用对该群集中的所有服务器具有管理员权限的帐户登录到域。 该帐户不需要是域管理员帐户，但可以是域用户帐户，该帐户位于每个群集服务器的 Administrators 组中。 此外，如果该帐户不是域管理员帐户，则必须为该帐户（或该帐户所在的组）提供域组织单位（OU）中的 "**创建计算机对象**" 和 "**读取所有属性**" 权限，驻留在中。

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>安装双节点文件服务器群集的步骤

若要安装双节点文件服务器故障转移群集，必须完成以下步骤。

第 1 步：将群集服务器连接到网络和存储

步骤 2：安装故障转移群集功能

步骤 3:验证群集配置

步骤 4：创建群集

如果你已经安装了群集节点并想要配置文件服务器故障转移群集，请参阅本指南后面的配置双节点文件服务器群集的步骤。

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>第 1 步：将群集服务器连接到网络和存储

对于故障转移群集网络，应避免出现单点故障。 有多种方法可以实现这一点。 可以通过多个不同的网络来连接群集节点。 或者，你可以将群集节点与通过组合网络适配器、冗余交换机、冗余路由器或可消除单故障点的类似硬件一起构造的一个网络连接起来（如果你使用 iSCSI 的网络，则必须创建此网络以及其他网络）。

对于双节点文件服务器群集，将服务器连接到群集存储时，必须至少公开两个卷（Lun）。 你可以根据需要公开其他卷来彻底测试配置。 不要向群集以外的服务器公开群集卷。

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>将群集服务器连接到网络和存储

1. 有关双节点故障转移群集和网络基础结构的详细信息，请参阅本指南前面的双节点故障转移群集的网络基础结构和域帐户要求。

2. 连接和配置群集中的服务器将使用的网络。

3. 如果测试配置包括客户端或非群集域控制器，请确保这些计算机可以通过至少一个网络连接到群集服务器。

4. 按照制造商的有关将服务器物理连接到存储的说明进行操作。

5. 确保向你将群集的服务器公开（仅公开这些服务器）你要在群集中使用的磁盘 (LUN)。 你可以使用以下任一接口来公开磁盘或 Lun：

    - 该接口由存储的制造商提供。

    - 如果使用的是 iSCSI，则使用合适的 iSCSI 接口。

6. 如果你已购买控制磁盘格式或功能的软件，请按照供应商提供的有关如何将该软件用于 Windows Server 的说明进行操作。

7. 在要进行群集的一台服务器上，依次单击 "开始"、"管理工具" 和 "计算机管理"，然后单击 "磁盘管理"。 （如果出现 "用户帐户控制" 对话框，请确认它显示的是所需操作，然后单击 "继续"。）在 "磁盘管理" 中，确认群集磁盘可见。

8. 如果你想要有大于 2 TB 的存储卷，并且使用 Windows 接口来控制磁盘的格式，则将该磁盘转换为称为 GUID 分区表 (GPT) 的分区形式。 为此，请备份磁盘上的所有数据，删除磁盘上的所有卷，然后在 "磁盘管理" 中右键单击该磁盘（而非分区），然后单击 "转换为 GPT 磁盘"。  对于小于 2 TB 的卷，你可以使用称为主启动记录 (MBR) 的分区形式，而不是使用 GPT。

9. 检查公开的任何卷或 LUN 的格式。 建议使用 NTFS 格式（对于见证磁盘，必须使用 NTFS）。

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>步骤 2：安装文件服务器角色和故障转移群集功能

在此步骤中，将安装文件服务器角色和故障转移群集功能。 这两台服务器都必须运行 Windows Server 2016 或 Windows Server 2019。

#### <a name="using-server-manager"></a>使用服务器管理器

1. 打开**服务器管理器**并在 "**管理**" 下拉下，选择 "**添加角色和功能**"。

   ![添加功能](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. 如果 "**开始之前**" 窗口打开，请选择 "**下一步**"。

3. 对于**安装类型**，请选择 "**基于角色或基于功能的安装**"，然后选择 "**下一步**"。

4. 确保**选择 "从服务器池中选择服务器**"，将突出显示计算机的名称，然后单击 "**下一步**"。

5. 对于服务器角色，请在角色列表中打开 "**文件服务**"，选择 "**文件服务器**"，然后单击 "**下一步**"。

   ![添加角色](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. 对于功能，从功能列表中选择 "**故障转移群集**"。  将显示一个弹出对话框，其中列出了也要安装的管理工具。  保留所有选定的，选择 "**添加功能**"，然后选择 "**下一步**"。

   ![添加功能](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. 在 "确认" 页上，选择 "安装"。

8. 安装完成后，重新启动计算机。

9. 在第二台计算机上重复上述步骤。

#### <a name="using-powershell"></a>使用 PowerShell

1. 右键单击 "开始" 按钮，然后选择 " **Windows powershell （管理员）** "，打开管理 PowerShell 会话。
2. 若要安装文件服务器角色，请运行以下命令：

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. 若要安装故障转移群集功能及其管理工具，请运行以下命令：

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. 完成后，可以使用以下命令来验证它们是否已安装：

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. 确认它们安装完成后，请通过以下命令重新启动计算机：

    ```PowerShell
    Restart-Computer
    ```

6. 在第二个服务器上重复上述步骤。

### <a name="step-3-validate-the-cluster-configuration"></a>步骤 3:验证群集配置

在创建群集之前，我们强烈建议你验证配置。 验证可帮助你确认服务器、网络和存储的配置是否满足故障转移群集的一组特定要求。

#### <a name="using-failover-cluster-manager"></a>使用故障转移群集管理器

1. 在**服务器管理器**中，选择 "**工具**" 下拉菜单，然后选择 "**故障转移群集管理器**"。

2. 在**故障转移群集管理器**中，请参阅 "**管理**" 下的中间列，然后选择 "**验证配置**"。

3. 如果 "**开始之前**" 窗口打开，请选择 "**下一步**"。

4. 在 "**选择服务器或群集**" 窗口中，添加将成为群集节点的两台计算机的名称。  例如，如果名称为节点1和节点2，则输入名称，然后选择 "**添加**"。  还可以选择 "**浏览**" 按钮，搜索名称 Active Directory。  一旦 "**选定的服务器**" 下列出，请选择 "**下一步**"。

5. 在 "**测试选项**" 窗口中，选择 "**运行所有测试（推荐）** "，然后选择 "**下一步**"。

6. 在 "**确认**" 页上，将显示它将检查的所有测试的列表。  选择 "**下一步**"，将开始测试。

7. 完成后，"**摘要**" 页会在测试运行后出现。 若要查看帮助主题以帮助你解释结果，请单击 "**有关群集验证测试的详细信息**"。

8. 仍在 "摘要" 页上，单击 "查看报告" 并阅读测试结果。 在配置中进行任何必要的更改，然后重新运行测试。 <br>若要在关闭向导后查看测试结果，请参阅*SystemRoot\Cluster\Reports\Validation Report date and time。*

9. 若要在关闭向导后查看关于群集验证的帮助主题，请在 "故障转移群集管理" 中单击 "帮助"，单击 "帮助主题"，单击 "内容" 选项卡，展开故障转移群集帮助的内容，然后单击验证故障转移群集配置.

#### <a name="using-powershell"></a>使用 PowerShell

1. 右键单击 "开始" 按钮，然后选择 " **Windows powershell （管理员）** "，打开管理 PowerShell 会话。

2. 若要验证故障转移群集的计算机（例如，计算机名为节点1和节点2），请运行命令：

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. 若要在关闭向导后查看测试结果，请参阅指定的文件（在 SystemRoot\Cluster\Reports @ no__t 中），然后在配置中进行任何必要的更改，然后重新运行测试。

有关详细信息，请参阅[验证故障转移群集配置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11))。

### <a name="step-4-create-the-cluster"></a>步骤 4：创建群集

以下将在计算机上创建群集，并创建群集。

#### <a name="using-failover-cluster-manager"></a>使用故障转移群集管理器

1. 在**服务器管理器**中，选择 "**工具**" 下拉菜单，然后选择 "**故障转移群集管理器**"。

2. 在**故障转移群集管理器**中，请参阅 "**管理**" 下的中间列，然后选择 "**创建群集**"。

3. 如果 "**开始之前**" 窗口打开，请选择 "**下一步**"。

4. 在 "**选择服务器**" 窗口中，添加将成为群集节点的两台计算机的名称。  例如，如果名称为节点1和节点2，则输入名称，然后选择 "**添加**"。  还可以选择 "**浏览**" 按钮，搜索名称 Active Directory。  一旦 "**选定的服务器**" 下列出，请选择 "**下一步**"。

5. 在 "**用于管理群集的访问点**" 窗口中，输入要使用的群集的名称。  请注意，这不是你将用于连接到文件共享的名称。  这仅用于管理群集。

   > [!NOTE]
   > 如果你使用的是静态 IP 地址，则需要选择要使用的网络，并输入它将用于群集名称的 IP 地址。  如果使用 DHCP 作为 IP 地址，将自动为你配置 IP 地址。

6. 选择 "**下一步**"。

7. 在 "**确认**" 页上，验证已配置的内容，然后选择 "**下一步**" 以创建群集。

8. 在 "**摘要**" 页上，它将为你提供创建它的配置。  您可以选择 "查看报告" 以查看创建的报告。

#### <a name="using-powershell"></a>使用 PowerShell

1. 右键单击 "开始" 按钮，然后选择 " **Windows powershell （管理员）** "，打开管理 PowerShell 会话。

2. 如果使用的是静态 IP 地址，请运行以下命令创建群集。  例如，计算机名称是节点1和节点2，群集名称将是群集，IP 地址将为1.1.1.1。

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. 如果使用 DHCP 进行 IP 地址，请运行以下命令创建群集。  例如，计算机名称是节点1和节点2，群集的名称将为 "群集"。

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>配置文件服务器故障转移群集的步骤

若要配置文件服务器故障转移群集，请执行以下步骤。

1. 在**服务器管理器**中，选择 "**工具**" 下拉菜单，然后选择 "**故障转移群集管理器**"。

2. 当故障转移群集管理器打开时，它应自动引入你创建的群集的名称。  如果没有，请在 "**管理**" 下参阅中间列，然后选择 "**连接到群集**"。  输入创建的群集的名称并**确定**。

3. 在控制台树中，单击你创建的群集旁边的 ">" 符号，展开其下的项。

4. 右键单击 "**角色**"，然后选择 "**配置角色**"。

5. 如果 "**开始之前**" 窗口打开，请选择 "**下一步**"。

6. 在角色列表中，选择 "**文件服务器**" 和 "**下一步**"。

7. 对于 "文件服务器类型"，请选择 "**文件服务器**" "常规" 和 "**下一步**"。<br>有关横向扩展文件服务器的信息，请参阅[横向扩展文件服务器概述](sofs-overview.md)。

   ![文件服务器类型](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. 在 "**客户端访问点**" 窗口中，输入要使用的文件服务器的名称。  请注意，这不是群集的名称。  这适用于文件共享连接。  例如，如果我想连接到 \\SERVER，则名称输入为 "服务器"。

   > [!NOTE]
   > 如果你使用的是静态 IP 地址，则需要选择要使用的网络，并输入它将用于群集名称的 IP 地址。  如果使用 DHCP 作为 IP 地址，将自动为你配置 IP 地址。

6. 选择 "**下一步**"。

7. 在 "**选择存储**" 窗口中，选择将保存共享的其他驱动器（而非见证服务器），然后选择 "**下一步**"。

8. 在 "**确认**" 页上，验证你的配置，然后选择 "**下一步**"。

9. 在 "**摘要**" 页上，它将为你提供创建它的配置。  您可以选择 "查看报告" 以查看文件服务器角色创建的报告。

10. 在控制台树中的 "**角色**" 下，你将看到你创建的新角色列出为你创建的名称。  突出显示后，在右侧的 "**操作**" 窗格中，选择 "**添加共享**"。

11. 运行共享向导并输入以下内容：

    - 共享的类型
    - 共享文件夹的位置/路径
    - 用户将连接到的共享的名称
    - 其他设置，例如基于访问的枚举、缓存、加密等
    - 文件级权限（如果它们将不是默认值）

12. 在 "**确认**" 页上，验证已配置的内容，然后选择 "**创建**" 以创建文件服务器共享。

13. 如果创建了共享，请在 "**结果**" 页上选择 "关闭"。  如果它无法创建共享，则会出现错误。

14. 选择 "**关闭**"。

15. 对任何其他共享重复此过程。