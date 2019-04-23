---
title: 安装和配置 NPS 服务器
description: 验证 VPN 服务器发送连接请求的 NPS 服务器处理的用户有权连接用户的标识，并记录在 NPS 中配置 RADIUS 记帐时选择该连接请求的方面。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890308"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>步骤 4： 安装和配置网络策略服务器 (NPS)

>   适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


&#171;  [**下一步：** 步骤 3：配置的远程访问服务器的 Always On VPN](vpn-deploy-ras.md)<br>
&#187;  [**下一步：** 步骤 5：配置 DNS 和防火墙设置](vpn-deploy-dns-firewall.md)


在此步骤中，以便 VPN 服务器发送连接请求进行处理安装网络策略服务器 (NPS):

- 执行以验证用户有权连接的授权。
- 执行身份验证来验证用户的标识。
- 执行记帐记录在 NPS 中配置 RADIUS 记帐时选择该连接请求的方面。

在本部分中步骤，可以通过完成以下操作：

1.  在计算机或 VM 计划为 NPS 服务器并安装在您的组织或公司网络上，你可以安装 NPS。

   >[!TIP] 
   >如果你的网络上已有一个或多个 NPS 服务器，不需要执行 NPS 服务器安装-相反，可以使用本主题来更新现有的 NPS 服务器的配置。

>[!NOTE]  
无法在 Windows Server Core 上安装网络策略服务器服务。

2.  在企业组织/NPS 服务器上，可以配置 NPS 充当 RADIUS 服务器处理接收到来自 VPN 服务器的连接请求。

## <a name="install-network-policy-server"></a>安装网络策略服务器

在此过程中，通过使用 Windows PowerShell 或服务器管理器添加角色和功能向导安装 NPS。 NPS 是网络策略和访问服务服务器角色的一个角色服务。

>[!TIP] 
>默认情况下，NPS 在所有已安装的网络适配器上侦听端口 1812、1813、1645 和 1646 上的 RADIUS 流量。 在安装 NPS，启用具有高级安全性的 Windows 防火墙，为这些端口的防火墙例外获取自动创建用于 IPv4 和 IPv6 通信。 如果您的网络访问服务器配置为这些默认值以外的端口上发送 RADIUS 流量，删除 NPS 安装期间在具有高级安全性的 Windows 防火墙中创建的例外和您使用的端口创建例外RADIUS 流量。

**适用于 Windows PowerShell 的过程：**

若要使用 Windows PowerShell，以管理员身份，运行 Windows PowerShell 执行此过程键入以下命令，然后按 ENTER。

`Install-WindowsFeature NPAS -IncludeManagementTools`

**过程为服务器管理器：**

1.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。 <p>将打开“添加角色和功能向导”。

2.  在开始之前，单击**下一步**。

    >[!NOTE] 
    >**开始之前**如果你以前已选择不显示添加角色和功能向导的页**默认情况下跳过此页**添加角色和功能向导的运行时。

1.  在选择安装类型，请确保**基于角色或基于功能的安装**已选中，然后单击**下一步**。

2.  在选择目标服务器，请确保**从服务器池中选择服务器**处于选中状态。

3.  在服务器池中，确保选择本地计算机，然后单击**下一步**。

4.  在中选择服务器角色**角色**，选择**网络策略和 Access Services**。 会打开一个对话框，询问是否将其添加所需的网络策略和访问服务功能。

5.  单击**添加功能**，然后单击**下一步**

6.  在选择功能，单击**下一步**，并在网络策略和访问服务中，查看的信息，然后单击**下一步**。

7.  在选择角色服务中，单击**网络策略服务器**。

8.  所需的网络策略服务器的功能，请单击**添加功能**，然后单击**下一步**。

9.  在确认安装选择中单击**如果需要自动重新启动目标服务器**。

10. 单击**是**以确认所选，然后单击**安装**。 <p>在安装过程中，安装进度页显示的状态。 当进程完毕时，消息"上的安装已成功*ComputerName*"会显示，其中*ComputerName*是在其安装网络策略服务器的计算机的名称。

11. 单击 **“关闭”**。

## <a name="configure-nps"></a>配置 NPS

安装 NPS，以处理所有身份验证、 授权，将 NPS 配置和连接的记帐职责请求后将收到来自 VPN 服务器。

### <a name="register-the-nps-server-in-active-directory"></a>在 Active Directory 中注册 NPS 服务器

在此过程中，您注册 Active Directory 中的服务器，以便它有权访问处理连接请求时的用户帐户信息。

**过程：**

1.  在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。

2.  在 NPS 控制台中，右键单击**NPS （本地）**，然后单击**在 Active Directory 中的注册服务器**以将其选中。<p>此时将打开网络策略服务器对话框。

3.  在网络策略服务器对话框中，单击**确定**两次。

有关注册 NPS 的备用方法，请参阅[在 Active Directory 域中注册 NPS 服务器](../../../../../networking/technologies/nps/nps-manage-register.md)。

### <a name="configure-network-policy-server-accounting"></a>配置网络策略服务器记帐

在此过程中，配置网络策略服务器记帐使用以下日志记录类型之一：

-   **事件日志记录**。 主要用于审核和故障排除的连接尝试。 你可以配置 NPS 事件日志记录通过获取在 NPS 控制台中的 NPS 服务器属性。

-   **用户身份验证和记帐请求记录到本地文件**。 主要用于连接分析和计费目的。 此外用作一种安全调查工具，因为它提供了一种攻击之后跟踪恶意用户的活动的方法。 你可以配置本地文件日志记录使用记帐配置向导。

-   **用户身份验证和记帐请求记录到 Microsoft SQL Server 符合 XML 数据库**。 用于允许多个服务器运行 NPS 拥有一个数据源。 此外提供了使用关系数据库的优点。 可以通过使用记帐配置向导配置 SQL Server 日志记录。

若要配置网络策略服务器记帐，请参阅[配置网络策略服务器记帐](../../../../../networking/technologies/nps/nps-accounting-configure.md)。

### <a name="add-the-vpn-server-as-a-radius-client"></a>添加 VPN 服务器作为 RADIUS 客户端

在中[为 Always On VPN 配置远程访问服务器](vpn-deploy-ras.md)部分中，安装并配置你的 VPN 服务器。 VPN 服务器在配置期间，VPN 服务器上添加 RADIUS 共享的机密。 

在此过程中，使用相同的共享机密文本字符串将 VPN 服务器配置为 NPS 中的 RADIUS 客户端。 使用相同的文本字符串，在 VPN 服务器上使用或 NPS 服务器和 VPN 服务器之间的通信失败。

>[!IMPORTANT] 
>向网络添加新的网络访问服务器 （VPN 服务器、 无线访问点、 身份验证切换或拨号服务器），必须先将服务器添加在 NPS 中的 RADIUS 客户端中，以便 NPS 已意识到，并且可以与网络访问服务器进行通信。

**过程：**

1.  在 NPS 服务器上，在 NPS 控制台中，双击**RADIUS 客户端和服务器**。

2.  右键单击**RADIUS 客户端**然后单击**新建**。 此时将打开新建 RADIUS 客户端对话框。

3.  确认**启用此 RADIUS 客户端**复选框处于选中状态。

4.  在中**友好名称**，输入 VPN 服务器的显示名称。

5.  在中**地址 （IP 或 DNS）**，输入 NAS IP 地址或 FQDN。<p>如果输入的 FQDN，请单击**验证**如果想要验证名称是否正确以及映射到有效的 IP 地址。

6.  在中**共享机密**，执行操作：

    1.  絋粄**手动**处于选中状态。

    2.  输入您还在 VPN 服务器输入的强文本字符串。

    3.  重新键入共享的机密中确认共享机密。

7.  单击 **“确定”**。 在 NPS 服务器上配置的 RADIUS 客户端的列表中显示的 VPN 服务器。

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>将 NPS 配置为 RADIUS 的 VPN 连接

在此过程中，通过以下方法将 NPS 配置为 RADIUS 服务器在你的组织网络上。 Nps，必须定义一个策略，仅允许用户在特定组中通过 VPN 服务器的访问组织/企业网络，然后仅 PEAP 身份验证请求中使用有效的用户证书时。

**过程：**

1.  在 NPS 控制台中，在标准配置中，确保**用于拨号或 VPN 连接的 RADIUS 服务器**处于选中状态。

2.  单击**配置 VPN 或拨号**。<p>配置 VPN 或拨号向导随即打开。

3.  单击**虚拟专用网络 (VPN) 连接**，然后单击**下一步**。

4.  指定拨号或 VPN 服务器，在 RADIUS 客户端中选择上一步中添加的 VPN 服务器的名称。<p>例如，如果你的 VPN 服务器 NetBIOS 名称 RAS1，请单击**RAS1**。

5.  单击“下一步” 。

6.  在配置身份验证方法，完成以下步骤：

    1.  清除**Microsoft 加密身份验证版本 2 (Ms-chapv2)** 复选框。

    2.  单击**可扩展身份验证协议**复选框以将其选中。

    3.  在类型 （基于访问权限和网络配置的方法） 中，单击**Microsoft:受保护的 EAP (PEAP)**，然后单击**配置**。<p>此时将打开编辑受保护的 EAP 属性对话框。

    4.  单击**删除**若要删除的安全密码 (EAP-MSCHAP v2) EAP 类型。

    5.  单击**添加**。 此时将打开添加 EAP 对话框。

    6.  单击**智能卡或其他证书**，然后单击**确定**。

    7.  单击**确定**关闭编辑受保护的 EAP 属性。

7.  单击“下一步” 。

8.  在指定用户组，请完成以下步骤：

    1.  单击**添加**。 此时将打开选择用户、 计算机、 服务帐户或组对话框。

    2.  类型**VPN 用户**然后单击**确定**。

    3.  单击“下一步” 。

9.  在指定 IP 筛选器中，单击**下一步**。

10. 在指定加密设置中，单击**下一步**。 不进行任何更改。<p>这些设置仅适用于 Microsoft 点对点加密 (MPPE) 连接，这种情况下不支持。

11. 在指定领域名称，单击**下一步**。

12. 单击**完成**以关闭向导。

## <a name="autoenroll-the-nps-server-certificate"></a>自动注册 NPS 服务器证书

在此过程中，则组策略在本地 NPS 服务器上手动刷新。 当组策略刷新，如果配置证书自动注册并正常运行，本地计算机是自动注册的证书由证书颁发机构 (CA)。

>[!NOTE]  
>当重新启动域成员计算机，或者当用户登录到域成员计算机自动刷新组策略。 此外，组策略定期刷新。 默认情况下，此定期刷新，发生在具有最多 30 分钟的随机偏移量每 90 分钟。

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

**过程：**

1.  在 NPS 上打开 Windows PowerShell。

2.  在 Windows PowerShell 提示符处，键入**gpupdate**，然后按 ENTER。

## <a name="next-step"></a>下一步
[第 5 步。配置 Always On VPN 的 DNS 和防火墙设置](vpn-deploy-dns-firewall.md):在此步骤中，通过使用 Windows PowerShell 或服务器管理器添加角色和功能向导安装网络策略服务器 (NPS)。 你还可以配置 NPS 以处理所有身份验证、 授权和记帐职责连接它接收来自 VPN 服务器的请求。



---

