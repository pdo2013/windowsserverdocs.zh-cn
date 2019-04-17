---
title: "自定义索赔发出 id_token 中，当使用广告 FS 2016 OpenID 连接或 OAuth"
description: "广告 FS 2016 中自定义 id 令牌 conecpts 技术概述"
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c17d50bb6d90726eafc3e585f16a7a8189a68392
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>自定义索赔发出 id_token 中，当使用广告 FS 2016 OpenID 连接或 OAuth

## <a name="overview"></a>概述
文章[此处](enabling-openId-connect-with-ad-fs.md)演示如何生成的 OpenID 连接登录使用广告 FS 的应用。 但是，默认情况下有仅适用于 id_token 索赔解决的设置。 广告 FS 2016 具有自定义在 OpenID 连接方案中的 id_token 的功能。

## <a name="when-are-custom-id-token-used"></a>当是自定义 ID 令牌使用？
在某些情况下，则可能客户端应用程序不具有尝试访问的资源。 因此，它不会急需访问标记。 在这种情况下的客户端应用程序本质上是需要仅 ID 令牌但这些一些其他索赔，若要帮助的功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>获取自定义的索赔 ID 中标记的限制有哪些？

### <a name="scenario-1"></a>方案 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  设置为 form_post response_mode
2.  仅公共客户端可以自定义声明中 ID 标记
3.  应相同的客户端标识符信赖方标识符

### <a name="scenario-2"></a>方案 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

与[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472)广告 FS 服务器上安装
1.  设置为 form_post response_mode
2.  范围 allatclaims 给客户端 – RP 配对。
你可以在以下示例所述使用 Grant-ADFSApplicationPermission cmdlet 分配范围：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>创建 OAuth 应用程序以处理 ID 令牌中自定义的声明
使用文章[此处](Enabling-OpenId-Connect-with-AD-FS-2016.md)创建应用程序使用的应用的 OpenID 连接广告 FS 登录。 然后按照以下步骤来接收使用自定义的索赔 ID 令牌用于在广告 FS 配置应用程序。

### <a name="create-the-application-group-in-ad-fs-2016"></a>创建 AD FS 2016 中的应用程序组

1.  创建称为 CustomTokenClient 根据新模板，如下所示，应用程序组。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. 此模板创建一个机密的客户端。 记下标识符，并与项目 SSL URL 指定退货 URI。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  在下一步中，选择**生成共享的机密**创建凭据的客户端并将生成的客户端凭据复制。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. 单击**下一步**，然后继续完成向导。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>创建依赖方
为了 ID 中添加自定义的索赔令牌，你需要创建其索赔将被添加到 ID 令牌 RP。 添加方信任依赖向导用于创建新信赖方如下所示：
 
![依赖方](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

创建依赖方后，右键单击信赖方条目，然后选择**编辑声称颁发策略**添加索赔颁发规则。 添加所需的自定义 ID 令牌索赔，如下所示：

![依赖方](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>将"allatclaims"范围分配的客户端和依赖方配对
使用广告 FS 服务器 PowerShell 分配的下面的示例中指定的新 allatclaims 范围 (更改客户机 Id 和 server:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>更改 ClientRoleIdentifier 和 ServerRoleIdentifier 根据你的应用程序设置

## <a name="test-the-custom-claims-in-id-token"></a>测试 ID 令牌自定义的声明

然后，使用同一位的代码，你始终用于访问索赔，你可以查看将成为 id_token 的一部分的其他索赔。
例如，在.NET MVC 示例应用中，打开控制器文件之一，输入代码等下方：


``` code
    [Authorize]
    public ActionResult About()
    {

        ClaimsPrincipal cp = ClaimsPrincipal.Current;

        string userName = cp.FindFirst(ClaimTypes.GivenName).Value;
        ViewBag.Message = String.Format("Hello {0}!", userName);
        return View();

    }

```

>[!NOTE]
>请注意，才能资源参数 Oauth2 请求。
>
>错误示例：
>
>**Https & #58; 月 / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7**
>
>很好的示例：
>
>**Https & #58; 月 / sts.contoso.com/adfs/oauth2/authorize?response_type=id_token&scope=openid&redirect_uri=https&#58;//myportal/auth&nonce=b3ceb943fc756d927777&client_id=6db3ec2a-075a-4c72-9b22-ca7ab60cb4e7&state=42c2c156aef47e8d0870&resource=https&#58;//customidrp1/**
>
>如果资源参数不是你可能会收到错误或没有任何自定义的索赔令牌该请求。

## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  