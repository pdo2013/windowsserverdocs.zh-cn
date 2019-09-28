---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: TPM 密钥证明
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d7104daaa10cf7093370cb309e0366e1ab2b9b51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389862"
---
# <a name="tpm-key-attestation"></a>TPM 密钥证明

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**：Justin Turner，具有 Windows 组的高级支持升级工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
尽管自 Windows 8 以来，对受 TPM 保护的密钥的支持已存在，但没有任何机制可以通过加密方式证明证书申请者私钥实际上由受信任的平台模块（TPM）保护。 此更新使 CA 能够执行该证明并在颁发的证书中反映该证明。  
  
> [!NOTE]  
> 本文假定读者熟悉证书模板概念（有关参考，请参阅[证书模板](https://technet.microsoft.com/library/cc730705.aspx)）。 它还假设读者熟悉如何配置企业 Ca，使其基于证书模板颁发证书（有关参考，请参阅 [Checklist：配置 Ca 以颁发和管理证书 @ no__t-0）。  
  
### <a name="terminology"></a>术语  
  
|术语|定义|  
|--------|--------------|  
|EK|认可密钥。 这是 TPM 内包含的非对称密钥（在制造时注入）。 EK 对于每个 TPM 都是唯一的，可以识别它。 不能更改或删除 EK。|  
|EKpub|引用 EK 的公钥。|  
|EKPriv|引用 EK 的私钥。|  
|EKCert|EK 证书。 TPM 制造商为 EKPub 颁发的证书。 并非所有 Tpm 都具有 EKCert。|  
|TPM|受信任的平台模块。 TPM 旨在提供基于硬件的安全性相关的功能。 TPM 芯片是安全加密处理器，旨在执行加密操作。 该芯片包含多个物理安全机制以使其防篡改，并且恶意软件无法篡改 TPM 的安全功能。|  
  
### <a name="background"></a>后台  
从 Windows 8 开始，可使用受信任的平台模块（TPM）来保护证书的私钥。 Microsoft 平台加密提供程序密钥存储提供程序（KSP）启用此功能。 实现涉及两个问题：  

-   不能保证密钥实际上受 TPM 保护（用户可以使用本地管理员凭据将软件 KSP 作为 TPM KSP 进行欺骗）。

-   不能限制允许其保护企业颁发的证书的 Tpm 的列表（如果 PKI 管理员要控制可用于在环境中获取证书的设备类型）。  

### <a name="tpm-key-attestation"></a>TPM 密钥证明  
TPM 密钥证明是指请求证书以加密方式向 CA 证明证书请求中的 RSA 密钥受 CA 信任的 "a" 或 "the" TPM 保护的证书的能力。 本主题后面的 "[部署概述](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)" 部分中详细讨论了 TPM 信任模型。  
  
### <a name="why-is-tpm-key-attestation-important"></a>为什么 TPM 密钥证明很重要？  
具有证明密钥的用户证书提供了更高的安全性保障, 并通过非作为后盾、反攻击和 TPM 提供的密钥的隔离进行了备份。  
  
使用 TPM 密钥证明，现在可以使用新的管理模式：管理员可以定义一组设备，用户可以使用这些设备来访问公司资源（例如 VPN 或无线访问点），**并确保不**能使用其他设备来访问它们。 这一新的访问控制范例非常**强大**，因为它与*硬件绑定*用户标识（比基于软件的凭据更强）相关联。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 密钥证明如何工作？  
通常，TPM 密钥证明基于以下支柱：  
  
1.  每个 TPM 附带一个唯一的非对称密钥，该密钥称为*认可密钥*（EK），由制造商刻录。 我们将此密钥的公共部分称为*EKPub* ，将关联的私钥称为*EKPriv*。 某些 TPM 芯片还具有 EKPub 制造商颁发的 EK 证书。 我们将此证书称为*EKCert*。  
  
2.  CA 通过 EKPub 或 EKCert 在 TPM 中建立信任。  
  
3.  用户向 CA 证明，为其请求证书的 RSA 密钥与 EKPub 有关的加密，并且用户拥有 EKpriv。  
  
4.  CA 颁发具有特殊颁发策略 OID 的证书，以表示该密钥现在证明受 TPM 保护。  
  
## <a name="BKMK_DeploymentOverview"></a>部署概述  
在此部署中，假定已设置 Windows Server 2012 R2 企业 CA。 此外，客户端（Windows 8.1）配置为使用证书模板针对该企业 CA 进行注册。 

部署 TPM 密钥证明有三个步骤：  
  
1.  **规划 TPM 信任模型：** 第一步是确定要使用的 TPM 信任模型。 可通过3种方法执行此操作：  
  
    -   **基于用户凭据的信任：** 企业 CA 相信用户提供的 EKPub 作为证书请求的一部分，而不执行除用户域凭据以外的任何验证。  
  
    -   **基于 EKCert 的信任：** 企业 CA 将验证作为证书请求的一部分提供的 EKCert 链，该链针对的*可接受 EK 证书链*的管理员管理列表。 可接受的链按制造商定义，并通过发证 CA 上的两个自定义证书存储来表示（一个存储用于中间，一个用于根 CA 证书）。 此信任模式意味着来自给定制造商的**所有**tpm 都受信任。 请注意，在此模式下，环境中使用的 Tpm 必须包含 EKCerts。
  
    -   **基于 EKPub 的信任：** 企业 CA 验证作为证书请求的一部分提供的 EKPub 是否显示在管理员托管的允许 EKPubs 列表中。 此列表表示为文件目录，其中每个文件的名称都是允许的 EKPub 的 SHA-1 哈希。 此选项可提供最高的保障级别，但需要更多管理工作，因为每个设备单独标识。 在此信任模式下，仅允许已将其 TPM EKPub 添加到 EKPubs 允许列表的设备注册证明证书。  
  
    根据所使用的方法，CA 会将不同的颁发策略 OID 应用于颁发的证书。 有关颁发策略 Oid 的详细信息，请参阅本主题中 "[配置证书模板](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)" 部分中的 "颁发策略 oid" 表。  
  
    请注意，可以选择 TPM 信任模型的组合。 在这种情况下，CA 将接受任何证明方法，并且颁发策略 Oid 将反映所有成功的证明方法。  
  
2.  **配置证书模板：** 本主题中的[部署详细信息](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)部分介绍了配置证书模板。 本文不介绍如何将此证书模板分配给企业 CA，或者如何向一组用户提供注册访问权限。 有关详细信息，请参阅 [Checklist：配置 Ca 以颁发和管理证书 @ no__t。  
  
3.  **为 TPM 信任模型配置 CA**  
  
    1.  **基于用户凭据的信任：** 不需要特定的配置。  
  
    2.  **基于 EKCert 的信任：** 管理员必须从 TPM 制造商处获取 EKCert 链证书，并将其导入到在执行 TPM 密钥证明的 CA 上由管理员创建的两个新证书存储。 有关详细信息，请参阅本主题中的[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)部分。  
  
    3.  **基于 EKPub 的信任：** 管理员必须获取将需要 TPM 证明证书的每个设备的 EKPub，并将其添加到允许的 EKPubs 列表。 有关详细信息，请参阅本主题中的[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)部分。  
  
    > [!NOTE]  
    > -   此功能需要 Windows 8.1/Windows Server 2012 R2。  
    > -   不支持第三方智能卡 Ksp 的 TPM 密钥证明。 必须使用 Microsoft 平台加密提供程序 KSP。  
    > -   TPM 密钥证明仅适用于 RSA 密钥。  
    > -   独立 CA 不支持 TPM 密钥证明。  
    > -   TPM 密钥证明不支持[非持久证书处理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署详细信息  
  
### <a name="BKMK_ConfigCertTemplate"></a>配置证书模板  
若要配置 TPM 密钥证明的证书模板，请执行以下配置步骤：  
  
1.  **兼容性**选项卡  
  
    在 "**兼容性设置**" 部分中：  
  
    -   确保为**证书颁发机构**选择了**Windows Server 2012 R2** 。  
  
    -   确保为**证书接收方**选择**Windows 8.1/Windows Server 2012 R2** 。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **加密**选项卡  
  
    确保为 "**提供程序" 类别**选择 "**密钥存储提供程序**"，并为 "**算法名称**" 选择**RSA** 。 请确保已选中 **"请求必须使用以下提供程序之一"** ，并且在 "**提供程序**" 下选择了 " **Microsoft 平台加密提供程序**" 选项。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **密钥证明**选项卡  
  
    这是 Windows Server 2012 R2 的新选项卡：  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    从三个可能的选项中选择证明模式。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **内容**表示不得使用密钥证明  
  
    -   **如果客户端支持，则需要：** 允许设备上不支持 TPM 密钥证明的用户继续注册该证书。 可以执行证明的用户将与特殊的颁发策略 OID 区分开来。 某些设备可能无法执行证明，因为旧的 TPM 不支持密钥证明，或者设备根本没有 TPM。
  
    -   **必填：** 客户端*必须*执行 TPM 密钥证明，否则证书请求将会失败。  
  
    然后选择 TPM 信任模型。 再次提供三个选项：  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **用户凭据：** 允许身份验证用户通过指定其域凭据来保证有效的 TPM。  
  
    -   **签署证书：** 设备的 EKCert 必须通过管理员管理的 TPM 中间 CA 证书验证到管理员管理的根 CA 证书。 如果选择此选项，则必须在颁发 CA 上设置 EKCA 和 EKRoot 证书存储区，如本主题中的[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)部分所述。  
  
    -   **认可密钥：** 设备的 EKPub 必须出现在 "PKI 管理员管理" 列表中。 此选项提供最高的保障级别，但需要更多的管理工作量。 如果选择此选项，则必须在颁发 CA 上设置 EKPub 列表，如本主题的[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)部分中所述。  
  
    最后，确定要在颁发的证书中显示的颁发策略。 默认情况下，每个强制类型都有一个关联的对象标识符（OID），如果传递该强制类型，则会将其插入证书，如下表所述。 请注意，可以选择强制方法的组合。 在这种情况下，CA 将接受任何证明方法，并且颁发策略 OID 将反映成功的所有证明方法。  
  
    **颁发策略 Oid**  
  
    |OID|密钥证明类型|描述|保证级别|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK 已验证"： 对于管理员托管的 EK 列表|高|  
    |1.3.6.1.4.1.311.21.31|认可证书|"EK 证书已验证"：当验证 EK 证书链时|中等|  
    |1.3.6.1.4.1.311.21.32|用户凭据|"在使用时，EK 可信"：对于用户-证明 EK|低|  
  
    如果已选择 "**包括颁发策略**" （默认配置），则将在已颁发的证书中插入 oid。  
  
    ![TPM 密钥证明](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 在证书中具有 OID 的一个潜在用途是将对 VPN 或无线网络的访问限制为特定设备。 例如，如果在证书中存在 OID 1.3.6.1.4.1.311.21.30，则访问策略可能允许连接（或访问其他 VLAN）。 这样，便可以限制对 EKPUB 列表中的 TPM EK 存在的设备的访问。  
  
### <a name="BKMK_CAConfig"></a>CA 配置  
  
1.  **在颁发 CA 上安装 EKCA 和 EKROOT 证书存储**  
  
    如果为模板设置选择了 "**签署证书**"，请执行以下配置步骤：  
  
    1.  使用 Windows PowerShell 在将执行 TPM 密钥证明的证书颁发机构（CA）服务器上创建两个新证书存储。  
  
    2.  从制造商处获取要在企业环境中允许的中间 CA 证书和根 CA 证书。 必须根据需要将这些证书导入之前创建的证书存储区（EKCA 和 EKROOT）。  
  
    下面的 Windows PowerShell 脚本将执行上述两个步骤。 在以下示例中，TPM 制造商 Fabrikam 提供了根证书*FabrikamRoot*和颁发 CA 证书*contoso-fabrikamca*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **如果使用 EK 认证类型，则设置 EKPUB List**  
  
    如果在模板设置中选择了 "**认可密钥**"，则下一个配置步骤是在颁发 CA 上创建和配置一个文件夹，其中每个文件夹都命名为允许的 EK 的 sha-1 哈希。 此文件夹充当允许获取 TPM 密钥证明证书的设备的 "允许列表"。 由于你必须为每个需要证明证书的设备手动添加 EKPUB，因此它向企业提供了一个保证获得 TPM 密钥证明证书授权的设备。 为此模式配置 CA 需要两个步骤：  
  
    1.  **创建 EndorsementKeyListDirectories 注册表项：** 使用 Certutil 命令行工具配置受信任的 EKpubs 定义的文件夹位置，如下表所述。  
  
        |操作|命令语法|  
        |-------------|------------------|  
        |添加文件夹位置|certutil-setreg CA\EndorsementKeyListDirectories + "<folder>"|  
        |删除文件夹位置|certutil-setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        EndorsementKeyListDirectories in certutil 命令是如下表中所述的注册表设置。  
  
        |值名称|类型|Data|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< 允许列表的本地路径或 UNC 路径 ><br /><br />例如：<br /><br />*\\ \ blueCA com\ekpub*<br /><br />*\\ \ bluecluster1 com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration @ no__t-0 @ no__t-1  
  
        *EndorsementKeyListDirectories*将包含 UNC 或本地文件系统路径的列表，每个路径都指向 CA 对其具有读取访问权限的文件夹。 每个文件夹可包含零个或多个允许列表项，其中每个项都是一个文件，其名称是可信 EKpub 的 SHA-1 哈希，没有文件扩展名。 
        创建或编辑此注册表项配置需要重新启动 CA，就像现有的 CA 注册表配置设置一样。 但是，对配置设置的编辑会立即生效，并且不需要重新启动 CA。  
  
        > [!IMPORTANT]  
        > 通过配置权限，使只有经过授权的管理员才具有读取和写入访问权限，确保列表中的文件夹不受篡改和未经授权的访问。 CA 的计算机帐户仅需要 "读取" 访问权限。  
  
    2.  **填充 "EKPUB" 列表：** 使用以下 Windows PowerShell cmdlet，通过在每个设备上使用 Windows PowerShell 来获取 TPM EK 的公钥哈希，然后将此公钥哈希发送到 CA 并将其存储在 EKPubList 文件夹中。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublicKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>疑难解答  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>密钥证明字段在证书模板上不可用  
如果模板设置不满足证明的要求，则 "密钥证明" 字段不可用。 常见原因如下：  
  
1.  兼容性设置配置不正确。 请确保将其配置如下：  
  
    1.  **证书颁发机构**：**Windows Server 2012 R2**  
  
    2.  **证书接收者**：**Windows 8.1/Windows Server 2012 R2**  
  
2.  加密设置配置不正确。 请确保将其配置如下：  
  
    1.  **提供程序类别**：**密钥存储提供程序**  
  
    2.  **算法名称**：**RSA**  
  
    3.  **提供程序**:**Microsoft 平台加密提供程序**  
  
3.  请求处理设置配置不正确。 请确保将其配置如下：  
  
    1.  不得选择 "**允许导出私钥**" 选项。  
  
    2.  不能选择 "**存档使用者的加密私钥**" 选项。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>验证 TPM 设备的认证  
使用 Windows PowerShell cmdlet **CAEndorsementKeyInfo**来验证特定的 TPM 设备是否受 ca 的证明信任。 有两个选项：一个用于验证 EKCert，另一个用于验证 EKPub。 Cmdlet 可以在 CA 上本地运行，也可以使用 Windows PowerShell 远程处理在远程 Ca 上运行。  
  
1.  若要验证对 EKPub 的信任，请执行以下两个步骤：  
  
    1.  **从客户端计算机提取 EKPub：** 可以通过**TpmEndorsementKeyInfo**从客户端计算机中提取 EKPub。 在提升的命令提示符下，运行以下命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **验证 CA 计算机上的 EKCert 信任：** 将提取的字符串（EKPub 的 SHA-1 哈希）复制到服务器（例如，通过电子邮件），并将其传递给 CAEndorsementKeyInfo cmdlet。 请注意，此参数必须为64个字符。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  若要验证对 EKCert 的信任，请执行以下两个步骤：  
  
    1.  **从客户端计算机提取 EKCert：** 可以通过**TpmEndorsementKeyInfo**从客户端计算机中提取 EKCert。 在提升的命令提示符下，运行以下命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo
        PS C:>\$a.manufacturerCertificates|Export-Certificate -filepath c:\myEkcert.cer
        ```  
  
    2.  **验证 CA 计算机上的 EKCert 信任：** 将提取的 EKCert （EkCert）复制到 CA （例如，通过电子邮件或 xcopy）。 例如，如果将证书文件复制到 CA 服务器上的 "c:\diagnose" 文件夹，请运行以下内容完成验证：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>请参阅  
[受信任的平台模块技术概述](https://technet.microsoft.com/library/jj131725.aspx)  
@no__t 0External 资源：受信任的平台模块 @ no__t-0  
