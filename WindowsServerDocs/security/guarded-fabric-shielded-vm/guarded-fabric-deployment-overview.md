---
title: 受保护的结构部署快速入门
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 48ac73e79709f28816ea9eff35361bd54710c66e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870529"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>受保护的结构部署快速入门

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍了受保护的结构、其要求和部署过程的摘要。 有关详细的部署步骤，请参阅为[受保护的主机和受防护的 Vm 部署主机保护者服务](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)。

喜欢视频？ 请参阅 Microsoft 虚拟学院课程[使用 Windows Server 2016 部署受防护的 vm 和受保护的构造](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)。

## <a name="what-is-a-guarded-fabric"></a>什么是受保护的构造

_受保护的构造_是一种 Windows Server 2016 hyper-v 构造，能够保护租户工作负荷，防止检测、窃取和篡改主机上运行的恶意软件以及系统管理员。 这些虚拟化的租户工作负荷（在静止和正在进行中）均称为受_防护的 vm_。 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>受保护的构造的要求是什么

受保护的结构的要求包括：

- **用于运行不受恶意软件攻击的受防护 Vm 的位置。**

    这些主机称为_受保护的主机_。 
    受保护的主机是 Windows Server 2016 Datacenter edition Hyper-v 主机，只有在这些主机可以将受防护的 Vm 证明其在称为主机保护者服务（HGS）的已知受信任状态下运行时，才可以运行受防护的 Vm。 
    HGS 是 Windows Server 2016 中的新服务器角色，通常部署为三节点群集。 

- **验证主机是否处于正常运行状态的方法。**

    HGS 执行_证明_，其中衡量受保护主机的运行状况。

- **安全地将密钥释放到正常主机的过程。**

    HGS 执行_密钥保护和密钥发布_，其中，将密钥释放回正常的主机。

- **用于自动进行受防护 Vm 的安全设置和托管的管理工具。**

    还可以选择将这些管理工具添加到受保护的结构中：

    - System Center 2016 Virtual Machine Manager （VMM）。 建议使用 VMM，因为它提供的附加管理工具超出了使用 Hyper-v 和受保护的构造工作负荷所提供的 PowerShell cmdlet。
    - System Center 2016 Service Provider Foundation （SPF）。 这是 Windows Azure Pack 与 VMM 之间的 API 层，以及使用 Windows Azure Pack 的先决条件。
    - Windows Azure Pack 提供了一个良好的图形 web 界面，用于管理受保护的构造和受防护的 Vm。 

在实践中，必须提前制定一项决策：受保护构造使用的[证明模式](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution)。 有两种方式：两个互相排斥的模式： HGS 可以衡量 Hyper-v 主机是否正常。 初始化 HGS 时，需要选择模式：  

- 主机密钥证明或密钥模式的安全性较低，但更易于采用  
- 基于 TPM 的证明（或 TPM 模式）更安全，但需要更多配置和特定硬件

如有必要，你可以使用已升级到 Windows Server 2019 Datacenter edition 的现有 Hyper-v 主机在密钥模式下部署，然后在支持服务器硬件（包括 TPM 2.0）时将其转换为更安全的 TPM 模式。 

既然您已经了解了这些部分，接下来让我们了解一下部署模型的示例。

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>如何从当前 Hyper-v 构造连接到受保护的构造

假设你有一个现有的 Hyper-v fabric，如以下图中的 Contoso.com，并且你想要构建一个 Windows Server 2016 受保护的构造。

![现有 Hyper-v 构造](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>步骤 1：部署运行 Windows Server 2016 的 Hyper-v 主机 

Hyper-v 主机需要运行 Windows Server 2016 Datacenter edition 或更高版本。 如果要升级主机，可以从标准版[升级](https://technet.microsoft.com/windowsserver/dn527667.aspx)到数据中心版。

![升级 Hyper-v 主机](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>步骤 2：部署主机保护者服务（HGS）

然后安装 HGS 服务器角色并将其部署为三节点群集，例如下图所示的 relecloud.com 示例。 这需要三个 PowerShell cmdlet：

- 若要添加 HGS 角色，请使用`Install-WindowsFeature` 
- 若要安装 HGS，请使用`Install-HgsServer` 
- 若要使用所选证明模式初始化 HGS，请使用`Initialize-HgsServer` 

如果现有的 Hyper-v 服务器不满足 TPM 模式的先决条件（例如，它们没有 TPM 2.0），则可以使用基于管理员的证明（AD 模式）初始化 HGS，这需要与 fabric 域 Active Directory 信任。 

在我们的示例中，假设 Contoso 最初在 AD 模式下部署，以便立即满足合规性要求，并计划在可以购买合适的服务器硬件后，转换为基于 TPM 的更安全的证明。 

![安装 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>步骤 3：提取标识、硬件基线和代码完整性策略

从 Hyper-v 主机提取标识的过程取决于所使用的证明模式。

对于 AD 模式，主机的 ID 是其已加入域的计算机帐户，该帐户必须是 fabric 域中指定安全组的成员。
指定组中的成员身份是主机是否正常运行的唯一决定。 

在此模式下，构造管理员只负责确保 Hyper-v 主机的运行状况。 由于在确定不允许或不允许运行的情况下，HGS 不起作用，因此恶意软件和调试器将按设计方式工作。 

但是，因为 VM 的工作进程（VMWP）是受保护的进程轻型（PPL），所以，尝试直接附加到进程的调试器（如 WinDbg）被阻止用于受防护的 Vm。
不会阻止其他调试技术（例如 LiveKd 使用的方法）。 与受防护的 Vm 不同，支持加密的 Vm 的工作进程不会作为 PPL 运行，因此，诸如 tcm.exe 这样的传统调试器将继续正常工作。

换句话说，用于 TPM 模式的严格验证步骤不会以任何方式用于 AD 模式。

对于 TPM 模式，需要执行以下三项操作： 

1.  来自每个 Hyper-v 主机上的 TPM 2.0 的_公共认可密钥_（或_EKpub_）。 若要捕获 EKpub，请`Get-PlatformIdentifier`使用。 
2.  _硬件基准_。 如果每个 Hyper-v 主机都是相同的，则需要使用单个基线。 如果不是这样，则需要为每个硬件类别都有一个。 基线采用可信计算组日志文件的形式，或 TCGlog。 TCGlog 包含主机在 UEFI 固件中通过内核进行的所有操作，最高可为主机完全启动的位置。 若要捕获硬件基准，请安装 Hyper-v 角色和主机保护者 Hyper-v 支持功能并使用`Get-HgsAttestationBaselinePolicy`。 
3.  _代码完整性策略_。 如果每个 Hyper-v 主机都是相同的，则需要使用单个 CI 策略。 如果不是这样，则需要为每个硬件类别都有一个。 Windows Server 2016 和 Windows 10 都具有新形式的 CI 策略强制，称为 "_虚拟机监控程序-强制代码完整性" （要求 hvci）_ 。 要求 HVCI 提供强强制，并确保仅允许主机运行受信任的管理员允许其运行的二进制文件。 这些说明包装在添加到 HGS 的 CI 策略中。 在允许运行受防护的 Vm 之前，HGS 会测量每个主机的 CI 策略。 若要捕获 CI 策略，请`New-CIPolicy`使用。 然后，必须使用`ConvertFrom-CIPolicy`将策略转换为其二进制形式。

![提取标识、基线和 CI 策略](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

这就是，受保护的构造是根据运行它的基础结构生成的。  
现在，你可以创建受防护的 VM 模板磁盘和防护数据文件，以便可以简单安全地预配受防护的 Vm。 

## <a name="step-4-create-a-template-for-shielded-vms"></a>步骤 4：为受防护的 Vm 创建模板

受防护的 VM 模板通过在已知的可信时间点创建磁盘签名来保护模板磁盘。 
如果以后恶意软件感染了模板磁盘，则其签名将不同于受安全防护的 VM 预配过程检测到的原始模板。 
受防护的模板磁盘是通过运行**受防护的模板磁盘创建向导**或`Protect-TemplateDisk`普通模板磁盘创建的。 

每个包含于[Windows 10 的远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)中的**受防护的 VM 工具**功能。
下载 RSAT 后，运行以下命令以安装**受防护的 VM 工具**功能：

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

可信管理员（如构造管理员或 VM 所有者）将需要一个证书（通常由托管服务提供商提供）来对 VHDX 模板磁盘进行签名。 

![受防护的模板磁盘向导](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

磁盘签名是通过虚拟磁盘的 OS 分区计算得出的。
如果 OS 分区发生了任何更改，签名也将更改。
这样，用户就可以通过指定适当的签名来强标识它们信任哪些磁盘。

在开始之前，请查看[模板磁盘要求](guarded-fabric-create-a-shielded-vm-template.md)。 

## <a name="step-5-create-a-shielding-data-file"></a>步骤 5：创建防护数据文件 

屏蔽数据文件（也称为 pdk 文件）捕获关于虚拟机的敏感信息，例如管理员密码。 

![屏蔽数据](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

防护数据文件还包括受防护的 VM 的安全策略设置。 创建防护数据文件时，必须选择以下两个安全策略之一：

- 屏蔽
   
    最安全的选项，可消除许多管理攻击媒介。

- 支持加密

    还提供了可加密 VM 的符合性优点的较低级别的保护，但允许 Hyper-v 管理员执行一些操作，如使用 VM 控制台连接和 PowerShell Direct。 

    ![支持新的加密 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

可以添加可选的管理组件，例如 VMM 或 Windows Azure Pack。 如果要在不安装这些组件的情况下创建 VM，请参阅分步[创建未受](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/)保护的 vm。

## <a name="step-6-create-a-shielded-vm"></a>步骤 6：创建受防护的 VM

创建受防护的虚拟机的方式与常规虚拟机不同。 在 Windows Azure Pack 中，体验比创建常规 VM 更为简单，因为你只需提供名称、防护数据文件（包含其他专用化信息）和 VM 网络。 

![Windows Azure Pack 中的新受防护的 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>下一步

> [!div class="nextstepaction"]
> [HGS 必备组件](guarded-fabric-prepare-for-hgs.md)
