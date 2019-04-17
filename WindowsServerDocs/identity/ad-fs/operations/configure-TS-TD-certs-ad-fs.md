---
title: "获取并广告 fs 配置令牌签名和令牌解密证书"
description: "本文介绍了如何获取和配置 TS 和 TD 广告 fs 证书"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>获取并广告 fs 配置 TS 和 TD 证书

本主题介绍任务以及你可以执行以确保你的广告 FS 令牌签名和令牌解密证书保持最新状态的过程。

令牌签名证书有标准 X509 证书用于安全地登录联合身份验证的服务器问题的所有标记。 标记解密证书都是标准 X509 证书用于解密任何传入标记。 他们还联盟元数据中发布。

有关其他信息，请参阅[证书要求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>确定是否广告 FS 自动续订证书
默认情况下，广告 FS 配置为在初始配置时并在到期日期接近证书时自动生成令牌签名和令牌解密证书。

你可以通过运行以下 Windows PowerShell 命令：`Get-AdfsProperties`。
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
AutoCertificateRollover 属性描述广告 FS 是否配置为续订令牌签名和令牌自动解密证书。

如果 AutoCertificateRollover 为 TRUE 设置，将续订广告 FS 证书，并自动配置广告 FS 中。 一旦新证书配置，以避免中断，你必须确保（由广告 FS 场中信赖方信任或索赔提供商信任）每个联盟合作伙伴更新此新证书。
    
如果广告 FS 未配置为续订令牌签名，并且令牌解密证书自动（如果 AutoCertificateRollover 设置为 False），广告 FS 不会自动将生成或开始使用新令牌签名或令牌解密证书。 你将需要手动执行这些任务。
    
如果广告 FS 配置了续订令牌签名和令牌自动解密证书（AutoCertificateRollover 设为 TRUE），你可以决定续订时：

CertificateGenerationThreshold 介绍了多少天的证书不后日期提前将生成一个新的证书。

CertificatePromotionThreshold 确定后的数天生成新的证书将会提升为主要证书（换句话说，广告 FS 将开始使用它来登录的标记它问题和解密令牌从身份提供商）。

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
如果广告 FS 配置了续订令牌签名和令牌自动解密证书（AutoCertificateRollover 设为 TRUE），你可以决定续订时：

 - **CertificateGenerationThreshold**介绍了多少天的证书不后日期提前将生成一个新的证书。
 - **CertificatePromotionThreshold**确定后的数天生成新的证书将会提升为主要证书（换句话说，广告 FS 将开始使用它来登录的标记它问题和解密令牌从的身份提供程序）。

## <a name="determine-when-the-current-certificates-expire"></a>确定时的当前证书到期
可以使用下面的过程，以找出主要令牌签名和令牌解密证书并确定时的当前证书过期。

你可以通过运行以下 Windows PowerShell 命令：`Get-AdfsCertificate –CertificateType token-signing` (或`Get-AdfsCertificate –CertificateType token-decrypting `)。 你可以检查 MMC 中的当前证书或者：服务-> 证书。

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

证书**IsPrimary**值设置为**如此**是广告 FS 当前正在使用的证书。

针对显示日期**不后**为新的主令牌签名，或解密证书必须配置的日期。

若要确保服务连续性，所有联盟合作伙伴（由广告 FS 场中信赖方信任或索赔提供商信任）必须都消耗的新令牌签名和令牌解密前此过期的证书。 我们建议你开始规划至少 60 天事先完成此过程。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>生成新的自签名的证书，手动之前的宽限期内末尾
可以使用以下步骤来生成新的自签名的证书，手动之前的宽限期内终止。

1. 确保你的登录到广告 FS 主服务器。
2. 打开 Windows PowerShell 并运行以下命令： `Add-PSSnapin "microsoft.adfs.powershell"`
3. （可选）你还可以查看广告 FS 中当前的签名证书。 若要执行此操作，运行以下命令：`Get-ADFSCertificate –CertificateType token-signing`。 查看命令输出以查看任何列出的证书不后日期。
4. 要生成一个新的证书，请执行以下命令以续订，并更新广告 FS 服务器上的证书：`Update-ADFSCertificate –CertificateType token-signing`。
5. 通过运行以下命令再次验证更新： `Get-ADFSCertificate –CertificateType token-signing`
6. 现在应列出两个证书，其中一个具有**不后**日期大约一年将来和为其**IsPrimary**值**False**。

>[!IMPORTANT]
>为了避免服务中断，请更新 Azure AD 有关的证书通过如何在运行这些步骤更新 Azure AD 具有有效令牌签名证书。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果你不使用自签名的证书…
如果你未使用自动生成默认、自签名令牌签名和令牌解密证书，你必须续订并手动配置这些证书。

首先，你必须从你的证书颁发机构获取新的证书，并将其导入的每个联合身份验证的服务器上的本地计算机个人证书的应用商店。 有关说明，请参阅[将证书导入](https://technet.microsoft.com/library/cc754489.aspx)文章。

然后，你必须配置此作为第二广告 FS 令牌签名证书或解密证书。 （你将其配置作为第二证书允许联盟合作伙伴足够的时间来使用此新证书之前将其提交到主证书）。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>要将新证书配置为辅助证书
1. 打开 PowerShell 并运行以下命令： `Set-ADFSProperties -AutoCertificateRollover $false`
2. 一次你已导入证书。 打开**广告 FS 管理**主机。
3. 展开**服务**，然后选择**证书**。
4. 在操作窗格中，单击**添加令牌签名证书**。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 从列表中显示的证书选择新的证书，然后单击确定。
6.  打开 PowerShell 并运行以下命令： `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>确保新证书有与之关联的专用键并广告 FS 服务帐户被授予阅读专用项的权限。 验证在每个联合身份验证的服务器上。 若要执行此操作，证书贴靠中，右键单击新的证书，所有任务，请都单击，然后都单击管理专用键。

您已允许你联盟合作伙伴消耗（他们拉你联盟元数据或向他们发送你的新证书公钥）你新证书足够时间，一旦你必须将升级到主证书辅助证书。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>若要将新的证书提升从次要的主要

1. 打开**广告 FS 管理**主机。
2. 展开**服务**，然后选择**证书**。
3. 单击辅助令牌签名证书。
4. 在**操作**窗格中，单击**设为主**。 在确认提示符下，单击是。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>更新联盟合作伙伴

### <a name="partners-who-can-consume-federation-metadata"></a>可以使用联盟元数据的合作伙伴
如果你有续订，并且配置新令牌签名或令牌解密证书，你必须确保所有你联盟合作伙伴 (资源组织或帐户组织的合作伙伴依赖表示广告 FS 中方的信任和新的证书有选取索赔提供商信任）。

### <a name="partners-who-can-not-consume-federation-metadata"></a>不能使用联盟元数据的合作伙伴
如果联盟合作伙伴无法使用你的联盟元数据，你必须手动向他们发送你的新令牌签名 / 令牌解密证书公钥。 将你新建的证书公钥发送 (.cer 文件或.p7b，如果你希望包含整个链) 资源组织或帐户组织合作伙伴的所有 (在广告 FS 由依赖方信任和索赔提供程序信任）。 有信任新证书其一侧实现更改的合作伙伴。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>（如果 AutoCertificateRollover False），升级到主
如果**AutoCertificateRollover**设置为**False**，不会自动生成广告 FS 或开始使用新令牌签名或标记解密证书。 你将需要手动执行这些任务。
在所有你联盟合作伙伴消耗的新辅助证书允许足够时间之后, 推广到主（在 MMC 贴靠中，单击辅助令牌签名证书，在操作窗格中，单击此辅助证书**设置为主要**。)

## <a name="updating-azure-ad"></a>更新 Azure AD
广告 FS 通过单一登录访问 Microsoft 云服务，如 Office 365 通过其现有的广告 DS 凭据用户身份验证。  有关使用证书的其他信息，请参阅[续订 Office 365 和 Azure AD 联合身份验证证书](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。