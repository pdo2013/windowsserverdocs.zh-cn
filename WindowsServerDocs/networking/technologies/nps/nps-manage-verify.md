---
title: 验证 NPS 更改后的配置
description: 本主题可用于 IP 地址或名称将更改为服务器后，验证 Windows Server 2016 网络策略服务器配置。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880668"
---
# <a name="verify-configuration-after-nps-changes"></a>验证 NPS 更改后的配置

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题可用于 IP 地址或名称将更改为服务器后，验证 NPS 配置。

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>验证 NPS IP 地址更改后的配置

可能情况下需要更改 NPS 或代理服务器，如将服务器移到不同的 IP 子网的 IP 地址。 

如果更改 NPS 或代理 IP 地址，则需要重新配置的 NPS 部署部分。 

使用以下常规准则来协助您验证 IP 地址的更改不会中断网络访问身份验证、 授权或记帐 NPS RADIUS 服务器和 RADIUS 代理服务器的网络上。

您必须是属于**管理员**，或等效的若要执行这些过程。

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>若要验证 NPS IP 地址更改后的配置

1. 重新配置所有 RADIUS 客户端，如无线访问点和 VPN 服务器，NPS 的新 IP 地址。

2. 如果 NPS 是远程 RADIUS 服务器组的成员，重新配置使用新的 IP 地址的 NPS NPS 代理。

3. 如果已配置 NPS 以使用 SQL Server 日志记录，验证运行 SQL Server 和 NPS 的计算机之间的连接仍在正常。

4. 如果已部署 IPsec 来保护您的 NPS 和 NPS 代理或其他服务器或设备之间的 RADIUS 流量，重新配置 IPsec 策略或具有高级安全性，以使用新的 IP 地址，NPS 的 Windows 防火墙中的连接安全规则。

5. 如果 NPS 是多宿主和已经配置了服务器要绑定到特定网络适配器，重新配置 NPS 使用新的 IP 地址的端口设置。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>若要验证配置后 NPS 代理 IP 地址更改

1. 重新配置所有 RADIUS 客户端，如无线访问点和 VPN 服务器，NPS 代理的新 IP 地址。

2. 如果 NPS 代理是多宿主并且已配置代理后，若要将绑定到特定网络适配器，重新配置 NPS 使用新的 IP 地址的端口设置。

3. 重新配置代理服务器 IP 地址与所有远程 RADIUS 服务器组的所有成员。 若要完成此任务，在具有 NPS 代理配置为 RADIUS 客户端每个 NPS:

    a. 双击**NPS （本地）**，双击**RADIUS 客户端和服务器**，单击**RADIUS 客户端**，然后在详细信息窗格中，双击 RADIUS 客户端和你想要更改。

    b. 在 RADIUS 客户端**属性**，在**地址\(IP 或 DNS\)**，键入 NPS 代理的新 IP 地址。

4. 如果已配置 NPS 代理服务器以使用 SQL Server 日志记录，验证运行 SQL Server 和 NPS 代理的计算机之间的连接仍在正常。

## <a name="verify-configuration-after-renaming-an-nps"></a>验证 NPS 重命名后的配置

可能情况下，当您需要更改 NPS 或代理服务器，例如当您重新设计的命名约定为服务器的名称。

如果更改 NPS 或代理名称，则需要重新配置的 NPS 部署部分。 

使用以下常规准则来协助您验证网络访问身份验证、 授权或记帐服务器名称更改不会中断。

您必须是属于**管理员**，或等效的若要执行此过程。

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>若要验证的 NPS 或代理名称更改后的配置

1. 如果 NPS 是远程 RADIUS 服务器组的成员，并且与计算机名称而不是 IP 地址配置了组，重新配置远程 RADIUS 服务器组使用新的 NPS 名称。

2. 如果在 NPS 中部署基于证书的身份验证方法，该名称更改使无效的服务器证书。 可以从证书颁发机构 (CA) 管理员请求新证书，或如果计算机是域成员计算机并且您将证书自动注册到域成员，则可以刷新组策略，以获取新的证书自动注册过程. 若要刷新组策略：

    a. 打开命令提示符或 Windows PowerShell。

    b. 键入 **gpupdate**，然后按 Enter。


3. 新的服务器证书后，请求 CA 管理员吊销旧证书。 

     吊销旧证书后，NPS 将继续使用它，直到旧证书过期。 默认情况下，旧证书将一个每周 10 小时的最长时间保持有效。 此时间段可能不同，具体取决于是否的证书吊销列表 (CRL) 到期时间和传输层安全性 (TLS) 缓存时间过期已修改的默认值。 默认 CRL 过期时间为一周;TLS 的默认缓存到期日期在 10 小时的时间。 

     如果你想要将 NPS 配置为立即使用新证书，但是，您可以手动重新配置网络策略以使用新证书。

4. 旧证书过期后，NPS 会自动开始使用新证书。 

5. 如果已配置 NPS 以使用 SQL Server 日志记录，验证运行 SQL Server 和 NPS 的计算机之间的连接仍在正常。

