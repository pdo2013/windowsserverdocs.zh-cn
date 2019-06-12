---
title: Azure AD 用户体验中的 AD FS 的 Web 主题
description: 以下文档介绍了如何更改 AD FS 窗体登录，使其类似于 Azure AD 用户体验。
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1064084fe357e54d7230f58e486aa4e62958f6ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445009"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>在 Active Directory 联合身份验证服务中使用 Azure AD 用户体验 Web 主题
AD FS 窗体登录中当前不会映射 Azure/O365 登录体验。  若要向最终用户提供更统一和无缝的体验，我们发布了以下级联样式表 web 主题可应用于 AD FS 服务器。  目前，窗体登录适用于 AD FS 上 Windows Server 2016 如下所示：

![当前登录](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


使用新的样式表，用户体验看起来更像 Azure 和 Office 365 登录体验。

## <a name="download-the-css-style-sheet"></a>下载的 CSS 样式表
您可以从以下 Github 下载 web 主题[位置](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)。


## <a name="enabling-the-new-web-theme"></a>启用新的 web 主题
若要启用新的 web 主题，请使用以下过程：

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>若要启用 AD FS 中新的 Azure AD 用户体验 web 主题
1. 以管理员身份启动 PowerShell
2. 创建新的 web 主题使用 PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. 设置为使用 PowerShell 的活动主题的新主题：`Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. 测试单一登录，通过转到 https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm![单一登录](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ![注意]您需要确保已启用该 idpinitiatedsignon。  默认情况下未启用它。  若要启用 idpinitiatedsignon，请使用以下 PowerShell 命令：  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>图像的建议
启用居中的 UI，可使用相同的图像的背景和可能已为 Azure Active Directory 公司品牌徽标。 通常情况下，并应用相同的建议的大小、 比率和格式。

### <a name="logo"></a>徽标

描述 | 约束 | 建议
------- | ------- | ----------
徽标显示在登录面板顶部。 | 透明 JPG 或 PNG<br>最大高度：36 px<br>最大宽度：245 px | 此处使用组织的徽标。<br>使用透明图像。 不要假设背景为白色。<br>不要在映像中添加徽标周围的填充量或你的徽标将看起来很小。

### <a name="background"></a>后台

描述 | 约束 | 建议
------- | ------- | ----------
此选项显示在登录页上，在后台是锚定到可视空间和缩放和裁剪来适应浏览器窗口的中心。    <br>如移动电话的屏幕较窄，不显示此映像。<br>页面加载时，将对此图像应用 0.55 不透明的黑色蒙板。 | JPG 或 PNG<br>图像尺寸：1920 x 1080 像素<br>文件大小：&lt; 300 KB | <br>使用映像在没有鲜明主题。 不透明登录窗体出现在此图像的中心之上，可以覆盖图像，具体取决于浏览器窗口的大小的任何部分。<br>保持文件大小较小，以确保快速加载。

## <a name="next-steps"></a>后续步骤
- [Windows Server 2016 中的 AD FS 自定义](AD-FS-Customization-in-Windows-Server-2016.md)
- [高级自定义](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [自定义 web 主题](Custom-Web-Themes-in-AD-FS.md)
