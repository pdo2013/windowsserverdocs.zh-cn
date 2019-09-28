---
title: 安装和配置 NPS 服务器
description: NPS 服务器处理 VPN 服务器发送的连接请求将验证用户是否具有连接权限、用户标识，并记录在 NPS 中配置 RADIUS 记帐时选择的连接请求的各个方面。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 5cb0d342afec9c28259efb7a2e15666358f3cb5b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404259"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>步骤 4： 安装和配置网络策略服务器（NPS）

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows 10

- [**一个**步骤 3：为始终启用 VPN 配置远程访问服务器](vpn-deploy-ras.md)
- [**一个**步骤 5：配置 DNS 和防火墙设置](vpn-deploy-dns-firewall.md)

在此步骤中，你将安装网络策略服务器（NPS）来处理 VPN 服务器发送的连接请求：

- 执行授权以验证用户是否有权进行连接。
- 执行身份验证以验证用户的身份。
- 执行记帐，记录在 NPS 中配置 RADIUS 记帐时选择的连接请求的各个方面。

本部分中的步骤可让你完成以下各项：

1. 在规划 NPS 服务器的计算机或 VM 上，并安装在你的组织或公司网络上，你可以安装 NPS。

   >[!TIP]
   >如果你的网络上已有一个或多个 NPS 服务器，则无需执行 NPS 服务器安装-相反，你可以使用本主题来更新现有 NPS 服务器的配置。

> [!NOTE]
> 你无法在 Windows Server Core 上安装网络策略服务器服务。

2. 在组织/企业 NPS 服务器上，你可以将 NPS 配置为作为 RADIUS 服务器执行，用于处理从 VPN 服务器接收的连接请求。

## <a name="install-network-policy-server"></a>安装网络策略服务器

在此过程中，您将使用 Windows PowerShell 或服务器管理器添加角色和功能向导 "安装 NPS。 NPS 是网络策略和访问服务服务器角色的一个角色服务。

>[!TIP]
>默认情况下，NPS 在所有已安装的网络适配器上侦听端口 1812、1813、1645 和 1646 上的 RADIUS 流量。 安装 NPS 时，如果启用具有高级安全性的 Windows 防火墙，则这些端口的防火墙例外会自动创建用于 IPv4 和 IPv6 通信。 如果网络访问服务器被配置为通过这些默认端口之外的端口发送 RADIUS 流量，请在安装 NPS 期间删除在 "高级安全 Windows 防火墙" 中创建的例外，并为用于的端口创建例外。RADIUS 流量。

**适用于 Windows PowerShell 的过程：**

若要使用 Windows PowerShell 执行此过程，请以管理员身份运行 Windows PowerShell，输入以下 cmdlet：

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**服务器管理器过程：**

1.  在服务器管理器中，选择 "**管理**"，然后选择 "**添加角色和功能**"。
        将打开“添加角色和功能向导”。

2.  在 "开始之前" 中，选择 "**下一步**"。

    >[!NOTE] 
    >如果以前选择了 "添加角色和功能向导" 运行时**默认跳过此页**，则不会显示 "添加角色和功能向导" 的 "**开始之前**" 页。

3.  在 "选择安装类型" 中，确保选择了 "**基于角色或基于功能的安装**"，然后选择 "**下一步**"。

4.  在 "选择目标服务器" 中，确保已选择 **"从服务器池中选择服务器"** 。

5.  在 "服务器池" 中，确保选中 "本地计算机"，然后选择 "**下一步**"。

6.  在 "选择服务器角色" 的 "**角色**" 中，选择 "**网络策略和访问服务**"。 此时会打开一个对话框，询问是否应添加网络策略和访问服务所需的功能。

7.  选择 "**添加功能**"，然后选择 "**下一步**"

8.  在 "选择功能" 中选择 "**下一步**"，在 "网络策略和访问服务" 中查看提供的信息，然后选择 "**下一步**"。

9.  在 "选择角色服务" 中，选择 "**网络策略服务器**"。

10. 对于网络策略服务器所需的功能，请选择 "**添加功能**"，然后选择 "**下一步**"。

11. 在 "确认安装选择" 中，选择 "**如果需要，自动重新启动目标服务器"** 。

12. 选择 **"是"** 以确认所选的，然后选择 "**安装**"。
    
    "安装进度" 页面将在安装过程中显示状态。 此过程完成后，将显示消息 " *computername*上安装成功"，其中*ComputerName*是安装了网络策略服务器的计算机的名称。

13. 选择**关闭**。

## <a name="configure-nps"></a>配置 NPS

安装 NPS 后，您可以将 NPS 配置为处理从 VPN 服务器接收的连接请求的所有身份验证、授权和记帐职责。

### <a name="register-the-nps-server-in-active-directory"></a>在 Active Directory 中注册 NPS 服务器

在此过程中，您将在 Active Directory 中注册服务器，以便它在处理连接请求时有权访问用户帐户信息。

**方法**

1.  在服务器管理器中，选择 "**工具**"，然后选择 "**网络策略服务器**"。 此时将打开 NPS 控制台。

2.  在 NPS 控制台中，右键单击 " **nps （本地）** "，然后选择 **"在 Active Directory 中注册服务器**"。
   
     此时将打开 "网络策略服务器" 对话框。

3.  在 "网络策略服务器" 对话框中，选择 **"确定"** 两次。

有关注册 NPS 的替代方法，请参阅[在 Active Directory 域中注册 Nps 服务器](../../../../../networking/technologies/nps/nps-manage-register.md)。

### <a name="configure-network-policy-server-accounting"></a>配置网络策略服务器记帐

在此过程中，使用以下日志记录类型之一配置网络策略服务器记帐：

- **事件日志记录**。 主要用于对连接尝试进行审核和故障排除。 您可以通过在 NPS 控制台中获取 NPS 服务器属性来配置 NPS 事件日志记录。

- **将用户身份验证和记帐请求记录到本地文件**。 主要用于连接分析和计费。 它也用作一种安全调查工具，因为它为您提供了一种在攻击后跟踪恶意用户活动的方法。 您可以使用记帐配置向导来配置本地文件日志记录。

- **将用户身份验证和记帐请求记录到与 XML 兼容的 Microsoft SQL Server 数据库**。 用于允许多台运行 NPS 的服务器具有一个数据源。 还提供了使用关系数据库的优点。 您可以使用记帐配置向导来配置 SQL Server 日志记录。

若要配置网络策略服务器记帐，请参阅[配置网络策略服务器记帐](../../../../../networking/technologies/nps/nps-accounting-configure.md)。

### <a name="add-the-vpn-server-as-a-radius-client"></a>将 VPN 服务器添加为 RADIUS 客户端

在[配置 ALWAYS ON vpn 的远程访问服务器](vpn-deploy-ras.md)部分，已安装并配置了 VPN 服务器。 在 VPN 服务器配置过程中，你在 VPN 服务器上添加了一个 RADIUS 共享机密。

在此过程中，将使用相同的共享机密文本字符串，将 VPN 服务器配置为 NPS 中的 RADIUS 客户端。 使用在 VPN 服务器上使用的相同文本字符串，或 NPS 服务器与 VPN 服务器之间的通信失败。

>[!IMPORTANT] 
>向网络中添加新的网络访问服务器（VPN 服务器、无线访问点、身份验证交换机或拨号服务器）时，必须在 NPS 中将该服务器添加为 RADIUS 客户端，以便 NPS 识别并可与网络访问服务器通信。

**方法**

1. 在 NPS 服务器上的 NPS 控制台中，双击 " **RADIUS 客户端和服务器**"。

2. 右键单击 " **RADIUS 客户端**"，并选择 "**新建**"。 此时将打开 "新建 RADIUS 客户端" 对话框。

3. 验证是否选中了 "**启用此 RADIUS 客户端**" 复选框。

4. 在 "**友好名称**" 中，输入 VPN 服务器的显示名称。

5. 在 "**地址（IP 或 DNS）** " 中，输入 NAS IP 地址或 FQDN。
     
     如果输入 FQDN，请选择 "**验证**" 以验证名称是否正确并映射到有效的 IP 地址。

6. 在 "**共享密钥**" 中，执行以下操作：

    1. 确保选择了 "**手动**"。

    2. 输入你还在 VPN 服务器上输入的强文本字符串。

    3. 在 "确认共享机密" 中重新输入共享机密。

7. 选择“确定”。 VPN 服务器将出现在 NPS 服务器上配置的 RADIUS 客户端列表中。

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>将 NPS 配置为 VPN 连接的 RADIUS

在此过程中，将 NPS 配置为组织网络上的 RADIUS 服务器。 在 NPS 上，必须定义一个策略，该策略仅允许特定组中的用户通过 VPN 服务器访问组织/企业网络，然后仅在 PEAP 身份验证请求中使用有效的用户证书时使用。

**方法**

1. 在 NPS 控制台中的 "标准配置" 中，确保已选择 "**用于拨号或 VPN 连接的 RADIUS 服务器**"。

2. 选择 "**配置 VPN 或拨号**"。
        
    此时会打开 "配置 VPN 或拨号向导"。

3. 选择 "**虚拟专用网络（VPN）连接**"，然后选择 "**下一步**"。

4. 在 "指定拨号或 VPN 服务器" 的 "RADIUS 客户端" 中，选择你在上一步中添加的 VPN 服务器的名称。 例如，如果 VPN 服务器的 NetBIOS 名称是 RAS1，请选择**RAS1**。

5. 选择“**下一步**”。

6. 在 "配置身份验证方法" 中，完成以下步骤：

    1. 清除 " **Microsoft 加密身份验证版本2（CHAPv2）** " 复选框。

    2. 选中 "**可扩展身份验证协议**" 复选框以将其选中。

    3. 在 "类型" （根据访问和网络配置的方法）中， **选择 "Microsoft：受保护的 EAP （** PEAP），然后选择 "**配置**"。
      
        此时将打开 "编辑受保护的 EAP 属性" 对话框。

    4. 选择 "**删除**" 以删除安全密码（MSCHAP V2） EAP 类型。

    5. 选择 **添加** 。 此时将打开 "添加 EAP" 对话框。

    6. 选择 "**智能卡或其他证书**"，然后选择 **"确定"** 。

    7. 选择 **"确定" 关闭 "** 编辑受保护的 EAP 属性"。

7. 选择“**下一步**”。

8. 在 "指定用户组" 中完成以下步骤：

    1. 选择 **添加** 。 此时将打开 "选择用户、计算机、服务帐户或组" 对话框。

    2. 输入**VPN 用户**，然后选择 **"确定"** 。

    3. 选择“**下一步**”。

9. 在 "指定 IP 筛选器" 中，选择**下一步**。

10. 在 "指定加密设置" 中，选择 "**下一步**"。 不要进行任何更改。

    这些设置仅适用于此方案不支持的 Microsoft 点对点加密（MPPE）连接。

11. 在 "指定领域名" 中，选择 "**下一步**"。

12. 选择 "**完成**" 关闭向导。

## <a name="autoenroll-the-nps-server-certificate"></a>自动注册 NPS 服务器证书

在此过程中，您将在本地 NPS 服务器上手动刷新组策略。 当组策略刷新时，如果证书自动注册已配置并且工作正常，则证书颁发机构（CA）会自动向本地计算机注册证书。

>[!NOTE]  
>当你重新启动域成员计算机或用户登录到域成员计算机时，组策略自动刷新。 此外，组策略会定期刷新。 默认情况下，这种定期刷新每90分钟执行一次，随机偏移最多30分钟。

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

**方法**

1. 在 NPS 上，打开 Windows PowerShell。

2. 在 Windows PowerShell 提示符下，键入**gpupdate**，然后按 enter。

## <a name="next-steps"></a>后续步骤

[步骤 5.为 Always On VPN](vpn-deploy-dns-firewall.md)配置 DNS 和防火墙设置：在此步骤中，你将使用 Windows PowerShell 或服务器管理器添加角色和功能向导 "安装网络策略服务器（NPS）。 你还可以将 NPS 配置为处理从 VPN 服务器接收的连接请求的所有身份验证、授权和记帐职责。