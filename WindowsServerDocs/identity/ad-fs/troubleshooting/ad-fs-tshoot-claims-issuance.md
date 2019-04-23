---
title: AD FS 故障排除-声明颁发
description: 本文档介绍如何使用 AD FS 令牌颁发问题进行疑难解答
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839658"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>AD FS 故障排除-声明颁发
声明是一个语句，一个使用者使其自身或另一个主题。  在信赖方颁发的声明，它们是给定的一个或多个值，然后由 AD FS 服务器颁发的安全令牌中打包。  此过程中有多个移动部件，因为声明颁发可以分解为这些关键部分。

>[!NOTE]  
>可以使用[ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest)上[ADFS 帮助](https://adfshelp.microsoft.com)站点以帮助解决声明的问题。   

## <a name="token-request"></a>令牌请求
当你转到信赖方时它将重定向到 AD FS 与令牌请求。  与请求可能出现的问题。  最值得注意的是：

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>请求与第三方 (尤其是 SAML) 格式设置

### <a name="pre-formated-urls-that-have-typos"></a>Pre 格式有拼写错误的 Url
时从 WS Federaion 信赖方颁发令牌的令牌请求附带了 URL 查询字符串参数。  如果信赖方不会在该 URL 中指定正确的参数，会重定向到 AD FS 则可能会出现问题导致的请求。


为了验证令牌的格式，可以使用 web 调试器工具


## <a name="token-response"></a>令牌响应

## <a name="authentication"></a>身份验证

## <a name="claim-rule-processing"></a>声明规则处理