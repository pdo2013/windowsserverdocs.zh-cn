---
title: 在 NPS 更改后验证配置
description: 在对服务器进行 IP 地址或名称更改后，你可以使用本主题来验证 Windows Server 2016 网络策略服务器配置。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ba1f5f494228f6bdba22b300a2336fa6eb37690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405352"
---
# <a name="verify-configuration-after-nps-changes"></a>在 NPS 更改后验证配置

>适用于：Windows Server（半年频道）、Windows Server 2016

在对服务器进行 IP 地址或名称更改后，可以使用本主题来验证 NPS 配置。

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>确认 NPS IP 地址更改后的配置

在某些情况下，需要更改 NPS 或代理的 IP 地址，例如将服务器移到不同的 IP 子网。 

如果更改了 NPS 或代理 IP 地址，则需要重新配置 NPS 部署的某些部分。 

使用以下常规指南来帮助你验证 IP 地址更改是否不会中断 NPS RADIUS 服务器和 RADIUS 代理服务器在网络上的网络访问身份验证、授权或记帐。

若要执行这些过程，您必须是 "**管理员**" 的成员或同等身份。

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>在 NPS IP 地址更改后验证配置

1. 使用 NPS 的新 IP 地址重新配置所有 RADIUS 客户端，如无线访问点和 VPN 服务器。

2. 如果 NPS 是远程 RADIUS 服务器组的成员，请使用 NPS 的新 IP 地址重新配置 NPS 代理。

3. 如果已将 NPS 配置为使用 SQL Server 日志记录，请验证运行 SQL Server 和 NPS 的计算机之间的连接是否仍然正常工作。

4. 如果已部署 IPsec 以保护 NPS 与 NPS 代理或其他服务器或设备之间的 RADIUS 流量，请在 "高级安全 Windows 防火墙" 中重新配置 IPsec 策略或连接安全规则，以使用 NPS 的新 IP 地址。

5. 如果 NPS 是多宿主的，并且您已将服务器配置为绑定到特定网络适配器，请使用新的 IP 地址重新配置 NPS 端口设置。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>验证 NPS 代理 IP 地址更改后的配置

1. 使用 NPS 代理的新 IP 地址重新配置所有 RADIUS 客户端，如无线访问点和 VPN 服务器。

2. 如果 NPS 代理是多宿主的，并且你已将代理配置为绑定到特定网络适配器，请使用新的 IP 地址重新配置 NPS 端口设置。

3. 用代理服务器 IP 地址重新配置所有远程 RADIUS 服务器组的所有成员。 若要完成此任务，请在已将 NPS 代理配置为 RADIUS 客户端的每个 NPS 上执行以下操作：

    a. 双击 " **NPS （本地）** "，双击 " **radius 客户端和服务器**"，单击 " **radius 客户端**"，然后在详细信息窗格中，双击要更改的 RADIUS 客户端。

    b. 在 RADIUS 客户端**属性**中的**ADDRESS \(IP 或 DNS @ no__t**中，键入 NPS 代理的新 IP 地址。

4. 如果已将 NPS 代理配置为使用 SQL Server 日志记录，请验证运行 SQL Server 和 NPS 代理的计算机之间的连接是否仍然正常工作。

## <a name="verify-configuration-after-renaming-an-nps"></a>在重命名 NPS 之后验证配置

在某些情况下，需要更改 NPS 或代理的名称（例如，在重新设计服务器的命名约定时）。

如果更改 NPS 或代理名称，则必须重新配置 NPS 部署的某些部分。 

使用以下常规指南来帮助你验证服务器名称更改是否不会中断网络访问身份验证、授权或记帐。

若要执行此过程，您必须是 "**管理员**" 的成员或同等身份。

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>在 NPS 或代理名称更改之后验证配置

1. 如果 NPS 是远程 RADIUS 服务器组的成员，并且组配置了计算机名称而不是 IP 地址，则使用新的 NPS 名称重新配置远程 RADIUS 服务器组。

2. 如果基于证书的身份验证方法是在 NPS 上部署的，则名称更改会使服务器证书失效。 你可以从证书颁发机构（CA）管理员请求新证书，或者，如果计算机是域成员计算机，并且你将证书自动注册到域成员，则可以刷新组策略以通过自动注册来获取新证书. 刷新组策略：

    a. 打开 "命令提示符" 或 "Windows PowerShell"。

    b. 键入 **gpupdate**，然后按 Enter。


3. 使用新的服务器证书后，请请求 CA 管理员吊销旧证书。 

     吊销旧证书后，NPS 将继续使用该证书，直到旧证书过期。 默认情况下，旧证书将保持有效的最长时间为一周和10小时。 此时间段可能会不同，具体取决于证书吊销列表（CRL）过期情况和传输层安全性（TLS）缓存时间到期是否已从其默认值中修改。 默认 CRL 过期时间为一周，默认的 TLS 缓存时间到期时间为10小时。 

     不过，如果想要将 NPS 配置为立即使用新证书，则可以使用新证书手动重新配置网络策略。

4. 旧证书过期后，NPS 将自动开始使用新证书。 

5. 如果已将 NPS 配置为使用 SQL Server 日志记录，请验证运行 SQL Server 和 NPS 的计算机之间的连接是否仍然正常工作。

