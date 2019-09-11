---
title: 为 SQL Server 中的 Always Encrypted 设置主机保护者服务
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 74146e854f4183970d2b92bb26babbd1b31f0bc2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870350"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>为包含 secure enclaves in SQL Server 的 Always Encrypted 设置主机保护者服务 

>适用于：Windows Server （半年频道），Windows Server 2019，SQL Server 2019 预览版
 
Always Encrypted 在 SQL Server 2019 预览版中[使用安全 enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) ，这是一项功能，旨在对存储在数据库中的敏感数据启用机密计算。 为 Always Encrypted 配置的安全 enclave 是基于虚拟化的安全（VBS）内存 enclave 时，主机保护者服务（HGS）在保护数据安全方面扮演着重要的角色。 VBS 内存 enclave 的安全性取决于 Windows 虚拟机监控程序的安全性，以及宿主 SQL Server 的计算机的安全性。 

因此，在数据库客户端应用程序允许 Always Encrypted 用来对敏感数据执行计算之前，应用程序必须使用受信任的 HGS 进行证明。 证明证明了承载 SQL Server 的计算机，其中包含 enclave，它处于正确的状态，并且可以信任。 对于本主题的其余部分，我们将 SQL Server 主持的计算机称为 "主机"。

应用程序需要两个互相排斥的方法来证明： 

- 主机密钥证明通过证明主机拥有已知的可信私钥来授权主机。 建议使用主机密钥证明，并为预生产环境建议运行 SQL Server 的物理主机或第2代虚拟机。
- TPM 证明验证硬件度量，以确保主机仅运行正确的二进制文件和安全策略。 对于生产环境，建议使用运行 SQL Server 的 TPM 证明和物理主机（而不是虚拟机）。

有关主机保护者服务及其可度量值的详细信息，请参阅[受保护的构造和受防护的 vm 概述](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)。 请注意，尽管文档讨论了受防护的 Vm，但相同的保护、体系结构和最佳做法也适用于使用 VBS enclaves Always Encrypted SQL Server。 

本文将帮助你在建议的配置中设置 HGS。 

## <a name="prerequisites"></a>先决条件 

本部分介绍 HGS 和主机计算机的先决条件。 

### <a name="hgs-servers"></a>HGS 服务器

- 1-3 服务器运行 HGS。 

  > [!NOTE]
  > 测试或预生产环境只需要1个 HGS 服务器。

  这些服务器应进行严密保护，因为它们控制哪些计算机可以使用安全 enclaves 的 Always Encrypted 运行 SQL Server 实例。 
  建议不同的管理员管理 HGS 群集，并且在与基础结构的其余部分隔离的物理硬件上运行 HGS，或在单独的虚拟化结构或 Azure 订阅中运行。

  - Windows Server 2019 Standard 或 Datacenter edition。
  - 2 Cpu
  - 8GB RAM
  - 100GB 存储

  您可以使用[长期服务通道（LTSC）](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年频道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要注册并下载 Windows Server 有问必答 Preview，请参阅[Windows 预览体验计划](https://insider.windows.com/for-business-getting-started-server/)入门。

- 选择由主机保护者服务创建的新 Active Directory 林的名称。 
  不应将 HGS 加入你的现有公司域，并应由单独的管理员来管理它。   

- 用于允许主机保护者服务节点上的入站 HTTP （TCP 80）或 HTTPS （TCP 443）流量的防火墙和路由规则： 

  - 运行 SQL Server 的计算机
  - 运行数据库客户端应用程序（如 web 服务器）的计算机，这些应用程序发出数据库查询，并将 Always Encrypted 与 secure enclaves 一起使用。 

### <a name="sql-server-host-machines"></a>SQL Server 主机

- SQL Server 实例应在满足以下要求的计算机上运行：

  - Windows 
    - Windows 10 企业版，1809版  
    - Windows Server 2019 Datacenter （半年频道），版本1809
    - Windows Server 2019 Datacenter
  - 用于生产的物理计算机，或用于测试的第2代虚拟机（请注意，Azure 不支持第2代 Vm）
  - [安装 SQL Server 的硬件和软件要求](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017)中列出的一般要求。   

  您可以使用[长期服务通道（LTSC）](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc)或[半年频道](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel)。 
  若要注册并下载 Windows Server 有问必答 Preview，请参阅[Windows 预览体验计划](https://insider.windows.com/for-business-getting-started-server/)入门。

- 特定于所选证明模式的要求：
  - **TPM 模式**是最强的证明模式，将使用受信任的平台模块（TPM）对你的数据中心（从每个 TPM 使用唯一 ID）进行加密验证，并运行受信任的硬件和固件配置（使用 TPM 基线），并运行可信内核和用户模式代码（使用 Windows Defender 应用程序控制）。 需要以下硬件才能使用 TPM 模式： 
    - 已安装并启用 TPM 2.0 模块 
    - 使用 Microsoft 安全启动策略启用安全启动（不启用第三方安全启动 CA 策略或任何自定义策略）
    - IOMMU （Intel VT 或 AMD SR-IOV）来防止直接内存访问攻击 

  - **主机密钥模式**使用非对称密钥对（非常类似于 SSH 密钥）标识和授权要 SQL Server 运行的主机。 此模式的设置更简单，并且没有任何特定硬件要求，但不会验证在主机上运行的软件或固件。  

若要检查 TPM 是否兼容，请在要使用安全 enclaves 的 Always Encrypted 运行 SQL Server 的主机上运行以下命令。 "2.0" 必须出现在支持的 SpecVersions 列表中才能使用 TPM 证明：

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>设置第一个 HGS 节点 

主机保护者服务使用三节点群集在高可用性配置中运行。 建议在添加其他节点之前，完全设置一个节点。 

在提升的 PowerShell 会话中运行以下所有命令。

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   对于具有 secure enclaves 的 Always Encrypted，运行 SQL Server 的主机和运行数据库客户端应用程序的计算机都需要与 HGS 联系，不过只有主机计算机需要证明。

4. 计算机重新启动后，将安装 HGS，而且服务器也将是配置了 Active Directory 的域控制器。 
   在 Active Directory 配置过程中，会将本地计算机管理员帐户添加到域管理员组，只有此组的成员可以登录到域控制器。
   使用域管理员帐户（例如administrator@bastion.local或 local\administrator）登录，或者创建另一个域管理员帐户进行登录，然后配置证明服务。
   需要选择 "TPM" 或 "主机密钥证明" 并运行相应的命令。 
   对于 HgsServiceName，请指定选择的 DNN。

   对于 TPM 模式：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   对于主机密钥模式：

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. 确保运行 SQL Server 的主机能够通过设置从企业 DNS 服务器到新的 HGS 域控制器的转发器来解析新的 HGS 域成员的 DNS 名称。 如果你使用的是 Windows Server DNS，则可以通过在组织中的 DNS 服务器上的提升的 PowerShell 控制台中运行以下命令来设置条件转发器。 根据环境需要，在下面的 Windows PowerShell 语法中替换名称和地址。 添加更多 HGS 节点后，再次运行此命令，以将其添加为主服务器。

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>为生产部署设置其他 HGS 节点

若要将节点添加到群集，请在提升的 PowerShell 会话中运行以下命令。 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. 将 DNS 客户端解析程序设置为指向主 HGS 服务器，使其能够解析你的 HGS 域名。 如果你使用的是具有桌面体验的服务器，你可以在 "网络和共享中心" 中执行此操作。 在服务器核心上，可以使用**sconfig.cmd**工具、选项8或**DnsClientServerAddress**设置 DNS 地址。 

3. 接下来，我们将此服务器升级到域控制器。 你将需要使用域管理员凭据来完成此任务，并且在运行命令后，系统将提示你输入目录服务修复模式密码。 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>为 HTTPS 配置 HGS 

默认情况下，初始化 HGS 服务器时，它会将 IIS 网站配置为仅限 HTTP 通信。

> [!NOTE]
> 若要防止中间人攻击，需要使用众所周知且受信任的 HGS 服务器证书配置 HTTPS，因此建议用于生产部署。

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> 对于安全 enclaves 的 Always Encrypted，SSL 证书需要在运行 SQL Server 的两个主机计算机上受信任，并且运行数据库客户端应用程序的计算机需要联系 HGS。 

## <a name="collect-attestation-info-from-the-host-machines"></a>从主机收集证明信息

设置完 HGS 后，需要使用主机计算机上的证明信息来配置它，以使其知道哪些计算机应该有权使用 Always Encrypted 和 VBS secure enclaves 来执行机密计算。 这些步骤根据你使用的证明模式而有所不同。 

### <a name="collect-tpm-attestation-artifacts"></a>收集 TPM 证明项目 

如果使用的是 TPM 模式，请在每台主机计算机上的提升的 PowerShell 会话中运行以下命令，以安装对证明的支持，并收集注册到主机保护者服务所需的信息。 

1. 若要在主计算机上安装 HGS 客户端，请安装受保护的主机功能，该功能还将安装 Hyper-v。 
   虽然你不会在此计算机上运行 Vm，但虚拟机监控程序需要启用隔离 VBS enclaves 的基于虚拟化的安全功能。

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 当系统提示你完成 Hyper-v 的安装时，请重新启动计算机。 
3. 编写代码完整性策略，以限制可在系统上运行的软件。 
   任何 Windows Defender 应用程序控制策略都已足够。 
   如果你仅在服务器上运行 Microsoft 软件，以下命令将快速为你创建一个策略。 
   此策略将处于审核模式，这意味着它将记录有关未经授权的代码的事件，但不会使其运行。  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender 应用程序控制具有许多功能，可涵盖各种安全 postures。 
   如果需要允许非 Microsoft 软件或自定义默认策略，请尝试《 [Windows Defender 应用程序控制部署指南》](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。   


4. 验证基于虚拟化的安全性是否正在您的计算机上运行以下命令。 
   如果 "DeviceGuardSecurityServicesRunning" 字段中列出了 "HypervisorEnforcedCodeIntegrity"，则会知道 VBS 正在运行。 
   如果未运行，请下载[Device Guard 准备工具](https://www.microsoft.com/download/details.aspx?id=53337)并运行 "DG_READINESS-要求 hvci" 来启用它。  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. 收集 TPM 标识符和基线：

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. 将 xml、tcglog 和 bin 文件从 C:\artifacts 复制到 HGS 服务器。
7. 如果这是要添加到 HGS 服务器的第一个 TPM 主机，则需要在每个 HGS 服务器上安装受信任的 TPM 根证书。 
   按照[HGS 文档上的指导](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates)完成此步骤。
8. 在 HGS 服务器上，授权此主机通过证明。 
   下面的脚本假定将3个文件复制到 HGS 服务器上的 C：\temp，并且你的服务器的计算机名为 "ServerA"。 
   调整路径和名称，使其与你自己的环境相匹配。 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. 你的第一台服务器现在可以证明！ 
   在主计算机上，运行以下命令告诉它要证明的位置（将 DNS 名称更改为你的 HGS 群集的名称），通常会将 HGS 服务名称与 HGS 域名结合使用。 
   如果收到 HostUnreachable 错误，请确保可以解析和 ping HGS 服务器的 DNS 名称。 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. 上述命令的结果应显示 AttestationStatus = 通过。 如果不是这样，请参阅[证明失败](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures)，以获取有关如何解决错误的指南。   
11. 为每台主机重复步骤1-10。 
    如果使用相同的硬件，则不需要为每台计算机捕获新的基线或 CI 策略。 
    第一台服务器的基线将涵盖所有配置完全相同的计算机，并且可以在多台计算机之间重复使用 CI 策略，只要 Microsoft 软件是计算机上的唯一软件即可。

### <a name="collecting-host-keys"></a>收集主机密钥 

> [!NOTE] 
> 仅建议在测试环境中使用主机密钥证明。 TPM 证明在运行受信任的代码，并使用推荐的安全设置配置了 enclaves，提供了 VBS 处理 SQL Server 的敏感数据的最强保障。 

如果选择在主机密钥证明模式下设置 HGS，则需要从每个主机计算机生成并收集密钥，并向主机保护者服务注册密钥。 

1. 若要在主计算机上安装 HGS 客户端，请安装受保护的主机功能，该功能还将安装 Hyper-v。 
   尽管你将不会在此计算机上运行 Vm，但虚拟机监控程序需要启用基于虚拟化的安全功能，这些功能将运行 Always Encrypted 查询的 VBS enclaves 隔离开来。 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. 当系统提示你完成 Hyper-v 的安装时，请重新启动计算机。
3. 生成唯一的主机密钥并将生成的公钥导出到文件中。 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   或者，如果想要使用自己的证书，也可以指定指纹。 
   如果要在多台计算机上共享证书，或者使用绑定到 TPM 或 HSM 的证书，这会很有用。 下面是创建与 TPM 绑定的证书的示例（该证书可防止其在另一台计算机上盗取并使用私钥，只需 TPM 1.2）：

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. 将主机密钥复制到主机保护者服务。 
5. 使用与你的环境相关的名称和路径向任何 HGS 节点注册主机密钥： 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. 你的第一台服务器现在可以证明！ 
   在主计算机上，运行以下命令告诉它要证明的位置（将 DNS 名称更改为你的 HGS 群集的名称），通常会将 HGS 服务名称与 HGS 域名结合使用。 
   如果收到 HostUnreachable 错误，请确保可以解析和 ping HGS 服务器的 DNS 名称。    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. 上述命令的结果应显示 AttestationStatus = 通过。 
   如果遇到 HostUnreachable 错误，则意味着主机无法与 HGS 通信。 
   请确保在主机和 HGS 服务器之间设置 DNS 解析，并且可以对服务器进行 ping 操作。 
   UnauthorizedHost 错误指示未向 HGS 服务器注册公钥–重复步骤4和步骤5以解决此错误。 
   如果所有其他操作均失败，请运行 HgsClientHostKey 并重复步骤3-6。   

8. 对于将使用 VBS enclaves 运行 SQL Server Always Encrypted 的每个服务器重复步骤1-7。     


