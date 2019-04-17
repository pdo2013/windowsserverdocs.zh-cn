---
title: "Azure AD 在广告 FS UX Web 主题"
description: "下面的文档介绍了如何更改广告 FS 表单登录以使其类似于 Azure AD 用户体验。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7bac1db17eb4facc7643fe0db0ccf00c119a45d
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2018
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>使用 Active Directory 联合身份验证服务在 Azure AD UX Web 主题
广告 FS 表单登录当前不镜像 Azure/O365 登录体验。  若要为最终用户提供更加统一和无缝体验，我们发布了按照层叠样式表 web 主题这可用于广告 FS 服务器。  当前，表单登录 Windows Server 2016 如下所示以下上广告 fs:

![当前登录](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


与新的样式表的用户体验将看上去更像 Azure 和 Office 365 登录体验。

## <a name="download-the-css-style-sheet"></a>下载 CSS 样式表
你可以从以下 Github 下载 web 主题[位置](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)。


## <a name="enabling-the-new-web-theme"></a>启用新的 web 主题
若要启用的新 web 主题，请使用以下步骤：

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>若要启用广告 FS 中新的 Azure AD UX web 主题
1.  开始 PowerShell 以管理员身份登录
2.  创建新的 web 主题使用 PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  设置为使用 PowerShell 活动主题的新主题：<ph x="1">组 AdfsWebConfig ActiveThemeName 自定义
![</ph>PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  通过转到 https:// 测试登录<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm![登录](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

>![注意]你需要确保该 idpinitiatedsignon 已启用。  它不是默认启用。  若要启用 idpinitiatedsignon，请使用以下 PowerShell 命令：  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>图像建议
以下是背景图像和徽标图像的大小建议：

### <a name="logo"></a>徽标
- 调整 24px 高度，256px 最大的宽度
- 不要添加内资产徽标周围的任何空白。  请确保透明资产背景。

### <a name="background"></a>背景
- 文件大小不应 200 KB 超过大小 1024 x 1080 像素。  你应使用的最高的分辨率可能与纵横比 16:9 月 16:10 保持限制条件下的图像大小。

## <a name="next-steps"></a>后续步骤
- [在 Windows Server 2016 的广告 FS 自定义](AD-FS-Customization-in-Windows-Server-2016.md)
- [高级自定义](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [自定义 web 主题](Custom-Web-Themes-in-AD-FS.md)
