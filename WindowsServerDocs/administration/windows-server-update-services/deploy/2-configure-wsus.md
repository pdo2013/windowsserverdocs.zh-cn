---
title: 步骤 2-配置 WSUS
description: Windows Server Update Service (WSUS) 主题-配置 WSUS 是用于部署 WSUS 需要四个步骤的第二步
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae151c2a6f0f5ef72b263fbe7f1f0d26933452af
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811001"
---
# <a name="step-2-configure-wsus"></a>步骤 2：将 WSUS 配置

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在服务器上安装 WSUS 服务器角色后，你必须正确配置它。 以下清单汇总了有关你的 WSUS 服务器执行初始配置所涉及的步骤。

|任务|描述|
|----|--------|
|[2.1.配置网络连接](#21-configure-network-connections)|使用网络配置向导配置群集网络。|
|[2.2.使用 WSUS 配置向导配置 WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|使用 WSUS 配置向导执行基本 WSUS 配置。|
|[2.3.配置 WSUS 计算机组](#23-configure-wsus-computer-groups)|在 WSUS 管理控制台创建计算机组以管理组织中的更新。|
|[2.4.配置客户端更新](#24-configure-client-updates)|指定如何以及何时将自动更新应用于客户端计算机。|
|[2.5.使用安全套接字层协议保护 WSUS](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|配置安全套接字层 (SSL) 协议以帮助保护 Windows Server Update Services (WSUS)。|

## <a name="21-configure-network-connections"></a>2.1. 配置网络连接
开始配置过程之前，确保你知道以下问题的答案：

1.  是否配置服务器防火墙以便让客户端访问服务器？

2.  该计算机是否连接至上游服务器（例如专为从 Microsoft 更新下载更新而设计的服务器）？

3.  你是否拥有代理服务器的名称以及代理服务器 的用户凭据（如需要）？

默认情况下，配置 WSUS 以将 Microsoft 更新用作获取更新的位置。 如果网络上有代理服务器，可以配置 WSUS 以使用代理服务器。 如果 WSUS 和 Internet 之间的企业防火墙，你可能必须配置防火墙以确保 WSUS 可获得更新。

> [!TIP]
> 虽然需要 Internet 连接线从 Microsoft 更新下载更新，WSUS 让你你能够将更新导入到连接到 Internet 的网络。

当你知道这些问题的答案时，你可以开始配置以下 WSUS 网络设置：

-   **Updates** 指定该服务器获得更新的方式（从 Microsoft 更新或其他 WSUS 服务器）。

-   **代理**如果你确定 WSUS 需要使用代理服务器具有 Internet 访问权限，需要在 WSUS 服务器中配置代理设置。

-   **防火墙**你确定 WSUS 安装在企业防火墙后，是否必须在边缘设备，以便完全允许 WSUS 流量完成一些附加步骤。

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. 从 WSUS 服务器到 Internet 的连接
如果 WSUS 和 Internet 之间存有企业防火墙，你可能必须配置防火墙以确保 WSUS 可获得更新。 为了从 Microsoft 更新获取更新，WSUS 服务器将端口 443 用于 HTTPS 协议。 尽管大多数企业防火墙允许这种类型的流量，但有限制由于公司的安全策略的服务器从 Internet 访问某些公司。 如果你的公司限制访问，您需要获得授权才能访问 Internet 从 WSUS 到以下 Url 列表：

- http://windowsupdate.microsoft.com

- http://*.windowsupdate.microsoft.com

- https://*.windowsupdate.microsoft.com

- http://*.update.microsoft.com

- https://*.update.microsoft.com

- http://*.windowsupdate.com

- http://download.windowsupdate.com

- https://download.microsoft.com

- http://*.download.windowsupdate.com

- http://wustat.windows.com

- http://ntservicepack.microsoft.com

- http://go.microsoft.com

- http://dl.delivery.mp.microsoft.com

- https://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> 有关 WSUS 未能获得更新由于防火墙配置所在的方案，请参阅[文章 885819](https://support.microsoft.com/kb/885819) Microsoft 知识库中。

以下部分描述如何配置位于 WSUS 和 Internet 之间的企业防火墙。 由于 WSUS 启动所有网络流量，不需要在 WSUS 服务器上配置 Windows 防火墙。 虽然 Microsoft 更新和 WSUS 之间的连接需要打开端口 80 和 443，你可以配置多台 SUS 服务器以便与自定义端口同步。

### <a name="212-connection-between-wsus-servers"></a>2.1.2. WSUS 服务器之间的连接
WSUS 上游和下游服务器会在 WSUS 管理员配置的端口上同步。 默认情况下，这些端口配置如下：

-   在 WSUS 3.2 及更早版本中，端口 80 用于 HTTP，443 用于 HTTPS

-   在 WSUS 6.2 及更高版本 (至少 Windows Server 2012)，端口 8530 用于 HTTP 和 8531 使用 HTTPS

WSUS 服务器上的防火墙必须配置为允许这些端口上存在入站流量。

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. 客户端（Windows 更新代理）与 WSUS 服务器之间的连接
侦听接口和端口在 WSUS 的 IIS 站点以及用于配置客户端 PC 的任何组策略设置中进行配置。 默认端口与上一部分 **WSUS 服务器之间的连接**中指定的端口相同，WSUS 服务器上的防火墙也必须配置为允许这些端口上存在入站流量。

## <a name="configure-the-proxy-server"></a>配置代理服务器
如果公司网络使用代理服务器，则代理服务器必须支持 HTTP 和 SSL 协议，并使用基本身份验证或 Windows 身份验证。 可以使用以下配置之一来满足这些要求：

1.  支持两个协议通道的单台代理服务器。 在这种情况下，将一个通道设置为使用 HTTP，将另一个通道设置为使用 HTTPS。

    > [!NOTE]
    > 可以设置一个代理服务器，它在 WSUS 服务器软件安装过程中为 WSUS 处理这两种协议。

2.  两台代理服务器，各自支持一个协议。 在这种情况下，一台代理服务器配置为使用 HTTP，另一个代理服务器配置为使用 HTTPS。

若要设置两台代理服务器（各自为 WSUS 处理一种协议），请使用以下过程：

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>设置 WSUS 以使用两台代理服务器

1.  使用本地 Administrators 组的成员帐户登录要作为 WSUS 服务器的计算机。

2.  安装 WSUS 服务器角色的。 在 WSUS 配置向导（下一部分中讨论）过程中，未指定代理服务器。

3.  以管理员身份打开命令提示符 (Cmd.exe)。 若要以管理员身份打开命令提示符，请转到“开始”  。 在中**开始搜索**，类型**命令提示符下**。 在开始菜单的顶部，右键单击**命令提示符**，然后单击**以管理员身份运行**。 如果**用户帐户控制**出现对话框，请输入相应的凭据 （如果需要），确认它显示的操作是您想要，然后单击**继续**。

4.  在命令提示符窗口中，转到 C:\Program Files\Update Services\Tools 文件夹。 键入以下命令：

    **wsusutil ConfigureSSlproxy [< proxy_server proxy_port >]-启用**，其中：

    1.  proxy_server 是支持 HTTPS 的代理服务器的名称。

    2.  proxy_port 是代理服务器端口号。

5.  关闭命令提示符窗口。

若要向 WSUS 配置添加使用 HTTP 协议的代理服务器，请使用以下过程：

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>添加使用 HTTP 协议的代理服务器

1.  打开 WSUS 管理控制台。

2.  在左窗格中，展开服务器名称，然后单击“选项”  。

3.  在“选项”  窗格中，单击“更新源和更新服务器”  ，然后单击“代理服务器”  选项卡。

4.  使用以下选项可修改现有代理服务器配置：

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>对 WSUS 配置更改或添加代理服务器

    1.  选中“在同步时使用代理服务器”  的复选框。

    2.  在“代理服务器名称”  文本框中，输入代理服务器的名称。

    3.  在“代理端口号”  文本框中，输入代理服务器的端口号。 默认端口号为 80。

    4.  Ff 代理服务器要求使用特定用户帐户，选择**使用用户凭据来连接到代理服务器**复选框。 对应的文本框中键入所需的用户名称、 域和密码。

    5.  如果代理服务器支持基本身份验证，请选择“允许基本身份验证(以明文形式发送密码)”  复选框。

    6.  单击 **“确定”** 。

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>从 WSUS 配置中删除代理服务器

    1.  若要从 WSUS 配置中删除代理服务器，请清除“在同步时使用代理服务器”  的复选框。

    2.  单击 **“确定”** 。

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. 使用 WSUS 配置向导配置 WSUS。
该过程假设你使用首次启用 WSUS 管理控制台时出现的 WSUS 配置向导。 在本主题的后部分中，你将学习如何使用 **“选项”** 页执行这些配置。

#### <a name="to-configure-wsus"></a>配置 WSUS 的步骤

1.  在“服务器管理器”导航窗格中，单击 **“仪表板”** ，单击 **“工具”** ，然后单击 **“Windows Server Update Services”** 。

    > [!NOTE]
    > 如果**完成 WSUS 安装**出现对话框，请单击**运行**。 在中**完成 WSUS 安装**对话框中，单击**关闭**安装成功完成。

2.  Windows Server Update Services 向导会打开。 在“开始之前”  页上，查看信息，再单击“下一步”  。

3.  阅读 **“加入 Microsoft 更新改善计划”** 页上的说明，评估你是否想参与其中。 如果要参与该计划。 保留默认选择，或清除该复选框，然后依次**下一步**。

4.  在“选择‘上游服务器’”  页上，有两个选项：

    1.  与 Microsoft 更新同步更新

    2.  从其他 Windows Server Update Services 服务器中进行同步

        -   如果您选择从另一台 WSUS 服务器同步，，指定与上游服务器的服务器名称和此服务器将在其进行通信的端口。

        -   若要使用 SSL，请选中 **“同步更新信息时使用 SSL”** 复选框。 服务器将使用端口 443 进行同步。 （确保该服务器和上游服务器支持 SSL）。

        -   如果副本服务器，请选择**这是上游服务器副本**复选框。

5.  为你的部署选择适当选项后，单击 **“下一步”** 继续。

6.  在 **“指定代理服务器”** 页上，选中 **“同步时使用代理服务器”** 复选框，然后在对应的框中键入代理服务器名称和端口号（默认是端口 80）。

    > [!IMPORTANT]
    > 如果你确定 WSUS 需要代理服务器才能访问 Internet，则必须完成此步骤。

7.  如果你想要使用特定用户凭据，选择连接到代理服务器**使用用户凭据来连接到代理服务器**复选框，然后相应然后键入用户名、 域和用户的密码框。 如果你想要启用基本身份验证的用户连接到代理服务器，选择**允许基本身份验证 （密码以明文形式发送）** 复选框。

8.  单击“下一步”  。 上**连接到上游服务器**页上，单击**启动连接**。

9. 连接它时，然后单击 **“下一步”** 继续。

10. 上**选择语言**页上，可以选择 WSUS 将收到更新的所有语言或语言子集的语言的选项。 选择语言子集将节省磁盘空间，但这十分重要，若要选择的所有所需的此 WSUS 服务器的所有客户端语言。 如果您选择仅获得特定语言获取更新，请选择**下载这些语言的更新仅**，然后选择为其想要更新; 否则，保持默认选择的语言。

    > [!WARNING]
    > 如果你选择 **“仅下载这些语言的更新”** 选项，且该服务器具有与其连接的下游 WSUS 服务器，该选项将强制下游服务器也仅使用所选的语言。

11. 为你的部署选择适当语言后，单击 **“下一步”** 继续。

12. **“选择产品”** 页允许你指定希望更新的产品。 选择产品类别，如 Windows 或特定产品，例如 Windows Server 2008。 选择产品类别选择该类别中的所有产品。

13. 选择你的部署，适当的产品选项，然后单击**下一步**。

14. 在 **“选择类别”** 页上，选择要包含的更新类别。 选择所有类别或它们的子集，然后单击“下一步”  。

15. “设置同步计划”  页使你可以选择手动还是自动执行同步。

    -   如果愿意**手动同步**，您必须从 WSUS 管理控制台启动同步过程。

    -   如果愿意**自动同步**，WSUS 服务器将每隔一段时间执行同步。

    设置“第一次同步”  的时间，然后指定你希望该服务器执行的“每天同步次数”  数量。 例如，如果你指定每天同步四次，从上午 3:00 开始，则同步将在上午 3:00、上午 9:00、下午 3:00 和下午 9:00 发生。

16. 为你的部署选择适当的产品选项后，单击 **“下一步”** 继续。

17. 在 **“完成”** 页上，你可通过选择 **“开始初始同步”** 对话框，即时启动同步。 如果不选择此选项，您需要使用 WSUS 管理控制台来执行初始同步。 如果你希望阅读有关其他设置的详细信息，请单击 **“下一步”** ，或单击 **“完成”** 来结束该向导并完成初始 WSUS 设置。

18. 在单击 **“完成”** 后，WSUS 管理控制台会出现。

既然你已执行基本的 WSUS 配置，请阅读下一部分了解有关使用 WSUS 管理控制台来更改设置的详细信息。

## <a name="23-configure-wsus-computer-groups"></a>2.3. 配置 WSUS 计算机组
计算机组是 Windows Server Update Services (WSUS) 部署的重要组成部分。 计算机组允许你测试更新并将更新作为特定计算机的目标。 有两个默认计算机组：所有计算机和未分配的计算机。 默认情况下，当每台客户端计算机首次联系 WSUS 服务器时，服务器会将该客户端计算机添加到这两组。

你可以按需要创建自定义计算机组来管理组织的更新。 根据最佳做法，先至少创建一个计算机组来测试更新，然后再将它们部署给组织中的其他计算机。

使用以下过程创建新组并将计算机分配给该组：

#### <a name="to-create-a-computer-group"></a>创建计算机组的步骤

1.  在 WSUS 管理控制台中下**Update Services**，展开 WSUS 服务器，展开**计算机**，右键单击**的所有计算机**，然后单击**添加计算机组**。

2.  在中**添加计算机组**对话框中**名称**，指定新组的名称，然后单击**添加**。

3.  单击**计算机**，然后选择你想要分配给该新组的计算机。

4.  右键单击在上一步骤中，选择的计算机名称，然后单击**成员身份更改**。

5.  在中**设置计算机组成员身份**对话框中，选择测试组所创建的然后单击**确定**。

## <a name="24-configure-client-updates"></a>2.4. 配置客户端更新
WSUS 设置自动配置 IIS 以将最新版本的自动更新分配给每台连接 WSUS 服务器的客户端计算机。 配置自动更新的最佳方式取决于网络环境。

-   在环境中使用 active directory 目录服务，可以使用现有的基于域的组策略对象 (GPO)，或创建一个新的 GPO。

-   在没有 active directory 环境中，使用本地组策略编辑器配置自动更新，然后将客户端计算机指向 WSUS 服务器。

> [!IMPORTANT]
> 下面的过程假定你的网络运行 active directory。 这些过程也假设你熟悉组策略且使用它来管理网络。

使用以下过程向客户端计算机配置自动更新。

-   [步骤 4：为自动更新配置组策略设置](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3.配置计算机组](#23-configure-wsus-computer-groups)本主题中

### <a name="configure-automatic-updates-in-group-policy"></a>在组策略中配置自动更新

如果已在网络中设置 active directory，可以通过它们包含在组策略对象 (GPO)，然后使用 WSUS 设置中配置该 GPO 同时配置一个或多个计算机。 我们建议你创建一个仅含有 WSUS 设置的新 GPO。

此 WSUS GPO 链接到适用于你的环境的 active directory 容器。 在简单的环境中，你可能将单一 WSUS GPO 连接到域。 在较为复杂的环境中，你可能会将多个 WSUS GPO 连接到几个组织单位 (OU)，从而将不同的 WSUS 策略设置应用到不同类型的计算机。

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>通过域启用 WSUS 的步骤

1.  在组策略管理控制台 (GPMC)，浏览到你想要配置 WSUS 的 GPO，然后单击**编辑**。

2.  在 GPMC 中，展开**计算机配置**，展开**策略**，展开**管理模板**，展开**Windows 组件**，然后依次**Windows Update**。

3.  在详细信息窗格中，双击 **“配置自动更新”** 。 “配置自动更新”  策略会打开。

4.  单击“已启用”  ，然后选择“配置自动更新”  设置下的以下选项之一：

    -   **下载通知和安装通知**。 该选项会在你下载和安装更新之前通知登录的管理用户。

    -   **自动下载和通知安装**。 该选项将自动开始下载更新，然后在安装更新之前通知登录的管理用户。 默认情况下选择此选项。

    -   **自动下载和计划安装**。 该选项自动开始下载更新，然后在你指定的当天和时间安装更新。

    -   **允许本地管理员选择设置**。 该选项可让本地管理员使用控制面板中的自动更新来选择配置选项。 例如，他们可以选择计划的安装时间。 本地管理员不能仅用自动更新。

5.  选择**允许客户端的目标**，选择**已启用**，然后键入你想要添加此计算机的 WSUS 计算机组的名称**此计算机的目标组名称**框。

    > [!NOTE]
    > “允许客户端目标设置”  使客户端计算机可以在自动更新重定向到 WSUS 服务器时将自己添加到 WSUS 服务器上的目标计算机组。 如果状态设置为已启用，此计算机将自己标识为特定计算机组的成员时将信息发送到 WSUS 服务器，使用它来确定哪些更新部署到此计算机。 此设置会向 WSUS 服务器指示客户端计算机使用的组。 必须在 WSUS 服务器上创建组，并将域成员计算机添加到该组。

6.  单击“确定”  关闭“允许客户端目标设置”  策略并返回 Windows 更新详细信息窗格。

7.  单击“确定”  关闭“配置自动更新”  策略并返回 Windows 更新详细信息窗格。

8.  在 **Windows Update** 详细信息窗格中，双击 **“指定 Intranet Microsoft 更新服务位置”** 。

9. 单击“已启用”  ，然后在“设置检测更新的 Intranet 更新服务”  框和“设置 Intranet 统计服务器”  框中，输入 WSUS 服务器的相同 URL。 例如，键入 *http://servername* 这两个框中 (其中*servername*是 WSUS 服务器的名称)。

    > [!WARNING]
    > 当你键入 WSUS 服务器的 Intranet 地址时，确保指定准备使用哪个端口。 默认情况下，WSUS 使用适用于 HTTP 的端口 8530 以及适用于 HTTPS 的端口 8531。 例如，如果使用 HTTP，则应键入 **http://servername:8530** 。

10. 单击 **“确定”** 。

将客户端计算机设置后，将需要几分钟才会在该计算机将出现**计算机**WSUS 管理控制台中的页。 对于配有基于域的组策略对象的客户端计算机，组策略将花费大约 20 分钟才能将新的策略设置应用于客户端计算机。 默认情况下，在后台每隔 90 分钟，0 到 30 分钟的随机偏移量与组策略更新。 如果你想要更快地更新组策略，可以打开客户端计算机并键入 gpupdate /force 上的命令提示符窗口。

对于使用本地组策略编辑器配置的客户端计算机，该 GPO 将立即应用，并更新大约需要 20 分钟。 如果你开始手动检测，，无需等待 20 分钟，让客户端计算机联系 WSUS。

因为等待检测开始是费时的过程，所以你可以使用以下过程立即启动检测。

##### <a name="to-initiate-wsus-detection"></a>启动 WSUS 检测的步骤

1.  在客户端计算机上使用提升的权限打开命令提示符窗口。

2.  键入 wuauclt.exe /detectnow，然后按 ENTER。

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. 使用安全套接字层协议保护 WSUS

可以使用安全套接字层 (SSL) 协议来帮助保护 WSUS 部署。 WSUS 使用 SSL 向 WSUS 服务器验证客户端计算机和下游 WSUS 服务器的身份。 WSUS 还使用 SSL 加密更新元数据。

> [!IMPORTANT]
> 配置为使用传输层安全性 (TLS) 或 HTTPS 的客户端和下游服务器还必须将配置为将完全限定的域名 (FQDN) 用于其上游 WSUS 服务器。

WSUS 仅将 SSL 用于元数据，而不用于更新文件。 这与 Microsoft 更新分发更新的方式相同。 Microsoft 通过对每个更新进行签名来降低在未加密通道上发送更新文件的风险。 此外，还会为每个更新计算哈希并随元数据一起发送。 下载更新时，WSUS 会检查数字签名和哈希。 如果更新已更改，未安装。

### <a name="limitations-of-wsus-ssl-deployments"></a>WSUS SSL 部署的限制
使用 SSL 保护 WSUS 部署时，必须考虑以下限制：

1.  使用 SSL 会增加服务器工作负荷。 应预计到 10% 的性能损失，因为对通过网络发送的所有元数据进行加密会产生成本。

2.  如果使用远程 SQL Server 数据库使用 WSUS，WSUS 服务器和数据库服务器之间的连接不受 SSL 保护。 如果必须保护数据库连接，请考虑以下建议：

-   将 WSUS 数据库移动到 WSUS 服务器。

-   将远程数据库服务器和 WSUS 服务器移动到专用网络。

-   部署 Internet 协议安全性 (IPsec) 以帮助保护网络流量。 有关 IPsec 的详细信息，请参阅 [创建和使用 IPsec 策略](https://go.microsoft.com/fwlink/?LinkID=203841)。

### <a name="configure-ssl-on-the-wsus-server"></a>在 WSUS 服务器上配置 SSL
WSUS 需要将两个端口用于 SSL：一个端口使用 HTTPS 发送加密的元数据，另一个端口使用 HTTP 发送更新。 将 WSUS 配置为使用 SSL 时，请考虑下列事项：

-   不能将整个 WSUS 网站配置为需要 SSL，因为必须对发送到 WSUS 网站的所有流量进行加密。 WSUS 仅对更新元数据进行加密。 如果在计算机尝试检索更新文件上的 HTTPS 端口，则传输将失败。

    只应要求将 SSL 用于以下虚拟根：

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    不应要求将 SSL 用于以下虚拟根：

    -   **Content**

    -   **清单**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   证书颁发机构 (CA) 的证书必须导入本地计算机受信任的根 CA 存储中，或下游 WSUS 服务器上的 Windows Server Update Service 受信任的根 CA 存储中。 如果证书仅导入到本地用户受信任的根 CA 存储，则下游 WSUS 服务器将不进行身份验证在上游服务器上。

    有关如何在 IIS 中使用的 SSL 证书的详细信息，请参阅[需要安全的套接字层 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846)。

-   必须将证书导入与 WSUS 服务器通信的所有计算机。 这包括所有客户端计算机、下游服务器和运行 WSUS 管理控制台的计算机。 证书应导入本地计算机受信任的根 CA 存储中，或 Windows Server Update Service 受信任的根 CA 存储中。

-   可以将任何端口用于 SSL。 但是，为 SSL 设置的端口还确定 WSUS 用于发送明文 HTTP 流量的端口。 请考虑以下示例：

    -   如果您使用行业标准端口 443 用于 HTTPS 通信，WSUS 使用的行业标准端口 80 用于明文 HTTP 流量。

    -   如果用于 HTTPS 通信使用任何非 443 的端口，WSUS 将数字位于的端口之前针对 HTTPS 端口上发送明文 HTTP 流量。 例如，如果将端口 8531 用于 HTTPS，则 WSUS 会将端口 8530 用于 HTTP。

-   如果服务器名称、SSL 配置或端口号更改，则必须重新初始化 *ClientServicingProxy* 。

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>在 WSUS 根服务器上配置 SSL

1.  使用 WSUS Administrators 组或本地 Administrators 组的成员帐户登录 WSUS 服务器。

2.  转到**启动**，类型**CMD**，右键单击**命令提示符下**，然后单击**以管理员身份运行**。

3.  导航到 *%ProgramFiles%***\Update Services\Tools\\* * 文件夹。

4.  在命令提示符窗口中，键入以下命令：

    **Wsusutil configuressl***certificateName*

    其中：

    *certificateName* 是 WSUS 服务器的 DNS 名称。

### <a name="configure-ssl-on-client-computers"></a>在客户端计算机上配置 SSL
在客户端计算机上配置 SSL 时，应考虑以下问题：

-   必须包含用于 WSUS 服务器上的安全端口的 URL。 因为不能在服务器上要求 SSL，所以确保客户端计算机可以使用安全通道的唯一方法是使用指定 HTTPS 的 URL。 如果任何非 443 的端口用于 SSL，则必须也在 URL 中包含该端口。

-   客户端计算机上的证书必须导入到本地计算机受信任的根 CA 存储区或自动更新服务受信任的根 CA 存储中。 如果证书导入到本地用户的受信任的根 CA 存储区，自动更新将服务器身份验证失败。

-   客户端计算机必须信任绑定到 WSUS 服务器的证书。 根据使用的证书类型，可能必须设置服务以使客户端计算机可以信任绑定到 WSUS 服务器的证书。

### <a name="configure-ssl-for-downstream-wsus-servers"></a>为下游 WSUS 服务器配置 SSL
以下说明配置下游服务器以同步到使用 SSL 的上游服务器。

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>将下游服务器同步到使用 SSL 的上游服务器

1.  使用本地 Administrators 组或 WSUS Administrators 组的成员用户帐户登录计算机。

2.  单击**启动**，单击**所有程序**，单击**管理工具**，然后单击**Windows Server Update Service**。

3.  在右窗格中，展开服务器名称。

4.  单击“选项”  ，然后单击“更新源和代理服务器”  。

5.  在“更新源”  页上，选择“从其他 Windows Server Update Services 服务器中进行同步”  。

6.  键入到上游服务器的名称**服务器名称**文本框。 键入服务器用于 SSL 连接到的端口号**端口号**文本框。

7.  选择**同步更新信息时使用 SSL**复选框，然后依次**确定**。

### <a name="additional-ssl-resources"></a>其他 SSL 资源
设置证书颁发机构所需的步骤是将证书绑定到 WSUS 网站并在客户端计算机之间建立信任，证书不在本指南的讨论范围之内。 有关详细信息以及有关如何安装证书和设置此环境的说明，请参阅以下主题：

-   [Suite B PKI 分步指南](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [实现和管理证书模板](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [active directory 证书服务升级和迁移指南](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [配置证书自动注册](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. 完成 IIS 配置
默认情况下，会为默认和所有新的 IIS 网站启用匿名读取访问。 某些应用程序（特别是 Windows SharePoint Services）可能会删除匿名访问。 如果发生这种情况，必须重新启用匿名读取访问权限，然后才能成功安装和运行 WSUS。

若要启用匿名读取访问，请安装适用版本的 IIS 的步骤操作：

1.  [启用匿名身份验证 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316)（在《IIS 7 操作指南 》中进行了介绍）。

2.  [启用匿名身份验证 (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391)（在《IIS 6.0 操作指南 》中进行了介绍）。

### <a name="27-configure-a-signing-certificate"></a>2.7. 配置签名证书
WSUS 能够发布自定义更新程序包来更新 Microsoft 和非 Microsoft 产品。 WSUS 可以使用验证码证书自动为你对这些自定义更新程序包进行签名。 若要启用自定义更新签名，必须在 WSUS 服务器上安装程序包签名证书。 有几个与自定义更新签名关联的注意事项。

1.  **证书分发**。 必须在 WSUS 服务器上安装私钥，并且必须在要接收自定义签名更新的所有客户端 PC 和服务器上受信任的证书存储中显式安装公钥。

2.  **过期**。 当自签名证书过期或接近过期时，WSUS 会在事件日志中记录事件。

3.  **证书更新/吊销**。 如果你想要更新或吊销证书 （即后发现它已过期），则 WSUS 提供启用此选项的功能。 实现此操作转变为手动任务，非常难以手动执行或成功地自动执行。


