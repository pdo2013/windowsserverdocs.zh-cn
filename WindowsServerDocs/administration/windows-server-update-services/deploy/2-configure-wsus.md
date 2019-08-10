---
title: 步骤 2-配置 WSUS
description: Windows Server Update Service (WSUS) 主题-配置 WSUS 是部署 WSUS 的四个步骤中的第二步
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5c4ac470d1187aa6186f6f05cab3df185a642fd
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914562"
---
# <a name="step-2-configure-wsus"></a>步骤 2：配置 WSUS

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

在服务器上安装 WSUS 服务器角色后，你必须正确配置它。 以下清单汇总了执行 WSUS 服务器的初始配置所涉及的步骤。

|任务|描述|
|----|--------|
|[2.1。配置网络连接](#21-configure-network-connections)|使用网络配置向导配置群集网络。|
|[2.2。使用 WSUS 配置向导配置 WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|使用 WSUS 配置向导执行基本 WSUS 配置。|
|[2.3。配置 WSUS 计算机组](#23-configure-wsus-computer-groups)|在 WSUS 管理控制台创建计算机组以管理组织中的更新。|
|[2.4。配置客户端更新](#24-configure-client-updates)|指定如何以及何时将自动更新应用于客户端计算机。|
|[2.5。采用安全套接字层协议保护 WSUS](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|配置安全套接字层 (SSL) 协议以帮助保护 Windows Server Update Services (WSUS)。|

## <a name="21-configure-network-connections"></a>2.1. 配置网络连接
开始配置过程之前，确保你知道以下问题的答案：

1.  是否配置服务器防火墙以便让客户端访问服务器？

2.  该计算机是否连接至上游服务器（例如专为从 Microsoft 更新下载更新而设计的服务器）？

3.  你是否拥有代理服务器的名称以及代理服务器 的用户凭据（如需要）？

默认情况下，配置 WSUS 以将 Microsoft 更新用作获取更新的位置。 如果在网络上有代理服务器, 则可以将 WSUS 配置为使用代理服务器。 如果 WSUS 与 Internet 之间存在企业防火墙, 你可能必须配置防火墙以确保 WSUS 能够获取更新。

> [!TIP]
> 虽然需要 Internet 连接线从 Microsoft 更新下载更新，WSUS 让你你能够将更新导入到连接到 Internet 的网络。

当你知道这些问题的答案时，你可以开始配置以下 WSUS 网络设置：

-   **Updates** 指定该服务器获得更新的方式（从 Microsoft 更新或其他 WSUS 服务器）。

-   **Proxy**如果你确定 wsus 需要使用代理服务器才能访问 Internet, 则需要在 WSUS 服务器中配置代理设置。

-   **防火墙**如果你发现 WSUS 位于企业防火墙后面, 则必须在边缘设备上执行其他一些步骤才能正确地允许 WSUS 流量。

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. 从 WSUS 服务器到 Internet 的连接
如果 WSUS 和 Internet 之间存有企业防火墙，你可能必须配置防火墙以确保 WSUS 可获得更新。 为了从 Microsoft 更新获取更新，WSUS 服务器将端口 443 用于 HTTPS 协议。 虽然大多数企业防火墙允许此类流量, 但由于公司的安全策略, 某些公司会限制服务器对 Internet 的访问。 如果你的公司限制访问, 你需要获得授权才能从 WSUS 访问以下 Url 列表:

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
> 对于由于防火墙配置而导致 WSUS 未能获得更新的方案, 请参阅 Microsoft 知识库中的[文章 885819](https://support.microsoft.com/kb/885819) 。

以下部分描述如何配置位于 WSUS 和 Internet 之间的企业防火墙。 由于 WSUS 会启动所有网络流量, 因此不需要在 WSUS 服务器上配置 Windows 防火墙。 虽然 Microsoft 更新和 WSUS 之间的连接需要打开端口 80 和 443，你可以配置多台 SUS 服务器以便与自定义端口同步。

### <a name="212-connection-between-wsus-servers"></a>2.1.2. WSUS 服务器之间的连接
WSUS 上游和下游服务器会在 WSUS 管理员配置的端口上同步。 默认情况下，这些端口配置如下：

-   在 WSUS 3.2 及更早版本中，端口 80 用于 HTTP，443 用于 HTTPS

-   在 WSUS 6.2 和更高版本 (至少为 Windows Server 2012) 上, 使用 HTTP 的端口8530和用于 HTTPS 的8531

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

3.  以管理员身份打开命令提示符 (Cmd.exe)。 若要以管理员身份打开命令提示符，请转到“开始”。 在 "**开始搜索**" 中, 键入**命令提示符**。 在 "开始" 菜单的顶部, 右键单击 "**命令提示符**", 然后单击 "以**管理员身份运行**"。 如果出现 "**用户帐户控制**" 对话框, 请输入适当的凭据 (如果已请求), 确认它显示的是所需操作, 然后单击 "**继续**"。

4.  在命令提示符窗口中, 请参阅 C:\Program Files\Update Services\Tools 文件夹。 键入以下命令:

    **Wsusutil.exe ConfigureSSlproxy [< proxy_server proxy_port >]-enable**, 其中:

    1.  proxy_server 是支持 HTTPS 的代理服务器的名称。

    2.  proxy_port 是代理服务器端口号。

5.  关闭命令提示符窗口。

若要向 WSUS 配置添加使用 HTTP 协议的代理服务器，请使用以下过程：

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>添加使用 HTTP 协议的代理服务器

1.  打开 WSUS 管理控制台。

2.  在左窗格中，展开服务器名称，然后单击“选项”。

3.  在“选项”窗格中，单击“更新源和更新服务器”，然后单击“代理服务器”选项卡。

4.  使用以下选项可修改现有代理服务器配置：

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>对 WSUS 配置更改或添加代理服务器

    1.  选中“在同步时使用代理服务器”的复选框。

    2.  在“代理服务器名称”文本框中，输入代理服务器的名称。

    3.  在“代理端口号” 文本框中，输入代理服务器的端口号。 默认端口号为 80。

    4.  Ff 代理服务器要求使用特定用户帐户, 并选中 "**使用用户凭据连接到代理服务器**" 复选框。 在对应的文本框中键入所需的用户名、域和密码。

    5.  如果代理服务器支持基本身份验证，请选择“允许基本身份验证(以明文形式发送密码)”复选框。

    6.  单击 **“确定”** 。

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>从 WSUS 配置中删除代理服务器

    1.  若要从 WSUS 配置中删除代理服务器，请清除“在同步时使用代理服务器”的复选框。

    2.  单击 **“确定”** 。

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. 使用 WSUS 配置向导配置 WSUS。
该过程假设你使用首次启用 WSUS 管理控制台时出现的 WSUS 配置向导。 在本主题的后部分中，你将学习如何使用 **“选项”** 页执行这些配置。

#### <a name="to-configure-wsus"></a>配置 WSUS 的步骤

1.  在“服务器管理器”导航窗格中，单击 **“仪表板”** ，单击 **“工具”** ，然后单击 **“Windows Server Update Services”** 。

    > [!NOTE]
    > 如果出现 "**完整的 WSUS 安装**" 对话框, 请单击 "**运行**"。 当安装成功完成时, 在 "**完成 WSUS 安装**" 对话框中, 单击 "**关闭**"。

2.  Windows Server Update Services 向导会打开。 在“开始之前” 页上，查看信息，再单击“下一步”。

3.  阅读 **“加入 Microsoft 更新改善计划”** 页上的说明，评估你是否想参与其中。 如果要参与该计划。 保留默认选择, 或清除复选框, 然后单击 "**下一步**"。

4.  在“选择‘上游服务器’” 页上，有两个选项：

    1.  与 Microsoft 更新同步更新

    2.  从其他 Windows Server Update Services 服务器中进行同步

        -   如果选择从其他 WSUS 服务器同步, 请指定服务器名称以及此服务器将与上游服务器通信的端口。

        -   若要使用 SSL，请选中 **“同步更新信息时使用 SSL”** 复选框。 服务器将使用端口 443 进行同步。 （确保该服务器和上游服务器支持 SSL）。

        -   如果这是副本服务器, 请选中 "**这是上游服务器的副本**" 复选框。

5.  为你的部署选择适当选项后，单击 **“下一步”** 继续。

6.  在 **“指定代理服务器”** 页上，选中 **“同步时使用代理服务器”** 复选框，然后在对应的框中键入代理服务器名称和端口号（默认是端口 80）。

    > [!IMPORTANT]
    > 如果你确定 WSUS 需要代理服务器才能访问 Internet，则必须完成此步骤。

7.  如果要使用特定用户凭据连接到代理服务器, 请选中 "**使用用户凭据连接到代理服务器"** 复选框, 然后在对应的框中键入用户的用户名、域和密码。 如果要为连接到代理服务器的用户启用基本身份验证, 请选中 "**允许基本身份验证 (以明文形式发送密码)** " 复选框。

8.  单击“下一步”。 在 "**连接到上游服务器**" 页上, 单击 "**开始连接**"。

9. 连接它时，然后单击 **“下一步”** 继续。

10. 在 "**选择语言**" 页上, 你可以选择 WSUS 将从其接收更新的语言-所有语言或语言子集。 选择语言子集将节省磁盘空间, 但必须选择此 WSUS 服务器的所有客户端需要的所有语言。 如果选择仅获取特定语言的更新, 请选择 "**仅下载这些语言的更新**", 然后选择要更新的语言。否则, 保留默认选择。

    > [!WARNING]
    > 如果你选择 **“仅下载这些语言的更新”** 选项，且该服务器具有与其连接的下游 WSUS 服务器，该选项将强制下游服务器也仅使用所选的语言。

11. 为你的部署选择适当语言后，单击 **“下一步”** 继续。

12. **“选择产品”** 页允许你指定希望更新的产品。 选择产品类别 (如 Windows) 或特定产品, 如 Windows Server 2008。 选择产品类别将选择该类别中的所有产品。

13. 为你的部署选择适当的产品选项, 然后单击 "**下一步**"。

14. 在 **“选择类别”** 页上，选择要包含的更新类别。 选择所有类别或它们的子集，然后单击“下一步”。

15. “设置同步计划”页使你可以选择手动还是自动执行同步。

    -   如果选择 "**手动同步**", 则必须从 WSUS 管理控制台启动同步过程。

    -   如果选择 "**自动同步**", WSUS 服务器将按设置的时间间隔同步。

    设置“第一次同步”的时间，然后指定你希望该服务器执行的“每天同步次数” 数量。 例如，如果你指定每天同步四次，从上午 3:00 开始，则同步将在上午 3:00、上午 9:00、下午 3:00 和下午 9:00 发生。

16. 为你的部署选择适当的产品选项后，单击 **“下一步”** 继续。

17. 在 **“完成”** 页上，你可通过选择 **“开始初始同步”** 对话框，即时启动同步。 如果未选择此选项, 则需要使用 WSUS 管理控制台来执行初始同步。 如果你希望阅读有关其他设置的详细信息，请单击 **“下一步”** ，或单击 **“完成”** 来结束该向导并完成初始 WSUS 设置。

18. 在单击 **“完成”** 后，WSUS 管理控制台会出现。

既然你已执行基本的 WSUS 配置，请阅读下一部分了解有关使用 WSUS 管理控制台来更改设置的详细信息。

## <a name="23-configure-wsus-computer-groups"></a>2.3. 配置 WSUS 计算机组
计算机组是 Windows Server Update Services (WSUS) 部署的重要部分。 计算机组允许你测试更新并将更新作为特定计算机的目标。 有两个默认计算机组：所有计算机和未分配的计算机。 默认情况下，当每台客户端计算机首次联系 WSUS 服务器时，服务器会将该客户端计算机添加到这两组。

你可以按需要创建自定义计算机组来管理组织的更新。 根据最佳做法，先至少创建一个计算机组来测试更新，然后再将它们部署给组织中的其他计算机。

使用以下过程创建新组并将计算机分配给该组：

#### <a name="to-create-a-computer-group"></a>创建计算机组的步骤

1.  在 WSUS 管理控制台中, 在 "**更新服务**" 下, 展开 WSUS 服务器, 展开 "**计算机**", 右键单击 "**所有计算机**", 然后单击 "**添加计算机组**"。

2.  在 "**添加计算机组**" 对话框的 "**名称**" 中, 指定新组的名称, 然后单击 "**添加**"。

3.  单击 "**计算机**", 然后选择要分配给此新组的计算机。

4.  右键单击在上一步中选择的计算机名称, 然后单击 "**更改成员身份**"。

5.  在 "**设置计算机组成员身份**" 对话框中, 选择你创建的测试组, 然后单击 **"确定"** 。

## <a name="24-configure-client-updates"></a>2.4. 配置客户端更新
WSUS 设置自动配置 IIS 以将最新版本的自动更新分配给每台连接 WSUS 服务器的客户端计算机。 配置自动更新的最佳方式取决于网络环境。

-   在使用 active directory 目录服务的环境中, 你可以使用现有的基于域的组策略对象 (GPO) 或创建新的 GPO。

-   在无 active directory 的环境中, 使用本地组策略编辑器配置自动更新, 然后将客户端计算机指向 WSUS 服务器。

> [!IMPORTANT]
> 以下过程假设你的网络运行 active directory。 这些过程也假设你熟悉组策略且使用它来管理网络。

使用以下过程向客户端计算机配置自动更新。

-   [步骤 4：为自动更新配置组策略设置](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3。本主题中](#23-configure-wsus-computer-groups)的配置计算机组

### <a name="configure-automatic-updates-in-group-policy"></a>在组策略中配置自动更新

如果你已经在网络中设置了 active directory, 则可以通过在组策略对象 (GPO) 中包含它们来同时配置一台或多台计算机, 然后使用 WSUS 设置来配置该 GPO。 我们建议你创建一个仅含有 WSUS 设置的新 GPO。

将此 WSUS GPO 链接到适合你的环境的 active directory 容器。 在简单的环境中，你可能将单一 WSUS GPO 连接到域。 在较为复杂的环境中，你可能会将多个 WSUS GPO 连接到几个组织单位 (OU)，从而将不同的 WSUS 策略设置应用到不同类型的计算机。

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>通过域启用 WSUS 的步骤

1.  在组策略管理控制台 (GPMC) 中, 浏览到要在其上配置 WSUS 的 GPO, 然后单击 "**编辑**"。

2.  在 GPMC 中, 依次展开 "**计算机配置**"、"**策略**" 和 "**管理模板**", 展开 " **Windows 组件**", 然后单击 " **Windows 更新**"。

3.  在详细信息窗格中，双击 **“配置自动更新”** 。 “配置自动更新” 策略会打开。

4.  单击“已启用”，然后选择“配置自动更新”设置下的以下选项之一：

    -   **下载通知和安装通知**。 该选项会在你下载和安装更新之前通知登录的管理用户。

    -   **自动下载和通知安装**。 该选项将自动开始下载更新，然后在安装更新之前通知登录的管理用户。 默认情况下选择此选项。

    -   **自动下载和计划安装**。 该选项自动开始下载更新，然后在你指定的当天和时间安装更新。

    -   **允许本地管理员选择设置**。 该选项可让本地管理员使用控制面板中的自动更新来选择配置选项。 例如，他们可以选择计划的安装时间。 本地管理员不能仅用自动更新。

5.  选择 "**启用客户端目标设定**", 选择 "**已启用**", 然后在 "**此计算机的目标组名称**" 框中键入要向其添加此计算机的 WSUS 计算机组的名称。

    > [!NOTE]
    > “允许客户端目标设置”使客户端计算机可以在自动更新重定向到 WSUS 服务器时将自己添加到 WSUS 服务器上的目标计算机组。 如果状态设置为 "已启用", 则此计算机会在向 WSUS 服务器发送信息时将自己标识为特定计算机组的成员, 该计算机使用它来确定要向此计算机部署哪些更新。 此设置会向 WSUS 服务器指示客户端计算机使用的组。 必须在 WSUS 服务器上创建组，并将域成员计算机添加到该组。

6.  单击“确定”关闭“允许客户端目标设置”策略并返回 Windows 更新详细信息窗格。

7.  单击“确定” 关闭“配置自动更新” 策略并返回 Windows 更新详细信息窗格。

8.  在 **Windows Update** 详细信息窗格中，双击 **“指定 Intranet Microsoft 更新服务位置”** 。

9. 单击“已启用”，然后在“设置检测更新的 Intranet 更新服务”框和“设置 Intranet 统计服务器”框中，输入 WSUS 服务器的相同 URL。 例如, 在这 *http://servername* 两个框中键入 (其中*servername*是 WSUS 服务器的名称)。

    > [!WARNING]
    > 当你键入 WSUS 服务器的 Intranet 地址时，确保指定准备使用哪个端口。 默认情况下，WSUS 使用适用于 HTTP 的端口 8530 以及适用于 HTTPS 的端口 8531。 例如, 如果使用 HTTP, 则应键入 **http://servername:8530** 。

10. 单击 **“确定”** 。

设置客户端计算机后, 该计算机将在 WSUS 管理控制台中的 "**计算机**" 页上显示之前需要几分钟。 对于配有基于域的组策略对象的客户端计算机，组策略将花费大约 20 分钟才能将新的策略设置应用于客户端计算机。 默认情况下, 每90分钟组策略在后台进行更新, 随机偏移量为0-30 分钟。 如果要更快地更新组策略, 可以在客户端计算机上打开命令提示符窗口, 然后键入 gpupdate/force。

对于使用本地组策略编辑器配置的客户端计算机, GPO 将立即应用, 并且更新需要大约20分钟。 如果你手动开始检测, 则不必等待20分钟, 客户端计算机便可联系 WSUS。

因为等待检测开始是费时的过程，所以你可以使用以下过程立即启动检测。

##### <a name="to-initiate-wsus-detection"></a>启动 WSUS 检测的步骤

1.  在客户端计算机上, 使用提升的权限打开命令提示符窗口。

2.  键入 wuauclt.exe/detectnow, 然后按 ENTER。

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. 使用安全套接字层协议保护 WSUS

可以使用安全套接字层 (SSL) 协议来帮助保护 WSUS 部署。 WSUS 使用 SSL 向 WSUS 服务器验证客户端计算机和下游 WSUS 服务器的身份。 WSUS 还使用 SSL 加密更新元数据。

> [!IMPORTANT]
> 配置为使用传输层安全性 (TLS) 或 HTTPS 的客户端和下游服务器还必须将配置为将完全限定的域名 (FQDN) 用于其上游 WSUS 服务器。

WSUS 仅将 SSL 用于元数据，而不用于更新文件。 这与 Microsoft 更新分发更新的方式相同。 Microsoft 通过对每个更新进行签名来降低在未加密通道上发送更新文件的风险。 此外，还会为每个更新计算哈希并随元数据一起发送。 下载更新时，WSUS 会检查数字签名和哈希。 如果更新已更改，则不会安装它。

### <a name="limitations-of-wsus-ssl-deployments"></a>WSUS SSL 部署的限制
使用 SSL 保护 WSUS 部署时，必须考虑以下限制：

1.  使用 SSL 会增加服务器工作负荷。 应预计到 10% 的性能损失，因为对通过网络发送的所有元数据进行加密会产生成本。

2.  如果将 WSUS 与远程 SQL Server 数据库一起使用，则 WSUS 服务器与数据库服务器之间的连接不通过 SSL 进行保护。 如果必须保护数据库连接，请考虑以下建议：

-   将 WSUS 数据库移动到 WSUS 服务器。

-   将远程数据库服务器和 WSUS 服务器移动到专用网络。

-   部署 Internet 协议安全性 (IPsec) 以帮助保护网络流量。 有关 IPsec 的详细信息，请参阅 [创建和使用 IPsec 策略](https://go.microsoft.com/fwlink/?LinkID=203841)。

### <a name="configure-ssl-on-the-wsus-server"></a>在 WSUS 服务器上配置 SSL
WSUS 需要将两个端口用于 SSL：一个端口使用 HTTPS 发送加密的元数据，另一个端口使用 HTTP 发送更新。 将 WSUS 配置为使用 SSL 时，请考虑下列事项：

-   不能将整个 WSUS 网站配置为需要 SSL，因为必须对发送到 WSUS 网站的所有流量进行加密。 WSUS 仅对更新元数据进行加密。 如果计算机尝试在 HTTPS 端口上检索更新文件, 则传输将失败。

    只应要求将 SSL 用于以下虚拟根：

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    不应要求将 SSL 用于以下虚拟根：

    -   内容

    -   **清单**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   证书颁发机构 (CA) 的证书必须导入本地计算机受信任的根 CA 存储中，或下游 WSUS 服务器上的 Windows Server Update Service 受信任的根 CA 存储中。 如果证书仅导入到本地用户受信任的根 CA 存储, 则下游 WSUS 服务器不会在上游服务器上进行身份验证。

    有关如何在 IIS 中使用 SSL 证书的详细信息, 请参阅[要求安全套接字层 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846)。

-   必须将证书导入与 WSUS 服务器通信的所有计算机。 这包括所有客户端计算机、下游服务器和运行 WSUS 管理控制台的计算机。 证书应导入本地计算机受信任的根 CA 存储中，或 Windows Server Update Service 受信任的根 CA 存储中。

-   可以将任何端口用于 SSL。 但是，为 SSL 设置的端口还确定 WSUS 用于发送明文 HTTP 流量的端口。 请考虑以下示例：

    -   如果将行业标准端口443用于 HTTPS 流量, 则 WSUS 会将行业标准端口80用于明文 HTTP 流量。

    -   如果使用443以外的任何端口进行 HTTPS 流量, 则 WSUS 会在端口上发送明文 HTTP 流量, 该端口以数字形式出现在 HTTPS 端口之前。 例如，如果将端口 8531 用于 HTTPS，则 WSUS 会将端口 8530 用于 HTTP。

-   如果服务器名称、SSL 配置或端口号更改，则必须重新初始化 *ClientServicingProxy* 。

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>在 WSUS 根服务器上配置 SSL

1.  使用 WSUS Administrators 组或本地 Administrators 组的成员帐户登录 WSUS 服务器。

2.  单击 "**开始**", 键入**CMD**, 右键单击 "**命令提示符**", 然后单击 "以**管理员身份运行**"。

3.  导航到 _% ProgramFiles%_ **\\ \Update Services\Tools**文件夹。

4.  在 "命令提示符" 窗口中, 键入以下命令:

    **Wsusutil configuressl** _

    其中：

    *certificateName* 是 WSUS 服务器的 DNS 名称。

### <a name="configure-ssl-on-client-computers"></a>在客户端计算机上配置 SSL
在客户端计算机上配置 SSL 时，应考虑以下问题：

-   必须包含用于 WSUS 服务器上的安全端口的 URL。 因为不能在服务器上要求 SSL，所以确保客户端计算机可以使用安全通道的唯一方法是使用指定 HTTPS 的 URL。 如果使用443以外的任何端口作为 SSL, 则还必须在 URL 中包含该端口。

-   必须将客户端计算机上的证书导入到本地计算机受信任的根 CA 存储区或自动更新服务受信任的根 CA 存储中。 如果仅将证书导入到本地用户的受信任的根 CA 存储, 自动更新将无法通过服务器身份验证。

-   客户端计算机必须信任绑定到 WSUS 服务器的证书。 根据使用的证书类型，可能必须设置服务以使客户端计算机可以信任绑定到 WSUS 服务器的证书。

### <a name="configure-ssl-for-downstream-wsus-servers"></a>为下游 WSUS 服务器配置 SSL
以下说明配置下游服务器以同步到使用 SSL 的上游服务器。

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>将下游服务器同步到使用 SSL 的上游服务器

1.  使用本地 Administrators 组或 WSUS Administrators 组的成员用户帐户登录计算机。

2.  依次单击 "**开始**"、"**所有程序**"、"**管理工具**", 然后单击 " **Windows Server 更新服务**"。

3.  在右窗格中，展开服务器名称。

4.  单击“选项”，然后单击“更新源和代理服务器”。

5.  在“更新源” 页上，选择“从其他 Windows Server Update Services 服务器中进行同步”。

6.  在 "**服务器名称**" 文本框中键入上游服务器的名称。 在 "**端口号**" 文本框中键入服务器用于 SSL 连接的端口号。

7.  选中 "**同步更新信息时使用 SSL** " 复选框, 然后单击 **"确定"** 。

### <a name="additional-ssl-resources"></a>其他 SSL 资源
设置证书颁发机构所需的步骤是将证书绑定到 WSUS 网站并在客户端计算机之间建立信任，证书不在本指南的讨论范围之内。 有关详细信息以及有关如何安装证书和设置此环境的说明，请参阅以下主题：

-   [Suite B PKI 分步指南](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [实现和管理证书模板](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [active directory 证书服务升级和迁移指南](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [配置证书自动注册](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. 完成 IIS 配置
默认情况下，会为默认和所有新的 IIS 网站启用匿名读取访问。 某些应用程序（特别是 Windows SharePoint Services）可能会删除匿名访问。 如果发生此情况, 则必须重新启用匿名读取访问, 然后才能成功安装和操作 WSUS。

若要启用匿名读取访问，请安装适用版本的 IIS 的步骤操作：

1.  [启用匿名身份验证 (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316)（在《IIS 7 操作指南 》中进行了介绍）。

2.  [启用匿名身份验证 (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391)（在《IIS 6.0 操作指南 》中进行了介绍）。

### <a name="27-configure-a-signing-certificate"></a>2.7. 配置签名证书
WSUS 能够发布自定义更新程序包来更新 Microsoft 和非 Microsoft 产品。 WSUS 可以使用验证码证书自动为你对这些自定义更新程序包进行签名。 若要启用自定义更新签名，必须在 WSUS 服务器上安装程序包签名证书。 有几个与自定义更新签名关联的注意事项。

1.  **证书分发**。 必须在 WSUS 服务器上安装私钥，并且必须在要接收自定义签名更新的所有客户端 PC 和服务器上受信任的证书存储中显式安装公钥。

2.  **过期**。 当自签名证书过期或接近过期时，WSUS 会在事件日志中记录事件。

3.  **证书更新/吊销**。 如果要更新或吊销证书 (即发现它过期之后), WSUS 未提供任何功能来启用此功能。 实现此操作转变为手动任务，非常难以手动执行或成功地自动执行。


