---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: TPM 密钥证明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3cfc047fe1a66617abbda1de5f2c5842dcbdeb12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862878"
---
# <a name="tpm-key-attestation"></a>TPM 密钥证明

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
同时支持的受 TPM 保护的密钥已存在 Windows 8 中，有个 Ca 以加密方式证明证书申请者私钥实际上保护的受信任的平台模块 (TPM) 的任何机制。 此更新，以执行该验证，并以反映该证明已颁发的证书的 CA。  
  
> [!NOTE]  
> 本文假定读者熟悉证书模板概念 (参考，请参阅[证书模板](https://technet.microsoft.com/library/cc730705.aspx))。 它还假定读者非常熟悉如何配置基于证书模板颁发证书的企业 Ca 使用 (参考，请参阅[核对清单：配置 Ca 以颁发和管理证书](https://technet.microsoft.com/library/cc771533.aspx))。  
  
### <a name="terminology"></a>术语  
  
|术语|定义|  
|--------|--------------|  
|EK|认可密钥。 这是 （在制造时注入） TPM 内部包含的非对称密钥。 EK 对于每个 TPM 是唯一的并可以对其进行标识。 不能更改或删除 EK。|  
|EKpub|引用的 EK 的公钥。|  
|EKPriv|引用的 EK 的私钥。|  
|EKCert|EK 证书。 由 TPM 制造商颁发的证书为 EKPub 的。 并非所有 tpm 都具有 EKCert。|  
|TPM|受信任的平台模块。 TPM 被设计用于提供基于硬件的安全相关功能。 TPM 芯片是安全加密处理器，旨在执行加密操作。 该芯片包含多个物理安全机制以使其防篡改，并且恶意软件无法篡改 TPM 的安全功能。|  
  
### <a name="background"></a>后台  
受信任的平台模块 (TPM) 从 Windows 8 开始，用于保护证书的私钥。 Microsoft 平台加密提供程序密钥存储提供程序 (KSP) 启用此功能。 没有实现的两个问题：  

-   出现密钥实际受的 TPM （某人可以软件 KSP 作为本地管理员凭据与 TPM KSP 中轻松地欺骗） 不能保证。

-   无法限制可以保护企业 （的 PKI 管理员想要控制的类型可用于获取证书在环境中的设备） 颁发的证书的 Tpm 的列表。  

### <a name="tpm-key-attestation"></a>TPM 密钥证明  
TPM 密钥证明是实体的密码学角度上向 CA 证明证书请求中的 RSA 密钥受 CA 信任的"a"或"the"TPM 请求证书的能力。 TPM 信任模型中讨论了更[部署概述](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)本主题后面的部分。  
  
### <a name="why-is-tpm-key-attestation-important"></a>TPM 密钥证明为什么很重要？  
使用 TPM 证明密钥的用户证书提供更高版本的安全保障，由非可导出性、 反攻击和隔离由 TPM 提供密钥的备份。  
  
TPM 密钥证明，新的管理模式现在已成为可能：管理员可以定义一组设备的用户可用于访问公司资源 （如 VPN 或无线访问点），且有**强**保证没有其他的设备可用于对其进行访问。 此新的访问控制范例**强**因为它绑定到*硬件绑定*用户标识，它是强于基于软件的凭据。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 密钥证明是如何工作的？  
一般情况下，TPM 密钥证明基于以下要点：  
  
1.  每个 TPM 附带了一个唯一的非对称密钥，名为*认可密钥*由制造商刻录 (EK)。 我们为此密钥的公共部分，请参阅*EKPub*和关联的私钥作为*EKPriv*。 某些 TPM 芯片还具有为 EKPub 颁发由制造商的 EK 证书。 我们对作为此证书，请参阅*EKCert*。  
  
2.  CA 建立在 TPM 中通过 EKPub 或 EKCert 信任。  
  
3.  用户为其请求证书的 RSA 密钥加密与 EKPub 和用户拥有 EKpriv 证明 CA。  
  
4.  在 CA 颁发证书与特殊的颁发策略来表示密钥现在证明由 TPM 保护的 OID。  
  
## <a name="BKMK_DeploymentOverview"></a>部署概述  
在此部署中，假定设置 Windows Server 2012 R2 企业 CA。 此外，客户端 (Windows 8.1) 配置为针对该企业 CA 使用注册的证书模板。 

有三个步骤部署 TPM 密钥证明：  
  
1.  **规划 TPM 信任模型：** 第一步是决定使用哪个 TPM 信任模型。 有 3 个受支持的方式执行此操作：  
  
    -   **信任基于用户凭据：** 企业 CA 信任用户提供 EKPub 作为证书请求的一部分，但不执行任何验证用户的域凭据以外。  
  
    -   **基于 EKCert 信任：** 企业 CA 验证证书请求的一部分提供针对一个管理员管理的列表在 EKCert 链*可接受的 EK 证书链*。 可接受的链是定义的每个制造商和通过两个自定义证书存储区表示在颁发 CA （一个存储的中间），以及一个用于根 CA 证书上。 此信任模式意味着**所有**是受信任的 Tpm 从给定的制造商。 请注意，在此模式下，环境中使用的 Tpm 必须包含 EKCerts。
  
    -   **基于 EKPub 信任：** 企业 CA 验证证书请求的一部分显示在管理员管理列表中的形式提供，EKPub 允许 EKPubs。 此列表表示为此目录中的每个文件的名称其中是允许 EKPub 的 sha-2 哈希的文件的目录。 此选项提供最高的保证级别，但需要更多的管理工作，因为单独标识每个设备。 在此信任模型中，已添加到允许列表的 EKPubs 其 TPM EKPub 设备有权注册 TPM 证明证书。  
  
    具体取决于使用哪种方法，CA 将已颁发的证书对应用不同的颁发策略 OID。 有关颁发策略 Oid 的详细信息，请参阅中的颁发策略 Oid 表[配置证书模板](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)本主题中的部分。  
  
    请注意，无法选择 TPM 信任模型的组合。 在这种情况下，CA 将接受任何证明方法和 Oid 将反映所有成功的证明方法的颁发策略。  
  
2.  **配置证书模板：** 配置证书模板中所述[部署详细信息](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)本主题中的部分。 本文不介绍如何将此证书模板分配给企业 CA 或如何注册访问权限提供给一组用户。 有关详细信息，请参阅[核对清单：配置 Ca 以颁发和管理证书](https://technet.microsoft.com/library/cc771533.aspx)。  
  
3.  **配置 CA TPM 信任模型**  
  
    1.  **信任基于用户凭据：** 任何特定的配置不是必需的。  
  
    2.  **基于 EKCert 信任：** 管理员必须从 TPM 制造商获取 EKCert 链证书并将其导入到两个新的证书存储，由管理员执行 TPM 密钥证明在 CA 上创建。 有关详细信息，请参阅[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    3.  **基于 EKPub 信任：** 管理员必须获取每个设备都将需要 TPM 证明证书并将其添加到允许 EKPubs 列表的 EKPub。 有关详细信息，请参阅[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    > [!NOTE]  
    > -   此功能需要 Windows 8.1 / Windows Server 2012 R2。  
    > -   不支持第三方智能卡 Ksp 的 TPM 密钥证明。 必须使用 Microsoft 平台加密提供程序 KSP。  
    > -   TPM 密钥证明仅适用于 RSA 密钥。  
    > -   独立 CA 不支持 TPM 密钥证明。  
    > -   TPM 密钥证明不支持[非持久性证书处理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署详细信息  
  
### <a name="BKMK_ConfigCertTemplate"></a>配置证书模板  
若要配置 TPM 密钥证明的证书模板，请执行下列配置步骤操作：  
  
1.  **兼容性**选项卡  
  
    在中**兼容性设置**部分：  
  
    -   请确保**Windows Server 2012 R2**选择了**证书颁发机构**。  
  
    -   请确保**Windows 8.1 / Windows Server 2012 R2**选择了**证书接收人**。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **加密**选项卡  
  
    请确保**密钥存储提供程序**选择了**提供程序类别**并**RSA**为选择**算法名称**。 请确保**请求必须使用以下提供程序之一**处于选中状态和**Microsoft 平台加密提供程序**下选择选项**提供程序**。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **密钥证明**选项卡  
  
    这是用于 Windows Server 2012 R2 的新选项卡：  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    从三个可能的选项中选择的证明模式。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **None:** 表示不必须使用密钥证明  
  
    -   **必需，如果客户端能够：** 不支持 TPM 密钥证明，以继续为该证书注册的设备上允许用户。 将使用特殊的颁发策略 OID 区分可以执行验证的用户。 某些设备可能不能执行由于旧的 TPM 密钥证明或根本没有 TPM 的设备不支持的证明。
  
    -   **必填：** 客户端*必须*执行 TPM 密钥证明，否则证书请求将失败。  
  
    然后，选择 TPM 信任模型。 再次，有三个选项：  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **用户凭据：** 允许通过指定其域凭据来保证有效 TPM 身份验证的用户。  
  
    -   **认可证书：** 设备的 EKCert 必须通过管理员管理 TPM 中间 CA 证书管理员管理根 CA 证书进行验证。 如果选择此选项，则必须设置 ekca，是和 EKRoot 证书发证 CA 上的存储，如中所述[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    -   **认可密钥：** 设备的 EKPub 必须出现在 PKI 管理员管理列表。 此选项提供最高的保证级别，但需要更多的管理工作。 如果选择此选项，则必须设置在颁发 CA 的 EKPub 列表中所述[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    最后，决定要显示在已颁发的证书的颁发策略。 默认情况下，每个强制类型都有关联的对象标识符 (OID) 将插入到证书中，如果它传递该强制类型，如下表中所述。 请注意，可以选择强制方法的组合。 在这种情况下，CA 将接受任何一种证明方法，颁发策略 OID 将反映所有成功的证明方法。  
  
    **颁发策略 Oid**  
  
    |OID|密钥证明类型|描述|保证级别|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"已验证的 EK": 有关管理员管理的 EK 的列表|高|  
    |1.3.6.1.4.1.311.21.31|认可证书|"EK 证书验证":EK 证书链进行验证时|中等|  
    |1.3.6.1.4.1.311.21.32|用户凭据|"EK 上受信任使用":为用户证明 EK|低|  
  
    Oid 将插入到已颁发的证书，如果**包括颁发策略**是选择 （默认配置）。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 一个使用可能的证书中存在 OID 是限制对 VPN 访问或无线网络与某些设备。 例如，你的访问策略可能会允许连接 （或访问不同的 VLAN） 如果 OID 1.3.6.1.4.1.311.21.30 是证书中存在。 这可以限制其 TPM EK 是 EKPUB 列表中存在的设备的访问权限。  
  
### <a name="BKMK_CAConfig"></a>CA 配置  
  
1.  **设置 ekca，是和 EKROOT 发证 CA 上的证书存储**  
  
    如果选择了**认可证书**模板设置，请执行下列配置步骤：  
  
    1.  使用 Windows PowerShell 将执行 TPM 密钥证明的证书颁发机构 (CA) 服务器上创建两个新的证书存储。  
  
    2.  获取中间和根 CA 证书从厂商想要允许它在企业环境中。 这些证书必须导入到根据先前创建的证书存储 （ekca，是和 EKROOT） 中。  
  
    以下 Windows PowerShell 脚本执行两个步骤。 在以下示例中，Fabrikam 的 TPM 制造商提供了根证书*FabrikamRoot.cer*和颁发 CA 证书*Fabrikamca.cer*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **如果使用 EK 证明类型，设置 EKPUB 列表**  
  
    如果选择了**认可密钥**模板设置中的下一步配置步骤是创建和配置在颁发 CA，其中包含 0 字节的文件的文件夹中，每个命名为允许 EK 对 sha-2 哈希。 此文件夹可作为有权获取 TPM 密钥证明证书的设备的"允许列表"。 由于你必须手动添加的每个设备所需的经验证的证书 EKPUB，它提供的设备的授权以获取 TPM 经验证的证书以保证了企业。 将 CA 配置为适用于此模式需要两个步骤：  
  
    1.  **创建 EndorsementKeyListDirectories 注册表项：** 使用 Certutil 命令行工具来配置受信任的 EKpubs 其中定义下表中所述的文件夹位置。  
  
        |操作|命令语法|  
        |-------------|------------------|  
        |添加文件夹位置|certutil.exe -setreg CA\EndorsementKeyListDirectories +"<folder>"|  
        |删除文件夹位置|certutil.exe -setreg CA\EndorsementKeyListDirectories -"<folder>"|  
  
        Certutil 命令中的 EndorsementKeyListDirectories 是下表中所述的注册表设置。  
  
        |值名称|在任务栏的搜索框中键入|数据|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< EKPUB 指向本地或 UNC 路径允许列表 ><br /><br />例如：<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories*将包含每个都指向 CA 具有读取访问权限的文件夹的 UNC 或本地文件系统路径列表。 每个文件夹可能包含零或更多允许列表条目，其中每个条目是为 SHA-2 的名称的文件的受信任 EKpub，哈希不带文件扩展名。 
        创建或编辑此注册表密钥配置需要重新启动 CA，就像现有 CA 注册表配置设置。 但是，对配置设置的编辑将立即生效，并且不需要重新启动 CA。  
  
        > [!IMPORTANT]  
        > 在列表中被篡改的文件夹的安全和未经授权的访问配置的权限，以便只有经过授权的管理员具有读取和写入访问权限。 CA 的计算机帐户需要读取访问权限。  
  
    2.  **填充 EKPUB 列表：** 使用以下 Windows PowerShell cmdlet，每个设备上使用 Windows PowerShell 获取 TPM EK 的公共密钥哈希，然后将此公钥哈希发送到 CA 并将其存储 EKPubList 文件夹。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>疑难解答  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>密钥证明字段将不可用的证书模板  
密钥证明字段不可用的模板设置不满足的要求进行证明。 常见原因如下：  
  
1.  兼容性设置配置不正确。 请确保配置，如下所示：  
  
    1.  **证书颁发机构**:**Windows Server 2012 R2**  
  
    2.  **证书接收方**:**Windows 8.1/Windows Server 2012 R2**  
  
2.  加密设置配置不正确。 请确保配置，如下所示：  
  
    1.  **提供程序类别**:**密钥存储提供程序**  
  
    2.  **算法名称**:**RSA**  
  
    3.  **提供程序**:**Microsoft 平台加密提供程序**  
  
3.  请求处理设置配置不正确。 请确保配置，如下所示：  
  
    1.  **允许导出私钥**不能选择的选项。  
  
    2.  **存档使用者的加密私钥**不能选择的选项。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>验证的证明的 TPM 设备  
使用 Windows PowerShell cmdlet**确认 CAEndorsementKeyInfo**，若要验证特定的 TPM 设备是否受信任的证明的 Ca。 有两个选项： 一个用于验证 EKCert，和另一个用于验证 EKPub。 Cmdlet 或者运行本地或远程 Ca 上一个 CA 上，通过使用 Windows PowerShell 远程处理。  
  
1.  对于验证 EKPub 上的信任关系，请执行以下两个步骤：  
  
    1.  **从客户端计算机中提取 EKPub:** 可以从客户端计算机通过提取 EKPub **Get TpmEndorsementKeyInfo**。 从提升的命令提示符，运行以下命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **验证在 CA 计算机上 EKCert 信任：** 将提取的字符串 （EKPub 的 sha-2 哈希） 复制到服务器 （例如，通过电子邮件），并将其传递给确认 CAEndorsementKeyInfo cmdlet。 请注意，此参数必须是 64 个字符。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  用于验证在 EKCert 信任，请执行以下两个步骤操作：  
  
    1.  **从客户端计算机中提取 EKCert:** 可以从客户端计算机通过提取 EKCert **Get TpmEndorsementKeyInfo**。 从提升的命令提示符，运行以下命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **验证在 CA 计算机上 EKCert 信任：** 将提取的 EKCert (EkCert.cer) 复制到 CA （例如，通过电子邮件或 xcopy）。 例如，如果复制在 CA 服务器上的证书文件"c:\diagnose"文件夹，运行以下命令，完成验证：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>请参阅  
[受信任的平台模块技术概述](https://technet.microsoft.com/library/jj131725.aspx)  
[外部资源：受信任的平台模块](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
