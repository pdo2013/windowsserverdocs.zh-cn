---
title: 将受保护的结构升级到 Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 274bdf027947ffb6fe807d4acd0a3b2174c20e28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867448"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>将受保护的结构升级到 Windows Server 2019

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本文介绍从 Windows Server 2016 中，Windows Server 1709 版或 Windows Server 到 Windows Server 2019 1803年的版本升级现有受保护的构造所需的步骤。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 中的新增功能

在受保护的构造运行于 Windows Server 2019 时，您可以利用多项新功能：

**托管密钥证明**是我们最新的证明模式，旨在使更轻松地运行受防护的 Vm 时的 HYPER-V 主机不具有 TPM 2.0 设备可用的 TPM 证明。 主机密钥证明使用密钥对进行身份验证主机与 HGS，无需要求主机加入到 Active Directory 域，消除 HGS 与企业林结构之间的 AD 信任和降低打开防火墙端口数。 主机密钥证明取代了 Active Directory 证明，在 Windows Server 2019 中已弃用。

**V2 证明版本**-若要支持主机密钥证明和新功能在将来，我们引入了到 HGS 的版本控制。 在 Windows Server 2019 HGS 的全新安装将导致使用 v2 证明，这意味着它可以支持的 Windows Server 2019 主机的主机密钥证明并仍支持 Windows Server 2016 上的 v1 主机的服务器。 2019 的就地升级将在版本 v1 保留，直到您手动启用 v2。 大多数 cmdlet 现在拥有一个-HgsVersion 参数允许你指定你想要与旧的或新式证明策略一起使用。

**适用于 Linux 的支持受防护的 Vm** -运行 Windows Server 2019 Hyper V 主机可以运行受防护的 Linux Vm。 虽然受防护的 Linux Vm 自 Windows Server 1709 版以来已经得到解决，Windows Server 2019 是第一个的 long 类型的值术语维护服务频道版本，对其进行支持。

**分支 office 改进**-我们所做更轻松地支持脱机受防护的 Vm 和回退配置分支机构中的 HYPER-V 主机上运行受防护的 Vm。

**TPM 主机绑定**-对于大多数安全工作负荷，即仅在第一台主机上运行了创建的但其他受防护的 VM，现在可以与使用主机的 TPM 该主机绑定 VM。 这是最适合用于特权的访问工作站和分支机构，而不是，需要先迁移在主机之间的常规数据中心工作负载。

## <a name="compatibility-matrix"></a>兼容性矩阵

在升级到 Windows Server 2019 受保护的构造之前，请查看以下的兼容性矩阵，以查看你的配置是否受支持。

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper V 主机** | 支持 | 支持<sup>1</sup>|
|**WS2019 Hyper V 主机** | 不支持<sup>2</sup> | 支持|

<sup>1</sup> Windows Server 2016 主机仅可证明对 Windows Server 2019 HGS 服务器使用 v1 证明协议。 对于 Windows Server 2016 主机不支持在 v2 证明协议中，包括主机密钥证明，专门提供的新功能。

<sup>2</sup> Microsoft 可察觉的问题而造成无法使用 TPM 证明成功证明对 Windows Server 2016 HGS 服务器从 Windows Server 2019 主机。 此限制将在 Windows Server 2016 的未来更新中解决。

## <a name="upgrade-hgs-to-windows-server-2019"></a>升级到 Windows Server 2019 HGS

我们建议升级到 Windows Server 2019 HGS 群集，然后再升级以确保所有主机，无论它们运行 Windows Server 2016 或 2019，可以都继续成功证明你的 HYPER-V 主机。

升级 HGS 群集需要暂时一个节点从群集中删除一次升级时。 这将减少你的群集的容量，以便响应来自 HYPER-V 主机的请求，并可能会导致响应时间很长或服务中断的租户。 确保有足够的容量来处理证明和密钥版本的请求，然后再升级 HGS 服务器。

若要升级 HGS 群集，群集，每次一个节点的每个节点上执行以下步骤：

1.  通过运行从群集中删除 HGS 服务器`Clear-HgsServer`在提升的 PowerShell 提示符。 此 cmdlet 将从故障转移群集中删除复制的 HGS 应用商店、 HGS 网站和节点。
2.  如果 HGS 服务器是域控制器 （默认配置），你将需要运行`adprep /forestprep`和`adprep /domainprep`正在升级以准备进行 OS 升级域的第一个节点上。 请参阅[Active Directory 域服务升级文档](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths)有关详细信息。
3.  执行[就地升级](../../get-started-19/install-upgrade-migrate-19.md)到 Windows Server 2019。
4.  运行[Initialize HgsServer](guarded-fabric-configure-additional-hgs-nodes.md)以节点重新加入群集。

所有节点已都升级到 Windows Server 2019 后，可以选择升级到 v2 来支持新功能，例如主机密钥证明的 HGS 版本。

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>升级到 Windows Server 2019 的 HYPER-V 主机

在升级到 Windows Server 2019 的 HYPER-V 主机之前，请确保 HGS 群集已升级到 Windows Server 2019，并且已将关闭 HYPER-V 服务器的所有 Vm。

1.  如果你的服务器 （始终的情况下使用 TPM 证明时） 上使用 Windows Defender 应用程序控制代码完整性策略，请确保策略处于审核模式，或尝试将服务器升级之前禁用。 [了解如何禁用 WDAC 策略](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  遵循中的指导[Windows Server 升级中心](http://aka.ms/upgradecenter)升级到 Windows Server 2019 主机。 如果你的 HYPER-V 主机故障转移群集的一部分，请考虑使用[群集操作系统滚动升级](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。
3.  [测试和重新启用](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies)Windows Defender 应用程序控制策略，如果你有一个在升级之前已启用。
4.  运行`Get-HgsClientConfiguration`检查是否**IsHostGuarded = True**，这意味着在主机成功地通过证明与 HGS 服务器。
5.  如果使用 TPM 证明，可能需要[重新捕获的 TPM 基线或代码完整性策略](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md)之后升级以通过认证。
6.  开始运行受防护的 Vm 在主机上再次 ！

## <a name="switch-to-host-key-attestation"></a>切换到托管密钥证明

如果你当前正在运行基于 Active Directory 的证明并想要升级到主机密钥证明，请按照以下步骤。 请注意，基于 Active Directory 的证明在 Windows Server 2019 中已弃用，并且可能在将来的版本中删除。

1.  确保 HGS 服务器在 v2 证明模式下操作，通过运行以下命令。 现有 v1 主机将继续证明即使 HGS 服务器升级到 v2。

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [生成主机密钥](guarded-fabric-create-host-key.md)从每个在 HYPER-V 托管并向 HGS 注册它们。 HGS 仍在 Active Directory 模式下操作，因为你将收到新的主机密钥不是立即生效的警告。 这是有意的不希望更改为主机密钥模式，直到所有的主机可成功证明主机密钥。

3.  一旦已为每个主机注册主机密钥，你可以配置 HGS 使用主机密钥证明模式：

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    如果您遇到主机键模式，并且需要恢复到基于 Active Directory 的证明，HGS 上运行以下命令：

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```