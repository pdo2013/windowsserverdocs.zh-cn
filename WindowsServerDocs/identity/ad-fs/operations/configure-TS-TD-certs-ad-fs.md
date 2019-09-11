---
title: 获取并配置 AD FS 的令牌签名和令牌解密证书
description: 本文档介绍如何获取和配置 AD FS 的 TS 和 TD 证书
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 608f78ed03bc102cbdffb8abcc52244450b97b3c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865633"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>获取和配置 AD FS 的 TS 和 TD 证书

本主题介绍了可执行的任务和过程，以确保 AD FS 令牌签名和令牌解密证书是最新的。

令牌签名证书是标准 X509 证书，用于安全地对联合服务器颁发的所有令牌进行签名。 令牌解密证书是用于解密任何传入令牌的标准 X509 证书。 它们还在联合元数据中发布。

有关其他信息，请参阅[证书要求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>确定 AD FS 是否自动续订证书
默认情况下，AD FS 配置为在初始配置时以及证书接近其到期日期时自动生成令牌签名和令牌解密证书。

您可以运行以下 Windows PowerShell 命令： `Get-AdfsProperties`。
  
  ![Set-adfsproperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
AutoCertificateRollover 属性描述 AD FS 是否配置为自动续订令牌签名和令牌解密证书。

如果将 AutoCertificateRollover 设置为 TRUE，则将在 AD FS 自动续订和配置 AD FS 证书。 一旦配置新证书，就必须确保使用此新证书更新每个联合伙伴（由信赖方信任或声明提供方信任在 AD FS 场中表示）。
    
如果 AD FS 未配置为自动续订令牌签名和令牌解密证书（如果 AutoCertificateRollover 设置为 False），则 AD FS 不会自动生成或开始使用新的令牌签名或令牌解密证书。 你必须手动执行这些任务。
    
如果将 AD FS 配置为自动续订令牌签名和令牌解密证书（AutoCertificateRollover 设置为 TRUE），则可以确定何时续订这些证书：

CertificateGenerationThreshold 描述了在日期后，将生成新证书的日期前的天数。

CertificatePromotionThreshold 确定生成新证书后要将其提升为主证书的天数（换言之，AD FS 会开始使用该证书对其颁发的令牌进行签名，并将令牌从标识提供者进行解密）。

![Set-adfsproperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
如果将 AD FS 配置为自动续订令牌签名和令牌解密证书（AutoCertificateRollover 设置为 TRUE），则可以确定何时续订这些证书：

 - **CertificateGenerationThreshold**描述了在日期后，将生成新证书的日期前的天数。
 - **CertificatePromotionThreshold**确定生成新证书后要将其提升为主证书的天数（换言之，AD FS 会开始使用该证书对它颁发的令牌进行签名，并从标识对令牌进行解密提供程序）。

## <a name="determine-when-the-current-certificates-expire"></a>确定当前证书何时过期
你可以使用以下过程来确定主要令牌签名和令牌解密证书，并确定当前证书的过期时间。

您可以运行以下 Windows PowerShell 命令： `Get-AdfsCertificate –CertificateType token-signing` （或`Get-AdfsCertificate –CertificateType token-decrypting `）。 或者，你可以在 MMC 中检查当前证书：服务-> 证书。

![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

**IsPrimary**值设置为**True**的证书是 AD FS 当前正在使用的证书。

"**不晚**于" 的显示日期是必须配置新的主令牌签名或解密证书的日期。

为了确保服务连续性，在此过期之前，所有联合合作伙伴（由信赖方信任或声明提供方信任在 AD FS 场中表示）必须使用新的令牌签名和令牌解密证书。 建议提前计划至少60天进行此过程。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>宽限期结束之前手动生成新的自签名证书
你可以使用以下步骤在宽限期结束之前手动生成新的自签名证书。

1. 确保你已登录到主 AD FS 服务器。
2. 打开 Windows PowerShell 并运行以下命令：`Add-PSSnapin "microsoft.adfs.powershell"`
3. 还可以选择在 AD FS 中检查当前签名证书。 为此，请运行以下命令： `Get-ADFSCertificate –CertificateType token-signing`。 查看命令输出，查看列出的任何证书的不在日期之后。
4. 若要生成新证书，请执行以下命令以续订和更新 AD FS 服务器上的证书： `Update-ADFSCertificate –CertificateType token-signing`。
5. 再次运行以下命令，验证更新：`Get-ADFSCertificate –CertificateType token-signing`
6. 现在应会列出两个证书，其中一个证书的日期**不能晚**于将来的一年，并且**IsPrimary**值为**False**。

>[!IMPORTANT]
>若要避免服务中断，请通过运行如何使用有效的令牌签名证书更新 Azure AD 中的步骤来更新 Azure AD 上的证书信息。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果使用的不是自签名证书 。
如果未使用默认自动生成的自签名令牌签名和令牌解密证书，则必须手动续订和配置这些证书。

首先，你必须从证书颁发机构获取新证书，并将其导入到每个联合服务器上的本地计算机个人证书存储中。 有关说明，请参阅[导入证书一](https://technet.microsoft.com/library/cc754489.aspx)文。

然后，必须将此证书配置为辅助 AD FS 令牌签名或解密证书。 （将其配置为辅助证书，以便在将其升级到主证书之前，允许联合伙伴有足够的时间来使用此新证书）。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>将新证书配置为辅助证书
1. 打开 PowerShell 并运行以下内容：`Set-ADFSProperties -AutoCertificateRollover $false`
2. 导入证书后。 打开**AD FS 管理**控制台。
3. 展开 "**服务**"，然后选择 "**证书**"。
4. 在 "操作" 窗格中，单击 "**添加令牌签名证书**"。
![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 从显示的证书列表中选择新证书，然后单击 "确定"。
6.  打开 PowerShell 并运行以下内容：`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>确保新证书具有与之关联的私钥，并确保向 AD FS 的服务帐户授予对私钥的读取权限。 请在每个联合服务器上进行验证。 为此，请在 "证书" 管理单元中，右键单击新证书，单击 "所有任务"，然后单击 "管理私钥"。

为联合身份验证伙伴提供足够的时间来使用新证书（他们请求联合元数据或向其发送新证书的公钥）后，必须将辅助证书升级为主证书。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>将新证书从辅助证书升级为主证书

1. 打开**AD FS 管理**控制台。
2. 展开 "**服务**"，然后选择 "**证书**"。
3. 单击 "辅助令牌签名证书"。
4. 在 "**操作**" 窗格中，单击 "**设为主**"。 在确认提示时单击 "是"。
![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>更新联合伙伴

### <a name="partners-who-can-consume-federation-metadata"></a>可以使用联合元数据的合作伙伴
如果你已经续订并配置了新的令牌签名或令牌解密证书，则必须确保你的所有联合合作伙伴（资源组织或帐户组织合作伙伴）在你的 AD FS 中由信赖方信任和声明提供方信任）已选取新证书。

### <a name="partners-who-can-not-consume-federation-metadata"></a>不能使用联合元数据的合作伙伴
如果联合身份验证伙伴不能使用联合元数据，则必须手动向其发送新的令牌签名/令牌解密证书的公钥。 将新的证书公钥（.cer 文件或. p7b）发送给所有资源组织或帐户组织合作伙伴（由信赖方信任和声明提供方信任在 AD FS 中表示）。 让合作伙伴在其端实施更改以信任新证书。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>提升为主要副本（如果 AutoCertificateRollover 为 False）
如果**AutoCertificateRollover**设置为**False**，则 AD FS 不会自动生成或开始使用新的令牌签名或令牌解密证书。 你必须手动执行这些任务。
允许所有联合身份验证伙伴使用新的辅助证书时，请将此辅助证书升级为主证书，并在 MMC 管理单元中单击辅助令牌签名证书，然后在 "操作" 窗格中，单击**设置为主**。）

## <a name="updating-azure-ad"></a>更新 Azure AD
AD FS 通过用户现有 AD DS 凭据对用户进行身份验证，从而提供对 Microsoft 云服务（如 Office 365）的单一登录访问。  有关使用证书的其他信息，请参阅[续订 Office 365 和 Azure AD 的联合身份验证证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。