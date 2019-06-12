---
title: 受保护的构造部署的快速入门
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 8e1ef34370b1459cd55705bc0069b49a572de303
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447528"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>受保护的构造部署的快速入门

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题说明什么是受保护的构造，其要求和部署过程的摘要。 有关详细的部署步骤，请参阅[部署主机保护者服务的受保护的主机和受防护的 Vm](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)。

喜欢视频？ 请参阅 Microsoft Virtual Academy 课程[部署受防护的 Vm 和受保护的构造与 Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)。

## <a name="what-is-a-guarded-fabric"></a>什么是受保护的构造

一个_受保护的构造_是 Windows Server 2016 HYPER-V 构造能够保护检查、 盗用和篡改从该主机上运行的恶意软件和系统管理员的租户工作负荷。 这些虚拟化的租户工作负荷，在 rest 和飞行中受保护-称为_受防护的 Vm_。 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>受保护的构造的要求有哪些？

受保护的构造的要求包括：

- **运行受防护的 Vm 免受恶意软件免费。**

    这些被称为_受保护的主机_。 
    受保护的主机都可以运行受防护的 Vm，仅当它们可证实到名为主机保护者服务 (HGS) 的外部权威机构中的已知、 可信状态运行的 Windows Server 2016 Datacenter edition 的 HYPER-V 主机。 
    HGS 是 Windows Server 2016 中的新服务器角色，并通常部署为一个三节点群集。 

- **一种方法来验证主机处于正常状态。**

    执行 HGS_证明_，其中它测量受保护的主机的运行状况。

- **一个安全地释放到正常的主机密钥的过程。**

    执行 HGS_密钥保护和密钥版本_，其中它释放回正常的主机密钥。

- **管理工具来自动执行安全预配和托管的受防护的 Vm。**

    （可选） 可以将这些管理工具添加到受保护的构造：

    - System Center 2016 Virtual Machine Manager (VMM)。 VMM 建议，因为它提供了其他管理工具超出你从使用只是附带的 HYPER-V 和受保护的构造的工作负荷的 PowerShell cmdlet 获取）。
    - System Center 2016 Service Provider Foundation (SPF)。 这是 Windows Azure 包和 VMM 中与使用 Windows Azure 包的必备组件之间的 API 层。
    - Windows Azure Pack 提供了一个图形的良好 web 界面管理受保护的构造和受防护的 Vm。 

在实践中，一个决策必须做得更清楚：[证明模式](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution)使用受保护的构造。 有两种方法 — 两个互相排斥的模式 — 通过其 HGS 可以衡量的 HYPER-V 主机处于正常状态。 在初始化 HGS，需要选择的模式：  

- 主机密钥证明或密钥模式，是不太安全但更轻松地采用  
- 基于 TPM 的证明还是 TPM 模式更安全，但需要更多配置和特定的硬件

如有必要，可以在使用现有的 HYPER-V 主机的已升级到 Windows Server 2019 Datacenter edition 的关键模式下部署，然后将转换为更安全的 TPM 模式提供支持 （包括 TPM 2.0） 的服务器硬件时。 

现在，您知道各组件的是，让我们演练部署模型的一个示例。

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>如何将从当前的 HYPER-V 构造导入受保护的构造

让我们设想以下场景 — 您有现有的 HYPER-V 构造，例如下图中，在 Contoso.com 并且你想要生成的 Windows Server 2016 受保护的构造。

![现有 HYPER-V 构造](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>第 1 步：部署运行 Windows Server 2016 的 HYPER-V 主机 

HYPER-V 主机需要运行 Windows Server 2016 Datacenter edition 或更高版本。 如果要升级的主机，则可以[升级](https://technet.microsoft.com/windowsserver/dn527667.aspx)从标准版到数据中心版。

![升级 HYPER-V 主机](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>步骤 2：部署主机保护者服务 (HGS)

然后安装 HGS 服务器角色，并将其部署为一个三节点群集，如 relecloud.com 示例如下图中所示。 这需要三个 PowerShell cmdlet:

- 若要添加的 HGS 角色，请使用 `Install-WindowsFeature` 
- 若要安装 HGS，请使用 `Install-HgsServer` 
- 若要初始化 HGS 证明你所选模式下的，使用 `Initialize-HgsServer` 

如果您现有的 HYPER-V 服务器不能满足的先决条件 TPM 模式下 （例如，它们没有 TPM 2.0），您可以初始化 HGS 使用管理员基于证明 （AD 模式），这需要 Active Directory 信任关系的 fabric 域。 

在本示例中，我们假设 Contoso 最初在 AD 模式下部署以立即满足合规性要求，以及将转换为更安全的基于 TPM 的证明后可购买适合的客户端硬件的计划。 

![安装 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>步骤 3:提取标识、 硬件基线和代码完整性策略

若要从 HYPER-V 主机中提取的标识的过程取决于正在使用的证明模式。

为 AD 模式 ID 是主机的已加入域的计算机帐户，它必须是主机的 fabric 域中指定的安全组的成员。
所指定的组成员身份是唯一确定主机是否处于正常状态。 

在此模式下，构造管理员完全负责确保 HYPER-V 主机的运行状况。 HGS 不起任何作用，在决定什么是或不能运行，因为恶意软件和调试器仍可正常而设计。 

但是，调试器尝试附加到进程 （如 WinDbg.exe) 直接阻止受防护的 Vm 的 VM 的工作进程 (VMWP.exe) 是一个受保护的进程的指示灯 (PPL)。
替代调试技术，例如使用的 LiveKd.exe，那些不被阻止。 与不同的受防护的 Vm，支持加密的 Vm 的工作进程将不运行 PPL 使传统的调试器喜欢 WinDbg.exe 将继续正常工作。

换言之，用于 TPM 模式下的严格的验证步骤不能以任何方式 AD 模式提供。

对于 TPM 模式，三个操作是必需的： 

1.  一个_公共认可密钥_(或_EKpub_) 从每个 HYPER-V 主机上的 TPM 2.0。 若要捕获 EKpub，使用`Get-PlatformIdentifier`。 
2.  一个_硬件基线_。 如果每个 HYPER-V 主机相同，则单个基线是只需要。 如果它们不是，您将需要一个用于每一类硬件。 基线是可信任计算组日志文件或 TCGlog 形式。 TCGlog 包含主机未从 UEFI 固件，通过内核，直至完全启动主机时的所有内容。 若要捕获硬件基线，请安装 HYPER-V 角色和主机保护者 HYPER-V 支持功能，并使用`Get-HgsAttestationBaselinePolicy`。 
3.  一个_代码完整性策略_。 如果每个 HYPER-V 主机相同，然后单个 CI 策略就足够了。 如果它们不是，您将需要一个用于每一类硬件。 Windows Server 2016 和 Windows 10 这两个具有一种新式的 CI 策略，名为强制_虚拟机监控程序强制执行的代码完整性 (HVCI)_ 。 HVCI 提供严格的强制，并确保主机仅允许运行受信任的管理员允许其运行的二进制文件。 这些说明都包装在添加到 HGS 的 CI 策略。 HGS 测量每个主机的 CI 策略之前它们要允许运行受防护的 Vm。 若要捕获的 CI 策略，请使用`New-CIPolicy`。 然后必须将策略转换为其二进制格式使用`ConvertFrom-CIPolicy`。

![提取标识、 基线和 CI 策略](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

这就是所有 — 生成受保护的构造，用于运行它的基础结构方面。  
现在，你可以创建受防护的 VM 的模板磁盘和屏蔽数据文件，因此受防护的 Vm 可以轻松且安全地设置。 

## <a name="step-4-create-a-template-for-shielded-vms"></a>步骤 4：为受防护的 Vm 创建模板

受防护的 VM 模板保护通过已知可信点在时间中创建磁盘签名模板磁盘。 
如果模板磁盘更高版本感染了恶意软件，它的签名将与原始模板将通过安全的受防护 VM 预配过程检测到这些不同。 
受防护的模板磁盘创建通过运行**受防护的模板磁盘创建向导**或`Protect-TemplateDisk`对常规模板磁盘。 

每个都附带**受防护的 VM 工具**中的功能[远程服务器管理工具适用于 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)。
下载 RSAT 后，运行以下命令来安装**受防护的 VM 工具**功能：

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

高信度的管理员，如构造管理员或 VM 所有者，将需要 （通常由托管服务提供商提供） 的证书进行签名 VHDX 模板磁盘。 

![受防护的模板磁盘向导](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

通过虚拟磁盘的 OS 分区计算磁盘签名。
如果 OS 分区上发生任何更改，也将更改签名。
这允许用户强标识通过指定适当的签名他们信任哪些磁盘。

审阅[模板磁盘要求](guarded-fabric-create-a-shielded-vm-template.md)在开始之前。 

## <a name="step-5-create-a-shielding-data-file"></a>步骤 5：创建防护数据文件 

防护数据文件，也称为.pdk 文件，捕获虚拟机，例如管理员密码等敏感信息。 

![屏蔽数据](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

防护数据文件还包括受防护的 VM 的安全策略设置。 创建防护数据文件时，必须选择两个安全策略之一：

- 受防护的
   
    最安全选项，消除了许多的管理的攻击媒介。

- 支持加密

    较低层的保护，仍能够加密 VM 的符合性优势，但允许 HYPER-V 管理员执行操作，例如使用 VM 控制台连接和 PowerShell Direct。 

    ![支持新的加密的 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

您可以添加可选的管理部分，如 VMM 或 Windows Azure Pack。 如果你想要创建的 VM，而无需安装这些片段，请参阅[分步说明，– 不使用 VMM 创建受防护的 Vm](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/)。

## <a name="step-6-create-a-shielded-vm"></a>步骤 6：创建受防护的 VM

创建受防护的虚拟机不同几乎从常规虚拟机。 在 Windows Azure Pack 中的体验会更容易于创建常规 VM，因为您只需提供一个名称，屏蔽数据文件 （包含专用化信息的其余部分） 和 VM 网络。 

![在 Windows Azure Pack 中的新受防护的 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [HGS 先决条件](guarded-fabric-prepare-for-hgs.md)
