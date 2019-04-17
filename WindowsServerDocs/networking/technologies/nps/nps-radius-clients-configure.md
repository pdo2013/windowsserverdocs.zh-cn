---
title: 配置 RADIUS 客户端
description: 本主题提供的有关适用于 Windows Server 2016 中的网络策略 Server 配置 RADIUS 客户端的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9af3189199c8282394ca34f181a90b4290c55044
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-radius-clients"></a>配置 RADIUS 客户端

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题配置为在 NPS RADIUS 客户端的网络的访问权限的服务器。

当你添加新的网络访问权限服务器 \（切换或拨号 server\ 进行身份验证的无线接入点 VPN 服务器）必须到你的网络添加作为 NPS，在客户端 RADIUS 服务器，然后配置 RADIUS 客户端与 NPS 服务器进行通信。

>[!IMPORTANT]
>客户端计算机和笔记本电脑、平板电脑、手机和其他计算机运行的客户端操作系统，如设备未 RADIUS 客户端。 RADIUS 客户端是网络的访问权限服务器的无线接入点，如 802.1 X 支持切换、虚拟专用网络 (VPN) 服务器和拨号服务器-，因为他们使用 RADIUS 协议 RADIUS 服务器，如网络策略服务器 \(NPS\) 服务器与其通信。

此外在 NPS 服务器属于 NPS 代理配置远程 RADIUS 服务器组时需要此步骤。 在此情况下，除了执行 NPS 代理，在此任务中的步骤必须执行以下操作：

- 上 NPS 的代理配置包含 NPS 服务器远程 RADIUS 服务器组。
- 在远程 NPS 服务器上，将配置为 RADIUS 客户端的 NPS 代理。

本主题中执行这些过程，你必须至少一个网络的访问权限服务器 \（切换或拨号 server\ 进行身份验证的无线接入点 VPN 服务器）或 NPS 代理物理安装在你的网络。

## <a name="configure-the-network-access-server"></a>配置网络的访问权限服务器

使用此过程与 NPS 配置为使用的网络访问权限服务器。 部署作为 RADIUS 客户端的网络访问权限服务器 (Nas) 时，你必须配置的客户端进行通信，其中将 Nas 配置为客户端 NPS 服务器。

此过程将提供有关应用来配置你的 Nas; 设置一般指南有关特定如何配置部署网络的设备上的说明，请参阅 NAS 产品文档。

### <a name="to-configure-the-network-access-server"></a>若要配置网络的访问权限服务器

1. 上，在**RADIUS 设置**、选择**RADIUS 身份验证**用户数据报协议 (UDP) 端口上**1812 年**和**RADIUS 记帐**UDP 端口上**1813 年**。
2. 在**身份验证的服务器**或**RADIUS 服务器**，指定 IP 地址或完整的域名称 (FQDN)，具体取决于的 nas 要求你 NPS 服务器。 
3. 在**机密**或**共享机密**，键入强密码。 当 NAS 配置为在 NPS RADIUS 客户端上时，您将使用相同的密码，因此不要忘记它。
4. 如果你要用作 PEAP 或 EAP 身份验证方法、配置 NAS 使用 EAP 身份验证。
5. 如果你在配置的无线接入点，**SSID**，指定服务设置标识符 \(SSID\)，即用作的网络名称字母数字字符串。 此名称向客户端的无线接入点的广播，而且是在你的无线保真度 \(Wi-Fi\) 热点用户看到。
6. 如果你在配置的无线接入点，**802.1 X 和 WPA**，启用**IEEE 802.1 X 身份验证**你是否希望将其部署 PEAP 的 MS-CHAP v2、PEAP-TLS 或 EAP-TLS。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>添加网络的访问权限服务器作为中 NPS RADIUS 客户端

使用此过程中 NPS RADIUS 客户为添加网络的访问权限服务器。 你可以使用此过程通过使用控制台 NPS RADIUS 客户端作为配置 NAS。

若要完成此过程，你必须**管理员**组。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>若要添加网络的访问权限服务器作为中 NPS RADIUS 客户端

1. 在 NPS 服务器，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。
2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**。 右键单击**RADIUS 客户端**，然后单击**新 RADIUS 客户端**。 
3. 在**新 RADIUS 客户端**，确认**启用此 RADIUS 客户端**复选框处于选中状态。
4. 在**新 RADIUS 客户端**中**的友好名称**，键入 nas 的显示名称。 在**地址（DNS）**，键入 NAS IP 地址或合法的域名 (FQDN)。 如果你输入 FQDN 后，请单击**验证**如果你想要验证名称正确，并且将映射到一个有效的 IP 地址。 
5. 在**新 RADIUS 客户端**中**供应商**，指定 NAS 制造商的名称。 如果你不知道 NAS 制造商的名称，请选择**RADIUS 标准**。
6. 在**新 RADIUS 客户端**中**共享机密**，执行下列操作之一：
    - 确保**手册**选择，然后在**共享机密**，键入还 NAS 输入的强密码。 重新键入中的共享的机密**确认共享的机密**。
    - 选择**生成**，然后单击**生成**可自动生成共享的机密。 配置的生成共享的机密节省 NAS，以便它可以与 NPS server 通信。
7. 在**新 RADIUS 客户端**中**其他选项**，如果你正在使用任何身份验证方法以外 EAP 和 PEAP，并且如果你的 NAS 支持使用消息 authenticator 属性，请选择**访问请求邮件必须包含消息 Authenticator 特性**。
8. 单击**确定**。 你的 NAS RADIUS NPS 服务器上配置的客户端的列表中显示。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 数据中心中配置 RADIUS 的 IP 地址范围内的客户端

如果你运行的 Windows Server 2016 Datacenter，您可以配置的 IP 地址范围在 NPS RADIUS 客户端。 这允许你一次而不是每个 RADIUS 客户端单独的添加添加到 NPS 控制台大量 RADIUS（如无线接入点）的客户端。

如果你运行的 NPS Windows Server 2016 标准你不能配置 IP 地址范围 RADIUS 客户端。

使用此过程添加作为配置了从同一 IP 地址范围 IP 地址的 RADIUS 客户端的一组网络的访问权限服务器 (Nas)。

所有覆盖范围内 RADIUS 客户必须使用相同的配置和共享的机密。

若要完成此过程，你必须**管理员**组。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>若要设置的 IP 地址范围 RADIUS 客户端

1. 在 NPS 服务器，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。
2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**。 右键单击**RADIUS 客户端**，然后单击**新 RADIUS 客户端**。
3. 在**新 RADIUS 客户端**中**的友好名称**，键入 Nas 集合的显示名称。
4. 在**地址 \(IP or DNS\)**，键入 IP 地址范围内使用无类别间域路由 \(CIDR\) 表示法 RADIUS 客户端。 例如，如果 Nas IP 地址范围 10.10.0.0，请键入**10.10.0.0/16**。
5. 在**新 RADIUS 客户端**中**供应商**，指定 NAS 制造商的名称。 如果你不知道 NAS 制造商的名称，请选择**RADIUS 标准**。
6. 在**新 RADIUS 客户端**中**共享机密**，执行下列操作之一：
    - 确保**手册**选择，然后在**共享机密**，键入还 NAS 输入的强密码。 重新键入中的共享的机密**确认共享的机密**。
    - 选择**生成**，然后单击**生成**可自动生成共享的机密。 配置的生成共享的机密节省 NAS，以便它可以与 NPS server 通信。
7. 在**新 RADIUS 客户端**中**其他选项**，如果你使用任何身份验证方法以外 EAP 和 PEAP，并且如果所有你的 Nas 都支持使用消息 authenticator 属性，请选择**访问请求邮件必须包含消息 Authenticator 特性**。
8. 单击**确定**。 你的 Nas 显示在列表中 RADIUS NPS 服务器上配置的客户端。

有关详细信息，请参阅[RADIUS 客户端](nps-radius-clients.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。