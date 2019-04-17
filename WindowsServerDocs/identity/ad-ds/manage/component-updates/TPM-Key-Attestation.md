---
ms.assetid: 16a344a9-f9a6-4ae2-9bea-c79a0075fd04
title: "TPM 关键审计"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d08efe825e10d526763a1654c7e090427719c51
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="tpm-key-attestation"></a>TPM 关键审计

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

**作者**：与 Windows 组索旋转器高级支持升级工程师  
  
> [!NOTE]  
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。  
  
## <a name="overview"></a>概述  
支持时为 TPM 保护键已经存在自 Windows 8 中，我们进行了没有机制 ca 加密证实证书申请者专用密钥真正受保护的受信任的平台模块 (TPM)。 此更新使执行该审计并反映中证书颁发该审计 CA。  
  
> [!NOTE]  
> 这篇文章假定熟悉证书模板概念读者 (请参考[证书模板](https://technet.microsoft.com/library/cc730705.aspx))。 它还假定读卡器是如何配置根据模板证书颁发证书企业 Ca 熟悉 (以供参考，请参阅[清单：颁发和管理证书配置 Ca](https://technet.microsoft.com/library/cc771533.aspx))。  
  
### <a name="terminology"></a>术语  
  
|术语|定义|  
|--------|--------------|  
|EK|认可密钥。 这是对称密钥包含在内部 TPM（插入在制造时间）。 EK 唯一的每个 TPM，并且可以来识别它。 EK 无法更改或删除。|  
|EKpub|是指公钥 EK。|  
|EKPriv|引用的 EK 专用键。|  
|EKCert|EK 证书。 TPM 制造商发出证书 EKPub。 并非所有 Tpm 都具有 EKCert。|  
|TPM|受信任的平台模块。 TPM 旨在提供硬件基于安全相关的功能。 TPM 芯片是安全加密的处理器设计用于执行加密的操作。 芯片包含多个物理安全机制，以使其篡改，并且无法篡改 TPM 的安全功能恶意软件。|  
  
### <a name="background"></a>背景  
受信任的平台模块 (TPM) 从 Windows 8 开始，用于安全证书专用密钥。 Microsoft 平台加密提供商键存储提供商 (KSP) 将启用该功能。 有两个关心实现：  

-   却只有一个键实际上受的 TPM（某人可以是软件 KSP 为与本地管理员凭据 TPM KSP 中轻松欺骗）不能保证。

-   无法限制 Tpm 保护企业（的 PKI 管理员，要控制可用于获取环境中的证书的设备类型）颁发的证书允许列表。  

### <a name="tpm-key-attestation"></a>TPM 关键审计  
TPM 关键审计是实体请求证书能够加密向 CA 证明加利福尼亚信任的"a"或者""TPM 受中的证书请求 RSA 键。 在更多讨论 TPM 信任模型[部署概述](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentOverview)按照本主题后面的部分。  
  
### <a name="why-is-tpm-key-attestation-important"></a>为什么很重要 TPM 关键审计？  
带有 TPM attested 键用户证书提供更高版本的安全保障、方法可非导出性、锤反打并隔离键 TPM 提供的备份。  
  
使用 TPM 关键审计，一种新管理模式现在有可能：管理员可以定义用户可用于访问公司资源（如 VPN 或无线接入点），并且拥有的设备的一组**强**保证可以使用任何其他设备访问它们。 此新访问控制范例是**强**因为它绑定到*硬件绑定*用户的身份，这比软件基于的凭据。
  
### <a name="how-does-tpm-key-attestation-work"></a>TPM 关键审计如何工作？  
一般情况下，TPM 关键审计根据以下石柱：  
  
1.  每个 TPM 附带唯一对称键，称为*认可密钥*(EK)，刻录制造商。 作为此项的公用部分，我们将*EKPub*和关联专用密钥作为*EKPriv*。 某些 TPM 芯片还有由 EKPub 制造商 EK 证书。 我们将此证书标记为*EKCert*。  
  
2.  CA 建立信任 TPM 通过 EKPub 或 EKCert 中。  
  
3.  为其请求证书 RSA 键加密与 EKPub 以及用户是否拥有 EKpriv，用户也证明到 CA。  
  
4.  CA 用特殊的颁发策略 OID 表示键现在 attested 受保护由 TPM 问题证书。  
  
## <a name="BKMK_DeploymentOverview"></a>部署概述  
在此部署，假设设置 Windows Server 2012 R2 企业 CA。 此外，配置 (Windows 8.1) 的客户端注册针对该企业 CA 使用证书模板。 

有三个步骤部署 TPM 关键审计：  
  
1.  **计划 TPM 信任模式：**第一步是决定使用哪个 TPM 信任型号。 有支持执行此操作的三种方法：  
  
    -   **信任基于用户凭据：**企业加利福尼亚信任作为证书请求的一部分的用户提供 EKPub 和执行没有验证之外的用户的域的凭据。  
  
    -   **根据 EKCert 的信任：**企业 CA 验证管理员管理列表作为证书请求的一部分提供的 EKCert 链*接受 EK 的证书链*。 接受链是定义每个制造商，而在（中间一个应用商店）和一个用于根证书颁发表达通过自定义证书两个应用商店。 此信任模式意味着**所有**Tpm 给定的制造商提供的受信任。 请注意，在此模式中，在环境中使用 Tpm 必须包含 EKCerts。
  
    -   **根据 EKPub 的信任：**企业 CA 验证时的管理员管理列表中显示的证书请求的一部分提供 EKPub 允许 EKPubs。 作为此目录中的每个文件的名称的位置设置允许 EKPub 的 SHA-2 哈希文件的目录表示此列表。 此选项提供的最高的保障级别，但需要管理更多的工作，因为每台设备分别识别出。 在此信任模型，已添加到允许列表中 EKPubs 其 TPM EKPub 设备都可以注册 TPM attested 证书。  
  
    具体取决于使用哪种方法，CA 将不同颁发策略应用 OID 到颁发的证书。 有关颁发策略 Oid 的更多详细信息，请参阅颁发策略 Oid 表中的[配置证书模板](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_ConfigCertTemplate)本主题中的部分。  
  
    请注意，可以选择 TPM 信任型号的组合。 在此情况下，CA 接受任一审计方法和 Oid 将反映成功的所有审计方法颁发策略。  
  
2.  **配置证书模板：**中所述配置证书模板[部署详细信息](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_DeploymentDetails)本主题中的部分。 这篇文章不键盘盖该证书模板赋予企业 CA 或如何注册提供一组用户访问。 有关详细信息，请参阅[清单：颁发和管理证书配置 Ca](https://technet.microsoft.com/library/cc771533.aspx)。  
  
3.  **配置为 TPM 信任模型 CA**  
  
    1.  **信任基于用户凭据：**不需要任何特定的配置。  
  
    2.  **根据 EKCert 的信任：**管理员必须从 TPM 制造商获取 EKCert 链证书，并将其导入到两个新证书存储由管理员，CA 执行 TPM 关键审计上创建。 有关详细信息，请参阅[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    3.  **根据 EKPub 的信任：**管理员必须为每台设备，将需要 TPM attested 证书，并将他们添加到允许 EKPubs 列表 EKPub 处获取。 有关详细信息，请参阅[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    > [!NOTE]  
    > -   此功能需要 Windows 8.1 月 Windows Server 2012 R2。  
    > -   有关第三方智能卡 Ksp TPM 关键审计不受支持。 Microsoft 必须使用平台加密提供商 KSP。  
    > -   TPM 关键审计仅用于 RSA 键。  
    > -   TPM 关键审计不支持独立 CA。  
    > -   不支持 TPM 关键审计[非永久性证书处理](https://technet.microsoft.com/library/ff934598)。  
  
## <a name="BKMK_DeploymentDetails"></a>部署详细信息  
  
### <a name="BKMK_ConfigCertTemplate"></a>配置证书模板  
若要配置 TPM 关键审计的证书模板，执行下列配置步骤操作：  
  
1.  **兼容性**选项卡  
  
    在**兼容性设置**部分：  
  
    -   确保**Windows Server 2012 R2**为选择**证书颁发机构**。  
  
    -   确保**Windows 8.1 月 Windows Server 2012 R2**为选择**证书收件人**。  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_CompatibilityTab.gif)  
  
2.  **加密**选项卡  
  
    确保**键存储提供商**为选择**提供商类别**和**RSA**为选择**算法名称**。 确保**请求必须使用下面提供商之一**选中和**Microsoft 平台加密提供商**下选择选项**提供商**。  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_CryptoTab.gif)  
  
3.  **键审计**选项卡  
  
    这是 Windows Server 2012 R2 的新选项卡：  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_ConfigCertTemplate.gif)  
  
    从三个可能的选项中选择审计模式。  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_KeyModes.gif)  
  
    -   **无：**意味着关键审计不得使用  
  
    -   **需要，如果客户支持：**允许用户 TPM 不支持的设备上的键审计继续该证书的注册。 可以执行审计的用户将区分用特殊的颁发 OID 策略中。 一些设备可能无法执行关键审计或并非在所有时遇到 TPM 设备不支持的旧 TPM 由于审计。
  
    -   **所需：**客户端*必须*执行 TPM 关键审计，否则将无法证书请求。  
  
    然后选择 TPM 信任型号。 再次有三个选项：  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_KeyTypeToEnforce.gif)  
  
    -   **用户凭据：**允许身份验证的用户，若要通过指定其域凭据有效 TPM 保证。  
  
    -   **认可证书：** EKCert 的设备必须验证通过管理员管理 TPM 中间 CA 证书到管理员管理根证书。 如果您选择此选项，你必须设置 EKCA 和 EKRoot 证书颁发在应用商店中所述[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    -   **认可密钥：**设备的 EKPub 必须 PKI 管理员管理列表中显示。 此选项提供的最高的保障级别，但需要管理的更多工作。 如果您选择此选项，你必须设置上颁发 EKPub 列表中所述[CA 配置](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md#BKMK_CAConfig)本主题中的部分。  
  
    最后，确定证书颁发中显示哪些颁发策略。 默认情况下，每个强制类型已将在下表中所述，它会通过该强制类型，如果插入到证书对象关联的标识符 (OID)。 请注意，可以选择强制方法的组合。 在此情况下，CA 将接受任一审计方法，并且颁发策略 OID 反映成功的所有审计方法。  
  
    **颁发策略 Oid**  
  
    |OID|关键审计类型|描述|保障级别|  
    |-------|------------------------|---------------|-------------------|  
    |1.3.6.1.4.1.311.21.30|EK|"EK 已验证": 有关 EK 管理员管理列表|高|  
    |1.3.6.1.4.1.311.21.31|认可证书|"EK 证书已验证": 验证 EK 的证书链时|中等|  
    |1.3.6.1.4.1.311.21.32|用户凭据|"EK 受信任的上使用": 对于用户 attested EK|低|  
  
    Oid 将插入颁发的证书，如果**包括颁发策略**已选择（默认配置）。  
  
    ![TPM 键审计](media/TPM-Key-Attestation/GTR_ADDS_IssuancePolicies.gif)  
  
    > [!TIP]  
    > 在证书存在 OID 时遇到的使用一个可能是限制访问 VPN 或无线网络到某些设备。 例如，你访问的策略中可能允许连接（或访问其他 VLAN）OID 1.3.6.1.4.1.311.21.30 是否出现在证书。 这允许你限制访问其 TPM EK 已存在于 EKPUB 列表中的设备。  
  
### <a name="BKMK_CAConfig"></a>加拿大配置  
  
1.  **设置在颁发 EKCA 和 EKROOT 证书应用商店**  
  
    如果你选择**认可证书**模板设置中，请执行下列配置步骤：  
  
    1.  使用 Windows PowerShell 将执行 TPM 关键审计认证颁发机构服务器上创建新的两个证书官方商城。  
  
    2.  获取中间并根 CA 你想要允许在你的企业环境的制造商出品的证书。 必须将这些证书导入相应地先前已创建证书存储（EKCA 和 EKROOT）中。  
  
    以下 Windows PowerShell 脚本可执行两个步骤。 在以下示例中，TPM 制造商 Fabrikam 提供根证书*FabrikamRoot.cer*和颁发的 CA 证书*Fabrikamca.cer*。  
  
    ```powershell  
    PS C:>\cd cert:  
    PS Cert:\>cd .\\LocalMachine  
    PS Cert:\LocalMachine> new-item EKROOT  
    PS Cert:\ LocalMachine> new-item EKCA  
    PS Cert:\EKCA\copy FabrikamCa.cer .\EKCA  
    PS Cert:\EKROOT\copy FabrikamRoot.cer .\EKROOT  
    ```  
  
2.  **如果使用的 EK 审计的键入，设置 EKPUB 列表**  
  
    如果你选择**认可密钥**模板设置中，在下一步配置步骤创建和配置颁发，包含 0 字节文件的文件夹，每个名字取自允许 EK SHA-2 希代码。 该文件夹可以作为允许获取 TPM 键 attested 证书的设备的"允许列表"。 你必须手动添加为每个设备需要 attested 的证书 EKPUB，因为它提供企业版的设备有权获取 TPM 关键 attested 的证书保证。 配置适用于此模式 CA 需要两个步骤：  
  
    1.  **创建 EndorsementKeyListDirectories 注册表项：**使用 Certutil 命令行工具来配置了受信任的 EKpubs 定义下表中所述的文件夹位置。  
  
        |操作|语法命令|  
        |-------------|------------------|  
        |添加文件夹位置|Certutil.exe-setreg CA\EndorsementKeyListDirectories +"<folder>"|  
        |删除文件夹位置|Certutil.exe-setreg CA\EndorsementKeyListDirectories-"<folder>"|  
  
        在命令 certutil EndorsementKeyListDirectories 是注册表设置下表中所述。  
  
        |值名称|键入|数据|  
        |--------------|--------|--------|  
        |EndorsementKeyListDirectories|REG_MULTI_SZ|< 本地或 UNC 通往 EKPUB 允许列表 ><br /><br />示例：<br /><br />*\\\blueCA.contoso.com\ekpub*<br /><br />*\\\bluecluster1.contoso.com\ekpub*<br /><br />D:\ekpub|  
  
        HKLM\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\\<CA Sanitized Name>  
  
        *EndorsementKeyListDirectories*将包含 UNC 或列表本地文件的系统路径，每个指向 CA 有权读取一个文件夹。 每个文件夹可能包含零或更多允许列表项，其中每项是 SHA-2 的同名文件的受信任的 EKpub 希无文件扩展名。 
        创建或编辑此注册表关键配置要求 CA，就像现有 CA 注册表配置设置重新启动。 但是，编辑配置设置将立即生效，将不需要重新启动 CA。  
  
        > [!IMPORTANT]  
        > 安全篡改从列表中的文件夹和配置权限，以便仅授权管理员的未授权的访问已经阅读并写入访问权限。 加拿大的计算机帐户要求读取访问权限。  
  
    2.  **填充 EKPUB 列表：**使用下面的 Windows PowerShell cmdlet 获取 TPM EK 的公用密钥哈希每台设备上使用 Windows PowerShell，然后发送此公用键哈希 CA 与并将其存储在 EKPubList 文件夹。  
  
        ```powershell  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        PS C:>$b=new-item $a.PublickKeyHash -ItemType file  
        ```  
  
## <a name="troubleshooting"></a>故障排除  
  
### <a name="key-attestation-fields-are-unavailable-on-a-certificate-template"></a>关键审计字段上不可用证书模板  
键审计字段不可用，如果模板设置不满足审计的要求。 常见原因是：  
  
1.  兼容性设置不正确配置。 请确保配置，如下所示：  
  
    1.  **证书颁发机构**: **Windows Server 2012 R2**  
  
    2.  **证书收件人**: **Windows 8.1 月 Windows Server 2012 R2**  
  
2.  未正确配置的加密设置。 请确保配置，如下所示：  
  
    1.  **提供商类别**:**键存储提供商**  
  
    2.  **算法名称**: **RSA**  
  
    3.  **提供商**: **Microsoft 平台的加密提供程序**  
  
3.  请求处理设置不正确配置。 请确保配置，如下所示：  
  
    1.  **允许专用键导出**不能选择的选项。  
  
    2.  **存档主题的密钥专用**不能选择的选项。  
  
### <a name="verification-of-tpm-device-for-attestation"></a>对于审计 TPM 设备的验证  
使用 Windows PowerShell cmdlet、**确认 CAEndorsementKeyInfo**，以验证 TPM 特定设备是否受信任的审计由 Ca。 有两个选项：一个用于验证 EKCert，以及用于验证 EKPub 其他。 Cmdlet 是本地在加拿大上或运行远程 ca 通过使用 Windows PowerShell 远程处理。  
  
1.  用于验证上 EKPub 信任，请执行以下两个步骤操作：  
  
    1.  **EKPub 提取客户端计算机：** EKPub 可提取客户端计算机通过从**获取 TpmEndorsementKeyInfo**。 从提升了权限的命令提示符运行以下命令：  
  
        ```  
        PS C:>\$a=Get-TpmEndorsementKeyInfo -hashalgorithm sha256  
        ```  
  
    2.  **验证上 EKCert CA 计算机上的信任：**复制到（例如，通过电子邮件）服务器的提取的字符串（EKPub SHA-2 希）并将其传递到确认 CAEndorsementKeyInfo cmdlet。 请注意，此参数必须 64 字符。  
  
        ```  
        Confirm-CAEndorsementKeyInfo [-PublicKeyHash] <string>  
        ```  
  
2.  用于验证上 EKCert 信任，请执行以下两个步骤操作：  
  
    1.  **EKCert 提取客户端计算机：** EKCert 可提取客户端计算机通过从**获取 TpmEndorsementKeyInfo**。 从提升了权限的命令提示符运行以下命令：  
  
        ```  
        PS C:>\$a= Get- TpmEndorsementKeyInfo  
        PS C:>\$a.manufacturerCertificates|Export-Certificate c:\myEkcert.cer  
        ```  
  
    2.  **验证上 KCert CA 计算机上的信任：**将提取的 EKCert (EkCert.cer) 复制到（例如，通过电子邮件或 xcopy）CA。 例如，如果复制 CA 服务器上的证书文件"c:\diagnose"文件夹，运行以下完成验证命令：  
  
        ```  
        PS C:>new-object System.Security.Cryptography.X509Certificates.X509Certificate2 "c:\diagnose\myEKcert.cer" | Confirm-CAEndorsementKeyInfo  
        ```  
  
## <a name="see-also"></a>请参阅  
[受信任的平台模块技术概述](https://technet.microsoft.com/library/jj131725.aspx)  
[受信任的平台模块的外部资源：](http://www.cs.unh.edu/~it666/reading_list/Hardware/tpm_fundamentals.pdf)  
  


