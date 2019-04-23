---
title: 故障排除主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851368"
---
# <a name="troubleshooting-guarded-hosts"></a>故障排除受保护的主机

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍部署或运行在受保护的构造中的受保护的 HYPER-V 主机时遇到的常见问题的解决方法。
如果您不确定的第一个尝试运行您的问题的性质[受保护的 fabric 诊断](guarded-fabric-troubleshoot-diagnostics.md)上的 HYPER-V 主机来缩小可能会导致。

## <a name="guarded-host-feature"></a>受保护的主机功能

如果您遇到问题的 HYPER-V 主机，首先确保**主机保护者 HYPER-V 支持**安装功能。
如果没有此功能，HYPER-V 主机将会丢失某些关键的配置设置和软件，这使其将证明和预配受防护的 Vm。

若要检查是否已安装该功能，使用服务器管理器或在提升的 PowerShell 窗口中运行以下命令：

```powershell
Get-WindowsFeature HostGuardian
```

如果未安装该功能，请使用以下 PowerShell 命令来安装它：

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>证明失败

如果主机未通过证明与主机保护者服务，它将不能运行受防护的 Vm。
输出[Get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914500.aspx)该主机上将显示在该主机证明的失败原因有关的信息。

下表介绍了可能会出现在值**AttestationStatus**字段和潜在的后续步骤，如果适用。

AttestationStatus         | 说明
--------------------------|------------
过期                   | 主机通过证明以前，但它已签发运行状况证书已过期。 确保主机和 HGS 时间保持同步。
InsecureHostConfiguration | 主机未通过证明，因为它不符合配置 HGS 的证明策略。 有关详细信息，请查阅 AttestationSubStatus 表。
NotConfigured             | 主机未配置为使用 HGS 的证明和密钥保护。 它是改为配置为本地模式。 如果此主机处于受保护的构造，使用[集 HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) ，让它针对 HGS 服务器 Url。
传递                    | 主机通过证明。
TransientError            | 最后一次证明尝试因网络、 服务或其他临时错误而失败。 重试上一操作。
TpmError                  | 主机无法完成其最后一次证明尝试由于你的 TPM 错误。 有关详细信息，请参阅 TPM 日志。
UnauthorizedHost          | 主机未通过证明，因为它未授权运行受防护的 Vm。 请确保该主机属于受信任的 HGS 运行受防护的 Vm 的安全组。
Unknown                   | 主机不被尝试尚未与 HGS 证明。


当**AttestationStatus**报告为**InsecureHostConfiguration**，将在中填充一个或多个原因**AttestationSubStatus**字段。
下表介绍 AttestationSubStatus，以及如何解决该问题的提示可能的值。

AttestationSubStatus       | 这意味着和要执行的操作
---------------------------|-------------------------------
BitLocker                  | 主机的 OS 卷未由 BitLocker 加密。 若要解决此问题，[启用 BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) OS 卷上或[禁用 HGS 上的 BitLocker 策略](guarded-fabric-manage-hgs.md#review-attestation-policies)。
CodeIntegrityPolicy        | 主机未配置为使用代码完整性策略或未使用受信任的 HGS 服务器的策略。 请确保代码完整性策略已配置，已重新启动主机，并且该策略向 HGS 服务器注册。 有关详细信息，请参阅[创建并应用代码完整性策略](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy)。
DumpsEnabled               | 主机配置为允许故障转储或实时内存转储，这不允许由 HGS 策略。 若要解决此问题，禁用在主机上的转储。
DumpEncryption             | 主机配置为允许故障转储或实时内存转储，但不会加密这些转储。 禁用转储在主机上的或[配置转储加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
DumpEncryptionKey          | 主机配置为允许与加密转储，但不是使用已知的 HGS 的证书来加密它们。 若要解决此问题，[更新转储加密密钥](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)在主机上或[向 HGS 注册密钥](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
FullBoot                   | 主机从睡眠状态或休眠状态恢复。 重新启动主机，以便清除、 完全启动。
HibernationEnabled         | 主机配置为允许未加密的休眠文件，这不允许由 HGS 策略休眠状态。 禁用休眠状态并重新启动主机，或[配置转储加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
HypervisorEnforcedCodeIntegrityPolicy | 主机未配置为使用虚拟机监控程序强制执行的代码完整性策略。 确保代码完整性是启用、 配置，并且由虚拟机监控程序强制执行。 请参阅[Device Guard 部署指南](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)有关详细信息。
Iommu                      | 主机的虚拟化基于安全功能未配置为要求 IOMMU 设备以防止直接内存访问攻击，所需的 HGS 策略。 验证主机有 IOMMU，它启用，且该 Device Guard 是[配置为要求 DMA 保护](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard)时启动 VBS。
PagefileEncryption         | 在主机上未启用页面文件加密。 若要解决此问题，请运行`fsutil behavior set encryptpagingfile 1`若要启用页面文件加密。 有关详细信息，请参阅[fsutil 行为](https://technet.microsoft.com/library/cc785435.aspx)。
SecureBoot                 | 安全启动此主机上未启用，或不使用 Microsoft 安全启动模板。 [启用安全启动](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot)与 Microsoft 安全启动模板，若要解决此问题。
SecureBootSettings         | 在此主机上的 TPM 基线不匹配任何的 HGS 由受信任。 通过安装新硬件或软件更改你 UEFI 启动颁发机构、 DBX 变量、 调试标志或自定义安全启动策略可以发生该错误。 如果您信任当前硬件、 固件和软件配置此计算机，你可以[捕获新的 TPM 基线](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware)并[向 HGS 注册](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
TcgLogVerification         | 无法获得或验证 TCG 日志 （TPM 基线）。 这可能指示主机的固件、 TPM 或其他硬件组件出现了问题。 如果您的主机配置为在启动 Windows 之前尝试 PXE 启动，过时的网络启动程序 (NBP) 也会导致此错误。 请确保所有 Nbp 都是最新的启用 PXE 启动。
VirtualSecureMode          | 主机上未运行虚拟化基于安全功能。 请确保启用 VBS 和你的系统满足已配置[平台的安全功能](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)。 请查阅[Device Guard 文档](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)VBS 要求有关的详细信息。
