---
title: 将受保护的结构升级到 Windows Server 2019
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 621d4175894bb235475155507a896a251dec0f7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386337"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>将受保护的结构升级到 Windows Server 2019

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本文介绍将现有受保护的构造从 Windows Server 2016、Windows Server 版本1709或 Windows Server 版本1803升级到 Windows Server 2019 所需的步骤。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 中的新增功能

在 Windows Server 2019 上运行受保护的构造时，可以利用几项新功能：

**主机密钥证明**是最新的证明模式，旨在使 hyper-v 主机不具备可用于 tpm 证明的 tpm 2.0 设备时更轻松地运行受防护的 vm。 主机密钥证明使用密钥对通过 HGS 对主机进行身份验证，消除了将主机加入到 Active Directory 域的要求，消除了 HGS 与企业林之间的 AD 信任，以及减少了打开的防火墙端口的数量。 主机密钥证明替代了 Windows Server 2019 中弃用 Active Directory 证明。

**V2 证明版本**-若要在将来支持主机密钥证明和新功能，我们已向 HGS 引入了版本控制。 Windows Server 2019 上的 HGS 全新安装将导致服务器使用 v2 证明，这意味着它可以支持 Windows Server 2019 主机的主机密钥证明，并且仍支持 Windows Server 2016 上的 v1 主机。 在手动启用 v2 之前，将保留到2019的就地升级。 大多数 cmdlet 现在具有-HgsVersion 参数，该参数可用于指定是否要使用旧版或新式认证策略。

**支持 linux 受防护的 vm** -运行 Windows Server 2019 的 hyper-v 主机可以运行 linux 受防护的 vm。 虽然 Windows Server 版本1709中已推出 Linux 防护 Vm，但 Windows Server 2019 是第一次长期服务通道，可支持它们。

**分支机构改进**-我们使你可以更轻松地在分支机构中运行受防护的 vm，并在 hyper-v 主机上支持脱机受防护的 vm 和回退配置。

**TPM 主机绑定**-对于最安全的工作负荷，你希望受防护的 VM 只能在创建它的第一台主机上运行，而不能在另一台主机上运行，你现在可以使用主机的 TPM 将 VM 绑定到该主机。 这适用于特权访问工作站和分支机构，而不是需要在主机之间迁移的常规数据中心工作负载。

## <a name="compatibility-matrix"></a>兼容性矩阵

在将受保护的构造升级到 Windows Server 2019 之前，请查看以下兼容性矩阵，查看是否支持你的配置。

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper-v 主机** | 支持 | 支持<sup>1</sup>|
|**WS2019 Hyper-v 主机** | 不支持的<sup>2</sup> | 支持|

<sup>1</sup>个 windows server 2016 主机只能使用 v1 认证协议来证明 windows SERVER 2019 HGS 服务器。 Windows Server 2016 主机不支持在 v2 证明协议中独占使用的新功能，包括主机密钥证明。

<sup>2</sup> Microsoft 意识到阻止 windows server 2019 主机使用 TPM 证明的问题无法成功地证明 windows SERVER 2016 HGS 服务器。 此限制将在将来的 Windows Server 2016 更新中得到解决。

## <a name="upgrade-hgs-to-windows-server-2019"></a>将 HGS 升级到 Windows Server 2019

建议在升级 Hyper-v 主机之前将 HGS 群集升级到 Windows Server 2019，以确保所有主机（无论是运行 Windows Server 2016 还是2019）都能继续成功证明。

升级 HGS 群集将要求你在升级时暂时从群集中删除一个节点。 这会降低群集的容量以响应来自 Hyper-v 主机的请求，并可能会导致响应时间变慢或租户的服务中断。 升级 HGS 服务器之前，请确保有足够的容量来处理证明和密钥发布请求。

若要升级 HGS 群集，请在群集的每个节点上执行以下步骤，一次一个节点：

1.  通过在提升的 PowerShell 提示符下运行`Clear-HgsServer` ，从群集中删除 HGS 服务器。 此 cmdlet 将从故障转移群集中删除 HGS 复制的存储、HGS 网站和节点。
2.  如果 HGS 服务器是域控制器（默认配置），则需要在要升级的第`adprep /forestprep`一个`adprep /domainprep`节点上运行和，以便为 OS 升级准备域。 有关详细信息，请参阅[Active Directory 域服务升级文档](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths)。
3.  执行到 Windows Server 2019 的[就地升级](../../get-started-19/install-upgrade-migrate-19.md)。
4.  运行[HgsServer](guarded-fabric-configure-additional-hgs-nodes.md)将节点加入到群集。

将所有节点升级到 Windows Server 2019 后，可以选择将 HGS 版本升级到 v2 以支持主机密钥证明等新功能。

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>将 Hyper-v 主机升级到 Windows Server 2019

将 Hyper-v 主机升级到 Windows Server 2019 之前，请确保已将你的 HGS 群集升级到 Windows Server 2019，并已将所有 Vm 从 Hyper-v 服务器中删除。

1.  如果在服务器上使用 Windows Defender 应用程序控制代码完整性策略（使用 TPM 证明时总是如此），请确保在尝试升级服务器之前策略处于审核模式或禁用状态。 [了解如何禁用 WDAC 策略](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  按照[Windows server 升级内容](../../upgrade/upgrade-overview.md)中的指导将你的主机升级到 Windows server 2019。 如果 Hyper-v 主机是故障转移群集的一部分，请考虑使用[群集操作系统滚动升级](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。
3.  [测试并重新启用](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies)Windows Defender 应用程序控制策略（如果已在升级前启用了一个）。
4.  运行`Get-HgsClientConfiguration`检查**IsHostGuarded = True**，这意味着主机已成功通过你的 HGS 服务器传递证明。
5.  如果使用的是 TPM 证明，则在升级到 pass 证明后，可能需要[重新捕获 TPM 基线或代码完整性策略](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md)。
6.  请在主机上重新开始运行受防护的 Vm！

## <a name="switch-to-host-key-attestation"></a>切换到主机密钥证明

如果当前正在运行基于 Active Directory 的证明并且要升级到主机密钥证明，请按照下面的步骤操作。 请注意，基于 Active Directory 的证明在 Windows Server 2019 中已弃用，可能会在将来的版本中删除。

1.  通过运行以下命令，确保你的 HGS 服务器在 v2 证明模式下运行。 即使将 HGS 服务器升级到 v2，现有 v1 主机仍将继续证明。

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  从每个 Hyper-v 主机[生成主机密钥](guarded-fabric-create-host-key.md)，并向 HGS 注册这些密钥。 因为 HGS 仍在 Active Directory 模式下运行，您将收到一条警告消息，指出新的主机密钥不会立即生效。 这是有意的，因为你不希望更改为主机密钥模式，直到所有主机都能成功证明主机密钥。

3.  为每个主机注册了主机密钥后，即可将 HGS 配置为使用主机密钥证明模式：

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    如果遇到主机密钥模式问题，需要恢复到基于 Active Directory 的证明，请在 HGS 上运行以下命令：

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```