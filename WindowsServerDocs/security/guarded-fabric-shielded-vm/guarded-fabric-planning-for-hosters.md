---
title: 受保护的构造和受防护的 VM 规划托管商的指南
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f280fbe682ebf706ce6ea5b53ea8af5e6f39d75d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857808"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>受保护的构造和受防护的 VM 规划托管商的指南

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍将需要对启用受防护的虚拟机在结构上运行的规划决策。 是否升级现有的 HYPER-V 构造或创建新的 fabric 时，运行受防护的 Vm 包含两个主要组件：

- 主机保护者服务 (HGS) 提供证明和密钥保护，以便你可以确保受防护的 Vm 将仅在已批准和正常运行的 HYPER-V 主机上运行。 
- 在其可以运行受防护的 Vm （和常规 Vm） 的已批准和正常运行的 HYPER-V 主机，这些参数称为受保护的主机。

![HGS 和受保护的主机](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>决策 #1:信任在构造中的级别

如何实现主机保护者服务和受保护的 HYPER-V 主机将取决于主要的强度想要实现在结构中的信任。 证明模式由管理信任的强度。 有两个互斥的选项：

1. 受信任的 TPM 证明

    如果你的目标是帮助保护虚拟机免受恶意管理员或已损坏的构造，则将使用受信任的 TPM 证明。 此选项适用于多租户托管的情况下也与在企业环境中，如域控制器或 SQL 或 SharePoint 等内容服务器的高价值资产。
    测量受保护的虚拟机监控程序代码完整性 (HVCI) 策略和它们的有效性之前允许运行主机由 HGS 强制执行受防护的 Vm。 

2. 主机密钥证明

    如果您的要求主要取决于需要的符合性虚拟机加密同时静态以及传输的数据，则将使用主机密钥证明。 此选项非常适合常规用途数据中心位置是熟练的 HYPER-V 主机和结构管理员有权访问的日常维护和操作的虚拟机的来宾操作系统。 

    在此模式下，构造管理员完全负责确保 HYPER-V 主机的运行状况。 
    HGS 不起任何作用，在决定什么是或不能运行，因为恶意软件和调试器仍可正常而设计。 
    
    但是，调试器尝试附加到进程 （如 WinDbg.exe) 直接阻止受防护的 Vm 的 VM 的工作进程 (VMWP.exe) 是一个受保护的进程的指示灯 (PPL)。 
    替代调试技术，例如使用的 LiveKd.exe，那些不被阻止。 
    与不同的受防护的 Vm，支持加密的 Vm 的工作进程将不运行 PPL 使传统的调试器喜欢 WinDbg.exe 将继续正常工作。

    类似的证明模式名为受信任的管理员证明 （基于 Active Directory） 与 Windows Server 2019 从开始已弃用。 

你选择的信任级别将决定您的 HYPER-V 主机，以及在构造应用的策略的硬件要求。 如有必要，可以部署使用现有硬件和受信任的管理员证明您受保护的构造，然后将其转换为受信任的 TPM 证明，升级硬件和需要增强 fabric 安全性时。

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>决策 #2:与新的独立 HYPER-V 构造的现有 HYPER-V 构造

如果有现有的 fabric （HYPER-V 或其他方式），它是很有可能，可以使用它来运行受防护的 Vm，以及常规的 Vm。 某些客户选择将受防护的 Vm 集成到其现有工具和构造，而其他人用于分隔出于业务原因在构造。

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>HGS 管理员规划主机保护者服务

无论是在受防护的 VM、 HYPER-V 独立主机 （以从正在保护的构造） 或一个逻辑上分隔使用不同的 Azure 上的虚拟机的专用物理服务器上部署在高度安全环境中，主机保护者服务 (HGS)订阅。   

| 区域 | 详细信息 |
|------|---------|
| 安装要求 | <ul><li>一台服务器 （建议使用以实现高可用性三节点群集）</li><li>进行回退，至少两个 HGS 服务器是必需的</li><li>服务器可以是虚拟或物理 （物理服务器具有 TPM 2.0 建议;TPM 1.2 也支持)</li><li>服务器核心安装的 Windows Server 2016 或更高版本</li><li>网络视线，到允许 HTTP 构造或[回退配置](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>建议用于访问验证的 HTTPS 证书</li></ul> |
| 大小调整 | 每个中等规模 (8 核/4 GB) HGS 服务器节点可处理 1,000 的 HYPER-V 主机。 |
| Management | 指定将管理 HGS 特定人员。 它们应该是独立于结构管理员。 有关比较，HGS 群集可以被认为是相同的方式为证书颁发机构 (CA) 管理隔离、 物理部署和整体安全敏感度级别方面。 |
| 主机保护者服务 Active Directory | 默认情况下，HGS 安装管理其自身内部 Active Directory。 这是自包含、 自我管理的林，是推荐的配置，以帮助隔离 HGS 从你的结构。<br><br>如果已有用于隔离高特权 Active Directory 林，你可以使用该林而不是 HGS 默认的林。 请务必 HGS 未加入到域中的 HYPER-V 主机或在结构管理工具所在的林。 执行此操作可能允许构造管理员 HGS 对其进行控制。 |
| 灾难恢复 | 有以下三个选项：<br><ol><li>在每个数据中心中安装单独的 HGS 群集并授权受防护的 Vm，若要在主数据库和备份数据中心中运行。 这就无需通过 WAN 拉伸群集，并允许您将虚拟机隔离，以便它们仅在其指定站点中运行。</li><li>在两个 （或多个） 的数据中心之间拉伸群集上安装 HGS。 如果 WAN 出现故障，但推送故障转移群集的限制，这可以提供复原能力。 不能隔离到一个站点; 的工作负荷在任何其他可以运行授权在一个站点中运行的 VM。</li><li>向另一个 HGS 注册 HYPER-V 主机，为故障转移。</li></ol>此外应通过将其配置导出，以便始终可以恢复本地备份每个 HGS。 有关详细信息，请参阅[导出 HgsServerState](https://technet.microsoft.com/library/mt652164.aspx)并[导入 HgsServerState](https://technet.microsoft.com/library/mt652168.aspx)。 |
| 主机保护者服务密钥 | 主机保护者服务使用两个非对称密钥对，加密密钥和签名密钥，每个所表示的 SSL 证书。 有两个选项来生成这些密钥：<br><ol><li>内部证书颁发机构 – 你可以生成这些密钥使用内部 PKI 基础结构。 这是适用于数据中心环境。</li><li>公开受信任的证书颁发机构 – 使用一组从可公开受信任的证书颁发机构获取的密钥。 这是托管方应使用的选项。</li></ol>请注意，虽然可能要使用自签名的证书，但不建议用于概念证明实验以外的部署方案。<br><br>除了拥有 HGS 密钥，宿主可以使用"自带你自己的密钥，"，租户可以提供其自己的密钥，以便某些 （或所有） 租户可以有其自己特定的 HGS 密钥。 此选项是适用于托管商可提供一个带外过程，用于租户能够上传其密钥。 |
| 主机保护者服务密钥存储 | 有关可能的最强安全，我们建议 HGS 密钥将创建并存储以独占方式在硬件安全模块 (HSM)。 如果不使用 Hsm，强烈建议将 BitLocker 应用 HGS 服务器上。 |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>构造管理员规划的受保护的主机

| 区域 | 详细信息 |
|------|---------|
| 硬件 | <ul><li>主机密钥证明：您可以使用任何现有硬件作为受保护的主机。 有几个例外情况 (若要确保你的主机可以使用新的安全机制从 Windows Server 2016 开始，请参阅[与基于 Windows Server 2016 虚拟化的代码完整性保护兼容的硬件](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。</li><li>受信任的 TPM 证明：可以使用具有任何硬件[硬件保障其他资格](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance)只要正确配置时 (请参阅[服务器配置了符合受防护的 Vm 和基于虚拟化的代码完整性保护](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)的特定配置)。 这包括 TPM 2.0 和 UEFI 2.3.1c 版本及更高版本。</li></ul> |
| 操作系统 | 我们建议使用服务器核心选项的 HYPER-V 主机操作系统。 |
| 性能影响 | 根据性能测试，我们预计约 5%密度的区别，运行受防护的 Vm 和非受防护的 Vm。 这意味着，如果给定的 HYPER-V 主机可以运行 20 个非受防护的 Vm，我们期望它可以运行 19 受防护的 Vm。<br><br>请务必验证与典型的工作负荷的大小调整。 例如，可能有一些出界者密集型面向写入 IO 工作负荷会进一步影响密度差异。 |
| 分支机构注意事项 | 从 Windows Server 1709 版开始，可以指定分支机构中受防护的 VM 作为本地运行的虚拟化 HGS 服务器的回退 URL。 可以使用回退 URL，当分支机构断开连接到数据中心的 HGS 服务器。 在上一版本的 Windows Server 中，在分支机构需求连接中运行主机保护者服务将开机或实时迁移到 HYPER-V 主机受防护的 Vm。 有关详细信息，请参阅[分支 office 注意事项](guarded-fabric-manage-branch-office.md)。 |
