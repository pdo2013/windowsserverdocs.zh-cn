---
title: 自定义要与 AD FS 2016 使用 OpenID Connect 或 OAuth 时在 id_token 中发出的声明
description: 自定义 id 令牌 conecpts AD FS 2016 中的技术概述
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 8c8e14f22a4bca5b6d32e841814a58a4b4ddca01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820638"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016"></a>自定义要与 AD FS 2016 使用 OpenID Connect 或 OAuth 时在 id_token 中发出的声明

## <a name="overview"></a>概述
文章[此处](enabling-openId-connect-with-ad-fs.md)演示了如何生成使用的 OpenID Connect 登录 AD FS 的应用程序。 但是，默认情况下有仅一组固定的声明在 id_token 中提供。 AD FS 2016 具有自定义 id_token OpenID Connect 方案中的功能。

## <a name="when-are-custom-id-token-used"></a>当为自定义 ID 使用令牌？
在某些情况下，很可能客户端应用程序不具有它正尝试访问的资源。 因此，它实际上并不需要访问令牌。 在这种情况下，客户端应用程序实质上是需要仅 ID 令牌，但具有某些其他声明，以帮助的功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>获取令牌 ID 的自定义声明的限制有哪些？

### <a name="scenario-1"></a>方案 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 设置为 form_post
2.  只有公共客户端可以自定义声明中获取 ID 令牌
3.  信赖方标识符应为相同的客户端标识符

### <a name="scenario-2"></a>方案 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

与[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) AD FS 服务器上安装
1.  response_mode 设置为 form_post
2.  将作用域 allatclaims 分配给客户端 – RP 对。
可以通过使用 Grant ADFSApplicationPermission cmdlet，如下面的示例所示分配作用域：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-an-oauth-application-to-handle-custom-claims-in-id-token"></a>创建 OAuth 应用程序来处理在 ID 令牌中的自定义声明
请参阅文章[此处](Enabling-OpenId-Connect-with-AD-FS-2016.md)若要创建应用程序使用的应用程序的 OpenID Connect 的 AD FS 登录。 然后按照以下步骤来配置 AD FS 中的应用程序，用于接收 ID 令牌与自定义声明。

### <a name="create-the-application-group-in-ad-fs-2016"></a>在 AD FS 2016 中创建应用程序组

1.  创建名为 CustomTokenClient 基于新模板，如下所示，应用程序组。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

2. 此模板创建机密客户端。 请将标识并指定返回的 URI 为 VS 项目 SSL URL。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

3.  在下一步中，选择**生成一个共享的机密**创建客户端凭据并将复制生成的客户端凭据。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

4. 单击**下一步**继续完成向导。

![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

### <a name="create-the-relying-party"></a>创建信赖方
为了添加自定义声明在 ID 令牌，需要创建 RP 的声明将添加在 ID 令牌中。 使用添加信赖方信任向导创建新的信赖方，如下所示：
 
![信赖方](media/Custom-Id-Tokens-in-AD-FS/rpsnap1.png)

创建信赖方后，右键单击信赖方条目，并选择**编辑声明颁发策略**添加声明颁发规则。 添加所需的自定义声明的 ID 令牌，如下所示：

![信赖方](media/Custom-Id-Tokens-in-AD-FS/rpsnap2.png)

### <a name="assign-allatclaims-scope-to-the-pair-of-client-and-relying-party"></a>将"allatclaims"作用域分配给对客户端和信赖方
在 AD FS 服务器上使用 PowerShell 将分配新 allatclaims 作用域值在下面的示例中 (更改的 clientID 和服务器：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "5db77ce4-cedf-4319-85f7-cc230b7022e0" -ServerRoleIdentifier "https://customidrp1/" -ScopeNames "allatclaims","openid"
```

>[!NOTE]
>按照你的应用程序设置更改 ClientRoleIdentifier 和 ServerRoleIdentifier

## <a name="test-the-custom-claims-in-id-token"></a>在 ID 令牌中测试自定义声明

然后，可以使用同一位始终具有用于访问声明的代码，了解将成为在 id_token 的一部分的其他声明。
例如，在.NET MVC 示例应用中，打开其中一个控制器文件，并输入类似的代码如下：


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
>请注意，Oauth2 请求中需要的资源参数。
>
>错误示例：
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token 和作用域 = openid 和 redirect_uri = https&#58;//myportal/auth 和 nonce = b3ceb943fc756d927777 client_id = 6db3ec2a-075a-4 c 72 9b22 ca7ab60cb4e7& 状态 = 42c2c156aef47e8d0870 资源 = 6db3ec2a-075a-4 c 72 9b22 ca7ab60cb4e7**
>
>很好的示例：
>
>**https&#58;//sts.contoso.com/adfs/oauth2/authorize?response_type=id_token 和作用域 = openid 和 redirect_uri = https&#58;//myportal/auth 和 nonce = b3ceb943fc756d927777 client_id = 6db3ec2a-075a-4 c 72 9b22 ca7ab60cb4e7& 状态 = 42c2c156aef47e8d0870 resource = https&#58;//customidrp1/ & response_mode = form_post**
>
>如果资源参数不在请求中可能会收到错误或没有任何自定义声明的令牌。

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  
