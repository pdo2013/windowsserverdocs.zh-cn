---
title: 主机保护者服务疑难解答
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 0479309efe629d204bdc98fe11a7ccb4447a7369
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940729"
---
# <a name="troubleshooting-guarded-hosts"></a>受保护主机的疑难解答

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍在受保护的构造中部署或运行受保护的 Hyper-v 主机时遇到的常见问题的解决方法。
如果你不确定问题的性质，请首先尝试在 Hyper-v 主机上运行[受保护的构造诊断](guarded-fabric-troubleshoot-diagnostics.md)，以缩小可能的原因。

## <a name="guarded-host-feature"></a>受保护的主机功能

如果你遇到 Hyper-v 主机问题，请首先确保安装了**主机保护者 Hyper-v 支持**功能。
如果没有此功能，Hyper-v 主机将丢失某些关键的配置设置和软件，使其能够通过证明和预配受防护的 Vm。

若要检查是否已安装该功能，请使用服务器管理器或在提升的 PowerShell 窗口中运行以下命令：

```powershell
Get-WindowsFeature HostGuardian
```

如果未安装该功能，请通过以下 PowerShell 命令进行安装：

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>证明失败

如果主机未通过主机保护者服务传递证明，它将无法运行受防护的 Vm。
该主机上的[get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914500.aspx)的输出将显示有关该主机未能通过证明的原因的信息。

下表说明了可能出现在**AttestationStatus**字段中的值和可能的后续步骤（如果适用）。

AttestationStatus         | 说明
--------------------------|------------
过期                   | 主机以前已通过证明，但颁发的健康证书已过期。 确保主机和 HGS 时间同步。
InsecureHostConfiguration | 主机未通过证明，因为它不符合在 HGS 上配置的认证策略。 有关详细信息，请参阅 AttestationSubStatus 表。
NotConfigured             | 主机未配置为使用 HGS 进行证明和密钥保护。 改为将其配置为本地模式。 如果此主机位于受保护的构造中，请使用[get-hgsclientconfiguration](https://technet.microsoft.com/library/dn914494.aspx)为其提供 HGS 服务器的 url。
过来                    | 主机通过证明。
TransientError            | 由于网络、服务或其他暂时性错误，上一次认证尝试失败。 重试上一个操作。
TpmError                  | 由于 TPM 错误，主机无法完成其上一次的认证尝试。 有关详细信息，请参阅 TPM 日志。
UnauthorizedHost          | 主机未通过证明，因为无权运行受防护的 Vm。 确保主机属于 HGS 信任的安全组，以运行受防护的 Vm。
Unknown                   | 宿主尚未尝试与 HGS 证明。

当**AttestationStatus**报告为**InsecureHostConfiguration**时，将在**AttestationSubStatus**字段中填充一个或多个原因。
下表说明了 AttestationSubStatus 的可能值，以及如何解决此问题的提示。

AttestationSubStatus       | 这是什么意思？
---------------------------|-------------------------------
BitLocker                  | 主机的 OS 卷未通过 BitLocker 加密。 若要解决此问题，请在 OS 卷上[启用 bitlocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) ，或[在 HGS 上禁用 bitlocker 策略](guarded-fabric-manage-hgs.md#review-attestation-policies)。
I        | 主机未配置为使用代码完整性策略，或者未使用 HGS 服务器信任的策略。 请确保已配置代码完整性策略，已重新启动主机，并且已将该策略注册到 HGS 服务器。 有关详细信息，请参阅[创建并应用代码完整性策略](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy)。
DumpsEnabled               | 主机配置为允许故障转储或实时内存转储，这是你的 HGS 策略所不允许的。 若要解决此问题，请在主机上禁用转储。
DumpEncryption             | 主机配置为允许故障转储或实时内存转储，但不加密这些转储。 请在主机上禁用转储或[配置转储加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
DumpEncryptionKey          | 该主机配置为允许和加密转储，但不使用 HGS 已知的证书对其进行加密。 若要解决此问题，请在主机上[更新转储加密密钥](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)或将[密钥注册到 HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
FullBoot                   | 主机从睡眠状态或休眠状态中恢复。 重新启动主机以允许进行干净的完全启动。
HibernationEnabled         | 主机配置为允许在不加密休眠文件的情况下进行休眠，而你的 HGS 策略不允许这样做。 禁用休眠并重启主机，或[配置转储加密](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption)。
HypervisorEnforcedCodeIntegrityPolicy | 主机未配置为使用虚拟机监控程序强制执行的代码完整性策略。 验证虚拟机监控程序是否已启用、配置和强制实施代码完整性。 有关详细信息，请参阅[Device Guard 部署指南](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies)。
Iommu                      | 主机的基于虚拟化的安全功能未配置为要求 IOMMU 设备防范直接内存访问攻击，这是因为您的 HGS 策略要求。 验证主机是否有 IOMMU、是否已启用，以及 Device Guard 是否配置为在启动 VBS 时[要求 DMA 保护](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard)。
PagefileEncryption         | 主机上未启用页面文件加密。 若要解决此问题，请运行 `fsutil behavior set encryptpagingfile 1` 以启用页面文件加密。 有关详细信息，请参阅[fsutil 行为](https://technet.microsoft.com/library/cc785435.aspx)。
SecureBoot                 | 此主机上未启用安全启动，或者未使用 Microsoft Secure Boot 模板。 若要解决此问题，请使用 Microsoft Secure Boot 模板[启用安全启动](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot)。
SecureBootSettings         | 此主机上的 TPM 基线与 HGS 信任的任何一个基线不匹配。 如果通过安装新的硬件或软件更改了 UEFI 启动机构、.DBX 变量、调试标志或自定义安全启动策略，则会发生这种情况。 如果信任此计算机的当前硬件、固件和软件配置，则可以[捕获新的 TPM 基线](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware)，并[将其注册到 HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts)。
TcgLogVerification         | 无法获取或验证 TCG 日志（TPM 基线）。 这可能表示主机的固件、TPM 或其他硬件组件有问题。 如果主机配置为在启动 Windows 之前尝试 PXE 启动，则过期的 Net Boot 程序（NBP）也可能导致此错误。 确保启用 PXE 启动时所有 Nbp 都是最新的。
VirtualSecureMode          | 主机上未运行基于虚拟化的安全功能。 确保启用 VBS 并且系统满足配置的[平台安全功能](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)。 有关 VBS 要求的详细信息，请参阅[Device Guard 文档](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)。

## <a name="modern-tls"></a>新式 TLS

如果你已部署了组策略或配置了 Hyper-v 主机以防使用 TLS 1.0，则在尝试启动受防护的 VM 时，可能会出现 "主机保护者服务客户端无法代表调用进程解除密钥保护程序的包装" 错误。
这是因为 .NET 4.6 中的默认行为是：在与 HGS 服务器协商支持的 TLS 版本时，不考虑系统默认 TLS 版本。

若要解决此问题，请运行以下两个命令，将 .NET 配置为对所有 .NET 应用使用系统默认 TLS 版本。

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> 系统默认的 TLS 版本设置将影响计算机上的所有 .NET 应用。 在将注册表项部署到生产计算机之前，请务必在隔离的环境中对其进行测试。

有关 .NET 4.6 和 TLS 1.0 的详细信息，请参阅[解决 TLS 1.0 问题第2版](https://docs.microsoft.com/security/solving-tls1-problem)。
