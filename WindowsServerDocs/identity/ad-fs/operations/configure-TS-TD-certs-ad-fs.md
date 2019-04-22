---
title: 获取和配置适用于 AD FS 令牌签名证书和令牌解密证书
description: 本文档介绍如何获取并配置 TS 和 TD 适用于 AD FS 证书
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820208"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>获取并配置 TS 和 TD 证书适用于 AD FS

本主题介绍的任务和可执行以确保你的 AD FS 令牌签名和令牌解密证书是最新的过程。

令牌签名证书是标准 X509 证书用于安全地对联合服务器颁发的所有令牌进行都签名。 令牌解密证书是标准 X509 证书用于解密任何传入令牌。 它们也会发布在联合身份验证元数据。

有关其他信息，请参阅[证书要求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>确定 AD FS 是否自动续订证书
默认情况下，AD FS 配置为在初始配置时和在证书接近到期日期时自动生成令牌签名证书和令牌解密证书。

可以运行以下 Windows PowerShell 命令： `Get-AdfsProperties`。
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
AutoCertificateRollover 属性描述是否配置了 AD FS 令牌签名证书和令牌解密证书自动续订。

如果 AutoCertificateRollover 设置为 TRUE，则将续订 AD FS 证书，并将其自动在 AD FS 中配置。 新的证书配置后，要避免服务中断，您必须确保，使用此新证书更新 （由信赖方信任或声明提供方信任表示 AD FS 场中） 每个联合身份验证伙伴。
    
如果 AD FS 不是配置为续订令牌签名和令牌解密证书自动 （如果 AutoCertificateRollover 设置为 False），使用 AD FS 将不会自动生成或开始使用新的令牌签名或令牌解密证书。 必须手动执行这些任务。
    
如果配置了 AD FS 令牌签名证书和令牌解密证书自动续订 （AutoCertificateRollover 设置为 TRUE），您可以确定续订时：

CertificateGenerationThreshold 介绍了多少天之前的证书不晚于日期的情况下将生成新证书。

CertificatePromotionThreshold 确定多少天后生成新证书将被提升为主要证书 （即，AD FS 将开始使用它则会颁发的令牌进行签名并解密从标识提供者的令牌）。

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
如果配置了 AD FS 令牌签名证书和令牌解密证书自动续订 （AutoCertificateRollover 设置为 TRUE），您可以确定续订时：

 - **CertificateGenerationThreshold**描述证书的不晚于日期提前的多少天的情况下将生成新证书。
 - **CertificatePromotionThreshold**确定多少天后生成新证书将被提升为主要证书 （即，AD FS 将开始使用它则会颁发的令牌进行签名并解密从标识的令牌提供程序）。

## <a name="determine-when-the-current-certificates-expire"></a>确定在当前证书过期后
若要标识的主要令牌签名和令牌解密证书并确定在当前证书过期后，可以使用以下过程。

可以运行以下 Windows PowerShell 命令： `Get-AdfsCertificate –CertificateType token-signing` (或`Get-AdfsCertificate –CertificateType token-decrypting `)。 或者，你可以检查在 MMC 中的当前证书：服务-> 证书。

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

为其证书**IsPrimary**值设置为**True**是当前正在使用 AD FS 的证书。

为显示的日期**不晚于**必须配置新的主令牌签名或解密证书的日期。

若要确保服务连续性，所有联合身份验证合作伙伴 （按信赖方信任或声明提供方信任表示 AD FS 场中） 必须使用新令牌签名证书和令牌解密证书过期之前。 我们建议在开始规划此过程至少提前 60 天。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>生成一个新的自签名的证书的宽限期结束之前手动
以下步骤可用于生成的宽限期结束之前手动新的自签名的证书。

1. 确保你登录到主 AD FS 服务器。
2. 打开 Windows PowerShell 并运行以下命令： `Add-PSSnapin "microsoft.adfs.powershell"`
3. （可选） 可以检查 AD FS 中的当前签名证书。 若要执行此操作，请运行以下命令： `Get-ADFSCertificate –CertificateType token-signing`。 查看命令输出以查看列出的任何证书的不晚于日期。
4. 若要生成新证书，请执行以下命令以续订并更新 AD FS 服务器上的证书： `Update-ADFSCertificate –CertificateType token-signing`。
5. 通过再次运行以下命令来验证更新： `Get-ADFSCertificate –CertificateType token-signing`
6. 此时会列出两个证书，其中之一已**不之后**日期大约为未来一年，为其**IsPrimary**值是**False**。

>[!IMPORTANT]
>若要避免服务中断，请使用有效的令牌签名证书更新 Azure AD 中如何运行的步骤更新 Azure AD 上的证书信息。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果不使用自签名的证书...
如果不使用默认的自动生成、 自签名令牌签名证书和令牌解密证书，必须续订并手动配置这些证书。

首先，必须从证书颁发机构获取新证书并将其导入每个联合身份验证服务器上的本地计算机个人证书存储。 有关说明，请参阅[导入证书](https://technet.microsoft.com/library/cc754489.aspx)一文。

然后必须配置此证书作为辅助 AD FS 令牌签名或解密证书。 （您将其配置为辅助证书以允许你的联合身份验证合作伙伴足够的时间来使用此新证书之前将其提升为主证书）。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>若要配置新证书作为辅助证书
1. 打开 PowerShell 并运行以下命令： `Set-ADFSProperties -AutoCertificateRollover $false`
2. 一次已导入证书。 打开**AD FS 管理**控制台。
3. 展开**服务**，然后选择**证书**。
4. 在操作窗格中单击**添加令牌签名证书**。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 从显示的证书的列表中选择新证书，然后单击确定。
6.  打开 PowerShell 并运行以下命令： `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>请确保新证书具有与之关联的私钥，AD FS 服务帐户被授予读取权限的私钥。 每个联合身份验证服务器上对此进行验证。 为此，请在证书管理单元中，右键单击新证书，单击所有任务，然后都单击管理私钥。

一旦已允许足够的时间进行联合身份验证合作伙伴以使用新证书 （它们拉取联合元数据或将其发送新的证书的公钥），必须升级为主要证书的辅助证书。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>若要升级辅助站点到主站点中的新证书

1. 打开**AD FS 管理**控制台。
2. 展开**服务**，然后选择**证书**。
3. 单击辅助令牌签名证书。
4. 在中**操作**窗格中，单击**设为主**。 在确认提示中单击是。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>更新联合合作伙伴

### <a name="partners-who-can-consume-federation-metadata"></a>合作伙伴可以使用联合身份验证元数据
如果已续订，并配置新的令牌签名或令牌解密证书，必须确保所有联合合作伙伴 (资源的组织或帐户的组织的合作伙伴表示中的 AD FS 的信赖方信任和声明提供方信任） 染上了新的证书。

### <a name="partners-who-can-not-consume-federation-metadata"></a>合作伙伴可以不使用联合身份验证元数据
如果你的联合身份验证合作伙伴不能使用你的联合身份验证元数据，您必须手动发送您的新令牌签名 / 令牌解密证书的公钥。 将新证书的公钥 （.cer 文件或如果想要包括的整个链的.p7b） 发送到所有资源组织或帐户 （由中的 AD FS 信赖方信任和声明提供方信任） 的组织伙伴。 已在他们那边信任新的证书实现更改合作伙伴。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>升级到主副本 （如果 AutoCertificateRollover 为 False）
如果**AutoCertificateRollover**设置为**False**、 AD FS 将不会自动生成或开始使用新令牌签名或令牌解密证书。 必须手动执行这些任务。
所有联合合作伙伴以使用新的辅助证书允许足够的时间内之后, 将提升为主要副本 （在 MMC 管理单元中，单击辅助令牌签名证书，然后在操作窗格中单击此辅助证书**将设为主**。)

## <a name="updating-azure-ad"></a>更新 Azure AD
AD FS 提供到 Office 365 等 Microsoft 云服务的单一登录访问由用户通过其现有的 AD DS 凭据进行身份验证。  使用证书的其他信息，请参阅[续订 Office 365 和 Azure AD 的联合身份验证证书](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。