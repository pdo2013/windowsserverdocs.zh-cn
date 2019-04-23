---
title: 配置 RADIUS 客户端
description: 本主题提供有关用于 Windows Server 2016 中的网络策略服务器配置 RADIUS 客户端的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cde37849-ce79-4c26-aa14-cd0ef31cae18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 851bdb0677a17567f81a2c331baad595fe40436a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839488"
---
# <a name="configure-radius-clients"></a>配置 RADIUS 客户端

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于将网络访问服务器配置为 NPS 中的 RADIUS 客户端。

添加新的网络访问服务器时\(VPN 服务器、 无线访问点进行身份验证交换机或拨号服务器\)到网络时，必须将服务器添加为 RADIUS 客户端在 NPS 中，，然后配置 RADIUS 客户端通信使用 NPS。

>[!IMPORTANT]
>客户端计算机和设备，如便携式计算机、 平板电脑、 手机和其他运行客户端操作系统的计算机不是 RADIUS 客户端。 RADIUS 客户端是网络访问服务器-如无线访问点、 802.1 X 的切换、 虚拟专用网络 (VPN) 服务器和拨号服务器的因为它们使用 RADIUS 协议来与 RADIUS 服务器，如网络策略进行通信服务器\(NPS\)服务器。

在 NPS 是在 NPS 代理服务器配置远程 RADIUS 服务器组的成员时，此步骤中还有必要。 在这种情况下，除了执行在 NPS 代理服务器上，此任务中的步骤必须执行以下操作：

- 在 NPS 代理服务器上，配置包含 NPS 的远程 RADIUS 服务器组。
- 在远程 NPS 作为 RADIUS 客户端配置 NPS 代理。

若要执行本主题中的过程，必须具有至少一个网络访问服务器\(VPN 服务器、 无线访问点进行身份验证交换机或拨号服务器\)或以物理方式安装在你的网络上的 NPS 代理。

## <a name="configure-the-network-access-server"></a>配置网络访问服务器

使用下列步骤使用 NPS 配置为使用网络访问服务器。 在部署为 RADIUS 客户端的网络访问服务器 (Nas) 时，必须配置客户端与其中为客户端配置 Nas NPSs 通信。

此过程提供了有关应使用以配置你 Nas; 设置的常规指导原则有关如何配置的设备在网络部署的特定说明，请参阅 NAS 产品文档。

### <a name="to-configure-the-network-access-server"></a>若要配置网络访问服务器

1. 在 NAS 上中**RADIUS 设置**，选择**RADIUS 身份验证**用户数据报协议 (UDP) 端口上**1812年**和**RADIUS 记帐**UDP 端口上**1813年**。
2. 在中**身份验证服务器**或**RADIUS 服务器**，指定按 IP 地址或根据 NAS 要求完全限定的域名 (FQDN)，在 NPS。 
3. 在中**机密**或**共享机密**，键入一个强密码。 当在 NPS 中的 RADIUS 客户端配置 NAS 时，将使用相同的密码，因此请不要忘记它。
4. 如果使用 PEAP 或 EAP 作为身份验证方法，配置 NAS 使用 EAP 身份验证。
5. 如果要在配置无线访问点**SSID**，指定服务设置标识符\(SSID\)，这是充当网络名称的字母数字字符串。 此名称到无线客户端广播的访问点和用户都可以无线保真度可见\(Wi-fi\)热点。
6. 如果要在配置无线访问点**802.1x 和 WPA**，启用**IEEE 802.1x 身份验证**如果想要部署 PEAP MS-CHAP v2、 PEAP-TLS 或 EAP-TLS。

## <a name="add-the-network-access-server-as-a-radius-client-in-nps"></a>在 NPS 中的 RADIUS 客户端添加网络访问服务器

使用此过程在 NPS 中的 RADIUS 客户端添加的网络访问服务器。 此过程可用于通过使用 NPS 控制台将 NAS 配置为 RADIUS 客户端。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>若要添加的网络访问服务器在 NPS 中的 RADIUS 客户端

1. 在 NPS 中，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。
2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**。 右键单击**RADIUS 客户端**，然后单击**新建 RADIUS 客户端**。 
3. 在中**新建 RADIUS 客户端**，确认**启用此 RADIUS 客户端**复选框处于选中状态。
4. 在中**新建 RADIUS 客户端**，在**友好名称**，键入 NAS 的显示名称。 在中**地址 （IP 或 DNS）**，键入 NAS IP 地址或完全限定的域名 (FQDN)。 如果输入的 FQDN，请单击**验证**如果想要验证名称是否正确以及映射到有效的 IP 地址。 
5. 在中**新建 RADIUS 客户端**，在**供应商**，指定 NAS 制造商名称。 如果不知道 NAS 制造商的名称，请选择**RADIUS 标准**。
6. 在中**新建 RADIUS 客户端**，在**共享机密**，执行下列操作之一：
    - 请确保**手动**已选中，然后在**共享机密**，键入在 NAS 也输入强密码。 重新键入中的共享的机密**确认共享的机密**。
    - 选择**Generate**，然后单击**生成**自动生成一个共享的机密。 将配置的生成共享的机密保存在 NAS 上，以便它可以与 NPS 通信。
7. 在中**新建 RADIUS 客户端**，在**其他选项**，如果你正在使用非 EAP 和 PEAP，任何身份验证方法，并且如果您的 NAS 支持消息身份验证器属性，请选择**访问请求消息必须包含消息身份验证器属性**。
8. 单击 **“确定”**。 您的 NAS 出现在 NPS 上配置的 RADIUS 客户端的列表中。

## <a name="configure-radius-clients-by-ip-address-range-in-windows-server-2016-datacenter"></a>在 Windows Server 2016 数据中心中配置 RADIUS 客户端的 IP 地址范围

如果你正在运行 Windows Server 2016 Datacenter，您可以通过 IP 地址范围在 NPS 中配置 RADIUS 客户端。 这可以大量 RADIUS 客户端 （如无线访问点），一次添加到 NPS 控制台而不是单独添加每个 RADIUS 客户端。

如果在 Windows Server 2016 标准版运行 NPS，无法通过 IP 地址范围配置 RADIUS 客户端。

使用此过程为使用相同的 IP 地址范围的 IP 地址而配置的 RADIUS 客户端添加一组网络访问服务器 (Nas)。

所有的范围中的 RADIUS 客户端必须使用相同的配置和共享的机密。

若要完成此过程，你必须是**Administrators**组的成员。

### <a name="to-set-up-radius-clients-by-ip-address-range"></a>若要设置的 IP 地址范围的 RADIUS 客户端

1. 在 NPS 中，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。
2. 在 NPS 控制台中，双击**RADIUS 客户端和服务器**。 右键单击**RADIUS 客户端**，然后单击**新建 RADIUS 客户端**。
3. 在中**新建 RADIUS 客户端**，在**友好名称**，键入 Nas 的集合的显示名称。
4. 在中**地址\(IP 或 DNS\)**，键入 RADIUS 客户端的 IP 地址范围，使用无类域间路由\(CIDR\)表示法。 例如，如果 IP 地址范围为 Nas 10.10.0.0，则输入**10.10.0.0/16**。
5. 在中**新建 RADIUS 客户端**，在**供应商**，指定 NAS 制造商名称。 如果不知道 NAS 制造商的名称，请选择**RADIUS 标准**。
6. 在中**新建 RADIUS 客户端**，在**共享机密**，执行下列操作之一：
    - 请确保**手动**已选中，然后在**共享机密**，键入在 NAS 也输入强密码。 重新键入中的共享的机密**确认共享的机密**。
    - 选择**Generate**，然后单击**生成**自动生成一个共享的机密。 将配置的生成共享的机密保存在 NAS 上，以便它可以与 NPS 通信。
7. 在中**新建 RADIUS 客户端**，在**其他选项**，如果正在使用非 EAP 和 PEAP，任何身份验证方法，并且如果所有你 Nas 支持消息身份验证器属性，请选择**访问请求消息必须包含消息身份验证器属性**。
8. 单击 **“确定”**。 在 NPS 上配置的 RADIUS 客户端的列表中显示你 Nas。

有关详细信息，请参阅[RADIUS 客户端](nps-radius-clients.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。