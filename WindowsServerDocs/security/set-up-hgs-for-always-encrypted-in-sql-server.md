---
title: 设置为始终加密 SQL Server 中的主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 70f6f8c2db742361deecaa216b053d8b1d057a3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812611"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>使用 SQL Server 中安全 enclaves Always encrypted 设置主机保护者服务 

>适用于：Windows Server （半年频道），Windows Server 2019，SQL Server 2019 预览
 
[Always Encrypted 安全 enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves)中 SQL Server 2019 预览版是一项旨在启用机密存储在数据库中的敏感数据的计算功能。 主机保护者服务 (HGS) 起着重要作用中安全的 enclave，配置为始终加密，是基于虚拟化的安全 (VBS) 内存 enclave 时保持数据安全。 VBS 内存 enclave 的安全取决于 Windows 虚拟机监控程序的安全性和更广泛地说，承载 SQL Server 的计算机的安全性。 

因此，数据库客户端应用程序允许 VBS 内存 enclave 的 Always Encrypted 用于对敏感数据执行计算之前，则该应用程序必须与受信任的 HGS 证明。 证明可证明托管 SQL Server，它包含 enclave 且处于正确状态是可以为受信任的计算机。 本主题的其余部分，我们将引用托管 SQL Server 作为只是主机的计算机。

有两个，证明该应用程序独占方式： 

- 主机密钥证明授权主机通过证明它拥有已知和受信任的私钥。 主机密钥证明以及物理主机或运行 SQL Server 的第 2 代虚拟机建议用于预生产环境。
- TPM 证明验证硬件度量，以确保主机运行正确的二进制文件和安全策略。 TPM 证明和运行 SQL Server 物理主机计算机 （而不是虚拟机） 被建议用于生产环境中。

有关主机保护者服务和它可以测量的详细信息，请参阅[受保护的构造和受防护的 Vm 概述](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)。 请注意，尽管文档讨论受防护的 Vm，但相同的保护措施、 体系结构和最佳实践适用于 SQL Server 使用 Always Encrypted VBS enclaves。 

本文将帮助你设置 HGS 在推荐的配置。 

## <a name="prerequisites"></a>先决条件 

本部分介绍 HGS 和主机计算机的先决条件。 

### <a name="hgs-servers"></a>HGS 服务器

- 1-3 运行 HGS 服务器。 

  > [!NOTE]
  > 只有 1 HGS 服务器是必需的测试或预生产环境。

  这些服务器应受到严密的保护，因为它们控制哪些计算机可以运行使用 Always Encrypted 安全 enclaves 和 SQL Server 实例。 
  建议不同管理员管理 HGS 群集并与你基础结构，或在单独的虚拟化结构或 Azure 订阅中的其余部分隔离的物理硬件上运行 HGS。

  - Windows Server 2019 Standard 或 Datacenter edition。
  - 2 个 Cpu
  - 8GB RAM
  - 100 GB 存储空间

  你可以使用[长期服务频道 (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年频道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要注册并下载 Windows Server Insider Preview，请参阅[开始使用 Windows 预览体验计划](https://insider.windows.com/for-business-getting-started-server/)。

- 选择新创建的主机保护者服务的 Active Directory 林的名称。 
  HGS 不应加入现有公司的域，并且应具有不同的管理员对其进行管理。   

- 防火墙和路由规则以允许从主机保护者服务节点上的入站的 HTTP (TCP 80) 或 HTTPS (TCP 443) 的流量： 

  - 运行 SQL Server 的计算机
  - 运行数据库客户端应用程序 （如 web 服务器） 的发布数据库查询并安全 enclaves 使用 Always Encrypted 的计算机。 

### <a name="sql-server-host-machines"></a>SQL Server 宿主计算机

- SQL Server 实例应符合以下要求的计算机上运行:

  - Windows: 
    - Windows 10 企业版，版本 1809  
    - Windows Server 2019 Datacenter （半年频道） 版本 1809
    - Windows Server 2019 Datacenter
  - 用于生产或用于测试 （请注意，Azure 不支持第 2 代虚拟机） 的第 2 代虚拟机的物理计算机
  - 中列出的常规要求[硬件和软件要求安装 SQL Server 的](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017)。   

  你可以使用[长期服务频道 (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年频道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要注册并下载 Windows Server Insider Preview，请参阅[开始使用 Windows 预览体验计划](https://insider.windows.com/for-business-getting-started-server/)。

- 要求特定于所选的证明模式：
  - **TPM 模式**是最强的证明模式，并且将使用受信任平台模块 (TPM) 密码学角度上验证主机计算机已知到你的数据中心 （使用来自每个 TPM 唯一 ID），运行受信任的硬件和固件（使用 TPM 基线），配置和运行高信度的内核和用户模式代码 （使用 Windows Defender 应用程序控制）。 使用 TPM 模式下需要以下硬件： 
    - 安装并启用了 TPM 2.0 模块 
    - 与 Microsoft 安全启动策略已启用安全启动 （不使第三方安全启动 CA 策略或任何自定义策略）
    - IOMMU （Intel VT d 或 AMD IOV） 以防止直接内存访问攻击 

  - **主机键模式**使用对称密钥对 （非常类似于 SSH 密钥） 来标识和授权想要运行在 SQL Server 的主机。 此模式是更方便地设置和不具有任何特定的硬件要求，但将不会验证该软件或固件主机计算机上运行。  

若要检查你的 TPM 是否兼容，运行以下命令在主机上要运行 SQL Server 安全 enclaves 使用始终加密。 "2.0"必须出现在列表中，以便使用 TPM 证明的受支持 SpecVersions:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>设置第一个 HGS 节点 

在高度可用的配置使用的是包含 3 个节点群集运行主机保护者服务。 建议你将设置一个节点完全添加其他节点之前。 

在提升的 PowerShell 会话中运行的所有以下命令。

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   使用安全 enclaves 始终加密，对于运行 SQL Server 和计算机的主机运行数据库客户端应用程序都需要联系 HGS，尽管仅主机需要证明。

4. 重新启动计算机后，将安装 HGS 和服务器也将使用配置的 Active Directory 域控制器。 
   Active Directory 在配置期间，本地计算机管理员帐户添加到 Domain Admins 组和只有此组的成员可以登录到域控制器。
   使用域管理员帐户登录 (例如，administrator@bastion.local或 bastion.local\administrator) 或创建另一个域管理员帐户登录，然后配置证明服务。
   你将需要选择 TPM 或主机密钥证明，并运行相应命令。 
   对于 HgsServiceName，请指定所选的 DNN。

   对于 TPM 模式：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   对于主机密钥模式：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. 请确保运行 SQL Server 的主机将能够通过设置到新的 HGS 域控制器从公司的 DNS 服务器转发器解析 DNS 名称的新 HGS 域成员。 如果使用的 Windows Server DNS，可以通过在你的组织的 DNS 服务器上提升的 PowerShell 控制台中运行以下命令来设置条件转发器。 替换的姓名和地址作为你的环境时需要以下 Windows PowerShell 语法中。 添加更多的 HGS 节点后，运行此命令再次将其添加主服务器。

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>设置生产部署的其他 HGS 节点

若要向群集添加节点，请在提升的 PowerShell 会话中运行以下命令。 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. 设置为指向主 HGS 服务器，以便它可以解析你 HGS 的域名的 DNS 客户端解析程序。 如果服务器在使用桌面体验，可以在网络和共享中心来执行此操作。 可以使用服务器核心**sconfig.exe**工具，选项 8，或**集 DnsClientServerAddress**设置 DNS 地址。 

3. 接下来，我们将此将服务器提升为域控制器。 你将需要域管理员凭据才能完成此任务，将提示您运行该命令后输入目录服务修复模式密码。 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>为 HTTPS 配置 HGS 

默认情况下，当初始化 HGS 服务器时它将 IIS 网站配置为仅限 HTTP 的通信。

> [!NOTE]
> 配置使用已知和受信任的 HGS 服务器证书的 HTTPS 需要避免拦截的攻击，因此建议对于生产部署中。

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> 有关使用安全 enclaves 始终加密，需要两台运行 SQL Server 的主机计算机上受信任的 SSL 证书和需要联系 HGS 运行数据库客户端应用程序的计算机。 

## <a name="collect-attestation-info-from-the-host-machines"></a>从主机收集证明信息

一旦 HGS 设置，它需要从主机的计算机的证明信息与配置它知道哪些计算机应该有权执行机密计算使用 Always Encrypted 和 VBS 安全 enclaves。 这些步骤因你使用的证明模式。 

### <a name="collect-tpm-attestation-artifacts"></a>收集的 TPM 证明项目 

如果使用 TPM 模式下，在提升的 PowerShell 会话中运行以下命令以安装对证明的支持和收集的信息，将需要注册主机保护者服务每台主机计算机上。 

1. 若要在主机计算机上安装 HGS 客户端，安装的受保护的主机功能，还将安装 HYPER-V。 
   尽管你将不会运行虚拟机在此计算机上，虚拟机监控程序需要启用隔离 VBS enclaves 的基于虚拟化的安全功能。

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 重新启动计算机时，系统提示完成安装 HYPER-V。 
3. 编写代码完整性策略来限制可以在系统上运行的软件。 
   任何 Windows Defender 应用程序控制策略就足够了。 
   如果只在服务器上运行 Microsoft 软件，则以下命令将快速为你创建策略。 
   该策略将处于审核模式，这意味着它将记录一个事件有关未经授权的代码，但不是会对其运行。  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender 应用程序具有许多功能，以涵盖各种安全 postures。 
   如果你需要允许非 Microsoft 软件或自定义默认策略，se [Windows Defender 应用程序部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。   


4. 验证基于虚拟化的安全性使用以下命令在计算机上运行。 
   您将知道是否 DeviceGuardSecurityServicesRunning 字段有"HypervisorEnforcedCodeIntegrity"列出在其中运行 VBS。 
   如果未运行，下载[设备防护准备工具](https://www.microsoft.com/download/details.aspx?id=53337)并运行"DG_Readiness.ps1-启用-HVCI"来启用它。  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. 收集的 TPM 标识符和基线：

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. 将从 C:\artifacts xml、 tcglog 和 bin 文件复制到 HGS 服务器中。
7. 如果这是要添加到 HGS 服务器的第一个 TPM 主机，您需要在每个 HGS 服务器上安装受信任的 TPM 根证书。 
   请按照[HGS 文档的指南](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates)来完成此步骤。
8. 在 HGS 服务器授权通过证明此主机。 
   下面的脚本假定 3 个文件复制到 C:\temp HGS 服务器上和你的服务器的计算机名称是"ServerA"。 
   调整的路径和名称以匹配您自己的环境。 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. 第一台服务器现在已准备好证明 ！ 
   在主机上运行以下命令以告诉它证明 （与 HGS 群集的 DNS 名称，通常将使用 HGS 服务名称与 HGS 域名相结合的更改） 的位置。 
   如果收到 HostUnreachable 错误，请确保可以解析并 ping HGS 服务器的 DNS 名称。 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. 上述命令的结果应显示该 AttestationStatus = 已通过。 如果未显示，请参阅[证明失败](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures)有关如何解决该错误的指南。   
11. 对每个主机重复步骤 1-10。 
    如果使用的完全相同的硬件，不需要捕获新的基线或为每台计算机的 CI 策略。 
    从第一台服务器的基线将涵盖所有具有相同配置的计算机和 CI 策略可重用于跨多台计算机，只要 Microsoft 软件是在计算机上的唯一软件。

### <a name="collecting-host-keys"></a>收集主机密钥 

> [!NOTE] 
> 主机密钥证明仅建议在测试环境中使用。 TPM 证明提供最强的保证 VBS enclaves 处理敏感数据在 SQL Server 上的运行受信任的代码和计算机都配置了推荐的安全设置。 

如果你选择在主机密钥证明模式下设置 HGS，将需要生成和从每台主机收集项并将它们注册到主机保护者服务。 

1. 若要获取安装在主计算机上的 HGS 客户端，请安装受保护的主机功能，它还将安装 HYPER-V。 
   尽管你将不会运行虚拟机在此计算机上，虚拟机监控程序需要启用隔离运行 Always Encrypted 查询 VBS enclaves 的基于虚拟化的安全功能。 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 重新启动计算机时，系统提示完成安装 HYPER-V。
3. 生成唯一的主机密钥并将生成的公钥导出到文件。 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   或者，可以指定一个指纹，如果想要使用你自己的证书。 
   这可以是你想要在多台计算机之间共享证书或使用证书绑定到 TPM 或 HSM 的情况下很有用。 下面是创建 TPM 绑定证书 （这可以防止被盗和另一台计算机上使用私钥并需要仅 TPM 1.2） 的示例：

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. 将主机密钥复制到主机保护者服务。 
5. 注册适用于你环境中使用相关名称和路径的任何 HGS 节点的主机密钥： 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. 第一台服务器现在已准备好证明 ！ 
   在主机上运行以下命令以告诉它证明 （与 HGS 群集的 DNS 名称，通常将使用 HGS 服务名称与 HGS 域名相结合的更改） 的位置。 
   如果收到 HostUnreachable 错误，请确保可以解析并 ping HGS 服务器的 DNS 名称。    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. 上述命令的结果应显示该 AttestationStatus = 已通过。 
   如果收到 HostUnreachable 错误，这意味着您的主机不能与 HGS 通信。 
   确保主机与 HGS 服务器之间建立 DNS 解析，可以对服务器进行 ping 操作。 
   一个 UnauthorizedHost 错误，指示与 HGS 服务器 – 重复执行步骤 4 和 5 以解决该错误未注册的公共密钥。 
   如果所有其他方法均失败，运行清除 HgsClientHostKey 并重复步骤 3-6。   

8. 为将运行 SQL Server 使用 Always Encrypted VBS enclaves 每个服务器重复步骤 1-7。     


