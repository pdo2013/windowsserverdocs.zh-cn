---
ms.assetid: 0f7e56fe-1cbc-43ff-bb87-f4672137ed7f
title: "广告金融服务和 Azure 多重身份验证"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 67ce41bbd351bedc8d87e31ddb79c48338e44de9
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: zh-CN
---
# <a name="ad-fs-and-azure-multi-factor-authentication"></a>广告金融服务和 Azure 多重身份验证

>适用于： Windows Server 2016

在 Windows Server 2016 的广告 FS 引入了，并基于引入与 Windows Server 2012 R2 的多重身份验证功能。   通过内置的 Azure MFA 适配器、安装和配置为与主要身份验证使用 Azure MFA 引入提供商从未如此简单。

## <a name="why-use-ad-fs-with-azure-mfa"></a>为什么使用 MFA Azure AD FS
当前，有一些与广告 FS 使用强的身份验证的限制。  与 Windows Server 2016 中的广告 FS，以下解决，并且组织可以充分利用 usuing Azure MFA 与他们的广告 FS 实现。  组织可以充分利用如下：

-   广告 FS 场可以轻松地配置与 Azure MFA 使用多重身份验证，而不会过度配置或附加的基础结构。

-   内置 MFA 体验很容易地与 Windows Server 2016 安装

-   Azure MFA 可以配置为替代身份验证的 intranet 机制允许的主要的身份验证提供或联网。

## <a name="how-it-works"></a>它的工作原理
为了使 Azure MFA 要用作主要身份验证提供商，将就像其他提供商内置被视为的身份验证提供。  也就是说，就像表单或证书身份验证。  仅区别将提供程序将作为仅面临实现 Azure MFA 提供程序的界面外部 auth 提供商。

下图显示在 Windows Server 2016 的广告金融服务和 Azure MFA 之间数据流量。

![广告金融服务和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_1.png)

## <a name="before-you-get-started"></a>在开始之前
以下是信息的你应该尝试使用 Azure MFA 的主要身份验证提供作为之前的意识到的列表。

-   设置为主要身份验证方法 Azure MFA 以你需要有 Azure Active Directory。

-   如上文所主要身份验证方法 Azure MFA 为孩子设置使用 Azure AD 连接。  你必须使用下面的以下方法之一设置，并对其进行配置

-   此功能在中不可用混合模式。  你的整个广告 FS 场必须部分 Server 2016

## <a name="using-azure-ad-connect-to-setup-azure-mfa-as-primary-authentication-provider"></a>使用 Azure AD 连接设置为主要身份验证的提供商的 Azure MFA
你主要身份验证的提供商原样混合方案，请使用 Azure MFA，因为可以配置并设置它使用 Azure AD 连接向导。  具体取决于你是否可以只需将其设置或已经就地拥有 Azure AD 连接你仍然可以使用它来安装和配置广告 FS 使用 Azure MFA。  使用下面的步骤

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-ad-fs"></a>使用 Azure AD 连接-新安装的 Azure AD 连接和广告 FS Azure MFA 设置
如果这是全新安装，而 simplyIf 这全新安装，只需按照说明进行操作，适用于设置可以找到的 Azure AD 连接和广告 FS 联盟[此处](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/)。   当你进入到审阅选项屏幕时，确保广告 fs 配置 Azure MFA 选中复选框，然后单击确认。   将自动 automatically.When 到审阅选项屏幕时，请确保**广告 fs 配置 Azure MFA**进行检查并单击**确认**。   将自动设置为主要身份验证提供商的 azure MFA。

![广告金融服务和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_2.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-existing-ad-fs"></a>使用 Azure AD 连接-新安装的 Azure AD 连接和现有广告 FS Azure MFA 设置
如果你已安装广告 FS，并且现在转安装 Azure AD 连接，你需要只需按照说明进行操作，适用于设置可以找到的 Azure AD 连接和广告 FS 联盟[此处](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/)并确保选择现有广告 FS 服务器场。   当你进入到审阅选项屏幕时，请确保**广告 fs 配置 Azure MFA**进行检查并单击**确认。** 将自动设置为主要身份验证提供商的 azure MFA。  .

![广告金融服务和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_3.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---existing-azure-ad-connect-and-existing-ad-fs"></a>使用 Azure AD 连接-现有 Azure AD 连接和现有广告 FS Azure MFA 设置
如果你有安装 Azure AD 连接或广告 FS，然后再次和其他任务，只需可以运行 Azure AD 连接向导选择**我广告 FS 场配置 Azure MFA**并按照，然后运行该向导通过完成。

![广告金融服务和 MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_4.png)

## <a name="setting-up-azure-mfa-with-server-manager-in-windows-server-2016"></a>Azure MFA 与 Windows Server 2016 服务器经理设置


