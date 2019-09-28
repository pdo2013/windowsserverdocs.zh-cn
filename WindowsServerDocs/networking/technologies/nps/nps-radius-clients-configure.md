---
title: 配置 RADIUS 客户端
description: 本主题提供有关为 Windows Server 2016 中的网络策略服务器配置 RADIUS 客户端的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6870029e02ae91b1ef5bf4d4302ac2bed2e27d84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405292"
---
# <a name="configure-radius-clients"></a>配置 RADIUS 客户端

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题在 NPS 中将网络访问服务器配置为 RADIUS 客户端。

向网络中添加新的网络访问服务器 @no__t 0VPN 服务器、无线访问点、身份验证交换机或拨号服务器 @ no__t）时，必须在 NPS 中将该服务器添加为 RADIUS 客户端，然后将 RADIUS 客户端配置为与 NPS 通信。

>[!IMPORTANT]
>客户端计算机和设备（如便携式计算机、平板电脑、电话和其他运行客户端操作系统的计算机）不是 RADIUS 客户端。 RADIUS 客户端是网络访问服务器（如无线访问点、支持 802.1 X 的交换机、虚拟专用网络（VPN）服务器和拨号服务器），因为它们使用 RADIUS 协议来与 RADIUS 服务器（如网络策略）进行通信服务器 \(NPS @ no__t 服务器。

如果 NPS 是在 NPS 代理上配置的远程 RADIUS 服务器组的成员，则此步骤也是必需的。 在这种情况下，除了在 NPS 代理上执行此任务中的步骤外，还必须执行以下操作：

- 在 NPS 代理上，配置包含 NPS 的远程 RADIUS 服务器组。
- 在远程 NPS 上，将 NPS 代理配置为 RADIUS 客户端。

若要执行本主题中的过程，你必须在网络上安装至少一个网络访问服务器 @no__t 0VPN 服务器、无线访问点、身份验证交换机或拨号服务器 @ no__t 或 NPS 代理。

## <a name="configure-the-network-access-server"></a>配置网络访问服务器

使用此过程来配置用于 NPS 的网络访问服务器。 将网络访问服务器（Nas）作为 RADIUS 客户端部署时，必须将客户端配置为与 Nas 配置为客户端的 NPSs 通信。

此过程提供了有关配置 Nas 时应使用的设置的一般指导原则;有关如何配置在网络上部署的设备的特定说明，请参阅 NAS 产品文档。

### <a name="to-configure-the-network-access-server"></a>配置网络访问服务器

1. 在 NAS 的 " **radius 设置**" 中，选择 "用户数据报协议（udp）端口**1812**上的**RADIUS 身份验证**" 和 UDP 端口**1813**上的 " **radius 记帐**"。
2. 在 "**身份验证服务器**" 或 " **RADIUS 服务器**" 中，根据 NAS 的要求，按 IP 地址或完全限定的域名（FQDN）指定 NPS。 
3. 在 "**机密**" 或 "**共享密钥**" 中，键入强密码。 将 NAS 配置为 NPS 中的 RADIUS 客户端时，将使用相同的密码，因此不要忘记。
4. 如果使用 PEAP 或 EAP 作为身份验证方法，请将 NAS 配置为使用 EAP 身份验证。
5. 如果要配置无线访问点，请在 " **SSID**" 中指定服务集标识符 \(SSID @ no__t-2，这是用作网络名称的字母数字字符串。 此名称由访问点向无线客户端广播，并对无线保真 \(Wi-Fi @ no__t 热点的用户可见。
6. 如果要配置无线访问点，请在**802.1 x 和 WPA**中启用**IEEE 802.1 x 身份验证**（如果想要部署 peap-ms-chap V2、peap-tls 或 eap-tls）。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>在 NPS 中将网络访问服务器添加为 RADIUS 客户端

使用此过程在 NPS 中将网络访问服务器添加为 RADIUS 客户端。 您可以使用此过程通过 NPS 控制台将 NAS 配置为 RADIUS 客户端。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>在 NPS 中将网络访问服务器添加为 RADIUS 客户端

1. 在 NPS 上服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。
2. 在 NPS 控制台中，双击 " **RADIUS 客户端和服务器**"。 右键单击 " **Radius 客户端**"，然后单击 "**新建 radius 客户端**"。 
3. 在 "**新建 Radius 客户端**" 中，验证是否选中了 "**启用此 radius 客户端**" 复选框。
4. 在 "**新建 RADIUS 客户端**" 的 "**友好名称**" 中，键入 NAS 的显示名称。 在 "**地址（IP 或 DNS）** " 中，键入 NAS IP 地址或完全限定的域名（FQDN）。 如果输入 FQDN，请单击 "**验证**" 以验证名称是否正确并映射到有效的 IP 地址。 
5. 在 "**新建 RADIUS 客户端**" 的 "**供应商**" 中，指定 NAS 制造商名称。 如果不确定 NAS 制造商名称，请选择 " **RADIUS 标准**"。
6. 在 "**新建 RADIUS 客户端**" 的 "**共享密钥**" 中，执行下列操作之一：
    - 确保选择 "**手动**"，然后在 "**共享密钥**" 中，键入还在 NAS 上输入的强密码。 在 "**确认共享机密**" 中重新输入共享机密。
    - 选择 "**生成**"，然后单击 "**生成**" 以自动生成共享机密。 在 NAS 上保存用于配置的已生成共享机密，使其能够与 NPS 通信。
7. 在 "**新建 RADIUS 客户端**" 的 "**其他选项**" 中，如果你使用的是 EAP 和 PEAP 以外的任何身份验证方法，并且如果你的 NAS 支持使用消息身份验证器属性，则选择 "**访问请求消息必须包含消息"身份验证器属性**。
8. 单击 **“确定”** 。 NAS 出现在 NPS 上配置的 RADIUS 客户端列表中。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 Datacenter 中按 IP 地址范围配置 RADIUS 客户端

如果运行的是 Windows Server 2016 Datacenter，则可以按 IP 地址范围在 NPS 中配置 RADIUS 客户端。 这使你可以一次向 NPS 控制台添加大量 RADIUS 客户端（如无线访问点），而不是单独添加每个 RADIUS 客户端。

如果在 Windows Server 2016 Standard 上运行 NPS，则无法按 IP 地址范围配置 RADIUS 客户端。

使用此过程将一组网络访问服务器（Nas）添加为 RADIUS 客户端，这些客户端均使用同一 IP 地址范围中的 IP 地址进行配置。

范围内的所有 RADIUS 客户端必须使用相同的配置和共享机密。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>按 IP 地址范围设置 RADIUS 客户端

1. 在 NPS 上服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。
2. 在 NPS 控制台中，双击 " **RADIUS 客户端和服务器**"。 右键单击 " **Radius 客户端**"，然后单击 "**新建 radius 客户端**"。
3. 在 "**新建 RADIUS 客户端**" 的 "**友好名称**" 中，键入 nas 集合的显示名称。
4. 在**Address \(IP 或 DNS @ no__t-2**中，通过使用无类别的域间路由 \(CIDR @ no__t 表示法键入 RADIUS 客户端的 IP 地址范围。 例如，如果 Nas 的 IP 地址范围是10.10.0.0，请键入**10.10.0.0/16**。
5. 在 "**新建 RADIUS 客户端**" 的 "**供应商**" 中，指定 NAS 制造商名称。 如果不确定 NAS 制造商名称，请选择 " **RADIUS 标准**"。
6. 在 "**新建 RADIUS 客户端**" 的 "**共享密钥**" 中，执行下列操作之一：
    - 确保选择 "**手动**"，然后在 "**共享密钥**" 中，键入还在 NAS 上输入的强密码。 在 "**确认共享机密**" 中重新输入共享机密。
    - 选择 "**生成**"，然后单击 "**生成**" 以自动生成共享机密。 在 NAS 上保存用于配置的已生成共享机密，使其能够与 NPS 通信。
7. 在 "**新建 RADIUS 客户端**" 的 "**其他选项**" 中，如果你使用的是 EAP 和 PEAP 以外的任何身份验证方法，并且你的所有 nas 支持使用消息身份验证器属性，则选择 "**访问请求消息必须包含消息身份验证器属性**。
8. 单击 **“确定”** 。 Nas 出现在 NPS 上配置的 RADIUS 客户端列表中。

有关详细信息，请参阅[RADIUS 客户端](nps-radius-clients.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。