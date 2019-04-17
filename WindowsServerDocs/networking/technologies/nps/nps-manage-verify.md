---
title: 确认后 NPS 服务器更改的配置
description: 你可以使用本主题 IP 地址或名称更改服务器后验证配置 Windows Server 2016 网络策略的服务器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>确认后 NPS 服务器更改的配置

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题验证 NPS server 配置后 IP 地址或名称将更改为服务器。

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>确认后 NPS Server IP 地址的更改的配置

可能会有情况，你需要更改 NPS 服务器或代理，例如当你到达服务器不同 IP 子的 IP 地址。 

如果你改变 NPS 服务器或代理 IP 地址，则必要重新配置 NPS 部署的各部分。 

使用以下常规指南向你提供帮助验证网络的访问权限身份验证、授权或记帐 NPS RADIUS 服务器和 RADIUS 代理服务器的网络上的 IP 地址更改不会中断。

你必须成员**管理员**，或相当，以执行这些过程。

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>若要验证后 NPS 配置服务器 IP 地址更改

1. 重新所有 RADIUS 客户端，如无线接入点和 VPN 服务器，使用新的 IP 地址 NPS 服务器的都配置。

2. NPS 服务器是否远程 RADIUS 服务器组中的成员，请重新 NPS 使用新的 IP 地址 NPS 服务器的代理配置。

3. 如果你已经将配置 NPS 服务器使用 SQL Server 日志记录，验证的计算机运行的 SQL Server 和 NPS 服务器之间的连接仍在正常。

4. 如果您已部署 IPsec 来保护你 NPS 服务器和 NPS 代理或其他服务器或设备之间的 RADIUS 通信，重新配置 IPsec 策略或连接安全规则在使用新的 IP 地址的 NPS 服务器高级安全 Windows 防火墙。

5. 如果你已经配置了服务器绑定到特定网络适配器 NPS 服务器是多宿主，重新配置 NPS 端口设置新的 IP 地址。

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>若要验证后 NPS 配置代理 IP 地址更改

1. 重新所有 RADIUS 客户端，如无线接入点和 VPN 服务器，使用新的 IP 地址的 NPS 代理都配置。

2. 如果 NPS 代理多主机，并且你已经将绑定到特定网络适配器的代理配置，重新配置 NPS 端口设置新的 IP 地址。

3. 重新配置所有所有远程 RADIUS 服务器具有的组成员代理服务器 IP 地址。 若要完成此任务中的，在每个 NPS 服务器具有 NPS 代理配置 RADIUS 客户：

    。 双击**NPS（本地）**，双击**RADIUS 客户端和服务器**，单击**RADIUS 客户端**，然后在详细信息窗格中，双击你想要更改 RADIUS 客户。

    b。 在 RADIUS 客户端**属性**中**地址 \(IP or DNS\)**，键入代理服务器 NPS 新的 IP 地址。

4. 如果你已经将配置 NPS 代理使用 SQL Server 日志记录，验证运行 SQL Server 和 NPS 代理计算机之间的连接仍在正常。

## <a name="verify-configuration-after-renaming-an-nps-server"></a>确认后重命名 NPS 服务器配置

可能情况下，当你需要更改 NPS 服务器或代理，如时重新命名约定设计为你服务器的名称。

如果你改变 NPS 服务器或代理名称时，很需要重新配置 NPS 部署的各部分。 

使用以下常规指南中验证的服务器名称更改不会中断网络的访问权限身份验证、授权或记帐向你提供帮助。

你必须成员**管理员**，或相当，若要执行此过程。

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>若要验证后 NPS 服务器或代理名称的更改的配置

1. 如果 NPS 服务器远程 RADIUS 服务器组中的成员，并且组配置了计算机名称，而不是 IP 地址，请重新新 NPS 服务器名称远程 RADIUS 服务器组配置。

2. 如果在 NPS 服务器部署证书基于身份验证方法、名称的更改使无效服务器证书。 你可以从认证颁发机构管理员请求新的证书，或如果计算机的域成员计算机并将你自动注册证书到域成员，你可以恢复组策略，以便获得新通过自动注册证书。 若要恢复组策略：

    。 打开 Command Prompt 或 Windows PowerShell。

    b。 键入**gpupdate**，然后按 ENTER。


3. 新的服务器证书后，可以请求 CA 管理员取消旧证书。 

     旧证书已被吊销后，NPS 继续使用它，直到过期旧证书。 默认情况下，旧的证书可以最大的一周 10 小时时间仍然有效。 此时段可能不同，具体取决于是否证书吊销列表 (CRL) 过期并且传输层安全性 (TLS) 缓存时间到期已修改从其默认设置。 默认 CRL 到期是一周;默认 TLS 缓存到期是 10 小时时间。 

     如果你想要配置 NPS 立即使用新的证书，但是，你可以手动重新配置使用新的证书网络策略。

4. 在旧的证书过期后，将 NPS 自动开始使用新的证书。 

5. 如果你已经将配置 NPS 服务器使用 SQL Server 日志记录，验证的计算机运行的 SQL Server 和 NPS 服务器之间的连接仍在正常。

