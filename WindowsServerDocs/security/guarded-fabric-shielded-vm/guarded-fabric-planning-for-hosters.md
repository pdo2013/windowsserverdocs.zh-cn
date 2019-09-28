---
title: 适用于托管商的受保护的构造和受防护 VM 规划指南
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7e0ffb24f888760df58711a867b7ac0ba2650647
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386531"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>适用于托管商的受保护的构造和受防护 VM 规划指南

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍在构造中启用受防护的虚拟机所需执行的规划决策。 无论升级现有 Hyper-v 构造还是创建新构造，运行受防护的 Vm 都包含两个主要组件：

- 主机保护者服务（HGS）提供证明和密钥保护，使你可以确保受防护的 Vm 仅在已批准和正常运行的 Hyper-v 主机上运行。 
- 受防护的和正常运行的 Hyper-v 主机（在其上可以运行受防护的 Vm （和常规 Vm）），这些主机称为受保护的主机。

![HGS 和受保护的主机](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>决策 #1：构造中的信任级别

如何实现主机保护者服务和受保护的 Hyper-v 主机主要取决于你在构造中要实现的信任度。 信任的强度由证明模式控制。 有两个互相排斥的选项：

1. 受 TPM 信任的证明

    如果你的目标是帮助保护虚拟机免受恶意管理员的攻击，则使用受 TPM 信任的证明。 此选项非常适合于企业环境中的多租户托管方案以及高价值资产，例如 SQL 或 SharePoint 等域控制器或内容服务器。
    在允许主机运行受防护的 Vm 之前，会测量受管理程序保护的代码完整性（要求 HVCI）策略，并强制其有效性。 

2. 主机密钥证明

    如果你的要求主要由要求静态和正在进行加密的虚拟机的符合性驱动，则你将使用主机密钥证明。 此选项非常适用于常规用途的数据中心，你可以在其中熟悉 Hyper-v 主机和构造管理员对虚拟机的来宾操作系统进行日常维护和操作的访问。 

    在此模式下，构造管理员只负责确保 Hyper-v 主机的运行状况。 
    由于在确定不允许或不允许运行的情况下，HGS 不起作用，因此恶意软件和调试器将按设计方式工作。 
    
    但是，因为 VM 的工作进程（VMWP）是受保护的进程轻型（PPL），所以，尝试直接附加到进程的调试器（如 WinDbg）被阻止用于受防护的 Vm。 
    不会阻止其他调试技术（例如 LiveKd 使用的方法）。 
    与受防护的 Vm 不同，支持加密的 Vm 的工作进程不会作为 PPL 运行，因此，诸如 tcm.exe 这样的传统调试器将继续正常工作。

    从 Windows Server 2019 开始，已弃用了名为 "管理-受信任的证明（基于 Active Directory）" 的类似证明模式。 

你选择的信任级别将规定 Hyper-v 主机的硬件要求以及你在构造上应用的策略。 如有必要，你可以使用现有的硬件和受信任的管理员证明来部署受保护的构造，然后在硬件升级后将其转换为受 TPM 信任的证明，并需要增强构造安全性。

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>决策 #2：现有 Hyper-v 结构与新的单独 Hyper-v 构造

如果你有现成的构造（Hyper-v 或其他），很可能会使用它来运行受防护的 Vm 以及常规 Vm。 有些客户选择将受防护的 Vm 集成到其现有的工具和构造中，而另一些客户会出于业务原因将其隔离开来。

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>主机保护者服务的 HGS 管理员规划

在高度安全的环境中部署主机保护者服务（HGS），无论是在专用物理服务器、受防护的 VM、隔离的 Hyper-v 主机上的 VM 上（与它所保护的构造分开），还是通过使用不同的 Azure 以逻辑方式分隔的环境中订阅.   

| 区域 | 详细信息 |
|------|---------|
| 安装要求 | <ul><li>一台服务器（建议使用三节点群集实现高可用性）</li><li>对于回退，至少需要两个 HGS 服务器</li><li>服务器可以是虚拟的，也可以是物理的（建议使用 TPM 2.0 的物理服务器;还支持 TPM 1.2）</li><li>Windows Server 2016 或更高版本的服务器核心安装</li><li>允许 HTTP 或[回退配置](guarded-fabric-manage-branch-office.md#fallback-configuration)的构造的网络线路</li><li>建议使用 HTTPS 证书进行访问验证</li></ul> |
| 大小调整 | 每个中间大小（8核/4 GB） HGS server 节点可以处理 1000 Hyper-v 主机。 |
| 管理 | 指定将管理 HGS 的特定人员。 它们应该独立于构造管理员。 为了进行比较，可以将 HGS 群集视为与证书颁发机构（CA）相同的方式，如管理隔离、物理部署和总体安全敏感度级别。 |
| 主机保护者服务 Active Directory | 默认情况下，HGS 安装自己的用于管理的内部 Active Directory。 这是一个自包含的自管理林，是用于帮助将 HGS 与构造隔离的推荐配置。<br><br>如果你已经有一个高度特权的 Active Directory 林用于隔离，则可以使用该林，而不是使用 HGS 默认林。 不能将 HGS 加入到与 Hyper-v 主机或构造管理工具相同的林中的域，这一点很重要。 这样做可能会允许构造管理员获得对 HGS 的控制。 |
| 灾难恢复 | 有以下三个选项：<br><ol><li>在每个数据中心安装单独的 HGS 群集，并授权受防护的 Vm 在主数据中心和备份数据中心运行。 这样就无需跨 WAN 扩展群集，并允许你隔离虚拟机，使其仅在指定的站点中运行。</li><li>在两个（或多个）数据中心之间的 stretch 群集上安装 HGS。 如果 WAN 停机，则会提供复原功能，但会推送故障转移群集的限制。 无法将工作负荷隔离到一个站点;授权在一个站点中运行的 VM 可以在任何其他站点上运行。</li><li>将 Hyper-v 主机注册为其他 HGS 作为故障转移。</li></ol>还应通过导出其配置来备份每个 HGS，以便始终可以在本地恢复。 有关详细信息，请参阅[HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate)和[HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate)。 |
| 主机保护者服务密钥 | 主机保护者服务使用两个非对称密钥对，每个密钥对都由一个 SSL 证书表示。 有两个选项可用于生成这些密钥：<br><ol><li>内部证书颁发机构-可以使用内部 PKI 基础结构生成这些密钥。 这适用于数据中心环境。</li><li>公开信任的证书颁发机构–使用从公开信任的证书颁发机构获取的一组密钥。 这是托管商应使用的选项。</li></ol>请注意，虽然可以使用自签名证书，但不建议将其用于概念证明实验室以外的部署方案。<br><br>除了拥有 HGS 密钥以外，主机托管服务还可以使用 "自带密钥"，其中租户可以提供自己的密钥，使某些（或所有）租户可以拥有自己的特定 HGS 密钥。 此选项适用于托管商，可为租户提供带外进程来上载其密钥。 |
| 主机保护者服务密钥存储 | 为了获得最高的安全性，我们建议仅在硬件安全模块（HSM）中创建并存储 HGS 密钥。 如果未使用 Hsm，强烈建议在 HGS 服务器上应用 BitLocker。 |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>针对受保护主机的结构管理员规划

| 区域 | 详细信息 |
|------|---------|
| 硬件 | <ul><li>主机密钥证明：你可以使用任何现有硬件作为受保护的主机。 有一些例外情况（为了确保主机可以使用从 Windows Server 2016 开始的新安全机制，请参阅[兼容硬件，其中包含基于 Windows server 2016 虚拟化的代码完整性保护](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。</li><li>受信任的 TPM 证明：你可以使用具有[硬件保证附加限定](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance)的任何硬件（只要它已正确配置）（请参阅[与受防护的 Vm 兼容的服务器配置和代码完整性的基于虚拟化的保护）](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)对于特定配置）。 这包括 TPM 2.0 和 UEFI 版本 2.3.1 c 及更高版本。</li></ul> |
| OS | 建议使用 Hyper-v 主机操作系统的 "服务器核心" 选项。 |
| 性能影响 | 根据性能测试，我们预计运行受防护的 Vm 和未受防护的 Vm 之间的密度大致为 5%。 这意味着，如果给定的 Hyper-v 主机可以运行20个未受防护的 Vm，则会预计它可以运行19个受防护的 Vm。<br><br>请确保根据典型工作负荷验证大小。 例如，可能会有一些离群的离线，它们会进一步影响密度差异。 |
| 分支机构注意事项 | 从 Windows Server 版本1709开始，你可以为分支机构中作为受防护的 VM 本地运行的虚拟化 HGS 服务器指定回退 URL。 当分支机构失去到数据中心内的 HGS 服务器的连接时，可以使用回退 URL。 在以前版本的 Windows Server 上，分支机构中运行的 Hyper-v 主机需要连接到主机保护者服务才能开机或实时迁移受防护的 Vm。 有关详细信息，请参阅[分支机构注意事项](guarded-fabric-manage-branch-office.md)。 |
