---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: "广告 FS 2016 单一登录设置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>广告 FS 单一登录设置

>适用于：Windows Server 2016，Windows Server 2012 R2

单一登录 (SSO) 可用于验证一次，并且不会提示输入其他凭据的情况下访问多个资源。  本文介绍了 SSO，允许你自定义此行为配置设置为默认 FS 广告的行为。  

## <a name="supported-types-of-single-sign-on"></a>支持的类型的单一登录

广告 FS 支持几种类型的单一登录体验：  
  
-   **会话 SSO**  
  
     会话 SSO cookie 是面向消除了更多提示，用户特定会话期间切换应用程序时身份验证的用户。 但是，如果特定会话结束时，用户将提示您输入凭据再次。  
  
     广告 FS 将会话 SSO cookie 默认设置如果尚未注册用户的设备。 如果在浏览会话结束后，重启此会话 cookie 将删除，并不任何更有效。  
  
-   **永久性 SSO**  
  
     为进一步消除提示，用户切换应用程序的只要永久性 SSO cookie 的有效时身份验证的用户写入持久 SSO cookie。 永久性 SSO 和会话 SSO 之间的区别是永久性 SSO 可以会保留，跨不同的会话。  
  
     如果已注册设备，广告 FS 将设置永久性 SSO cookie。 广告 FS 用户选中"保留我登录"选项，还将设置永久性 SSO cookie。 如果永久性 SSO cookie 更加不正确，它将拒绝，删除。  
  
-   **应用程序特定 SSO**  
  
     在 OAuth 方案中，刷新令牌用于维护 SSO 状态特定应用程序的范围内的用户。  
  
     如果已注册设备，广告 FS 将设置基于永久性 SSO cookie 整个使用期限内已注册设备，这是 7 天，默认情况下一个刷新标记的过期时间。 刷新标记的过期时间用户选中"保留我登录"选项，如果等于永久性 SSO cookie 整个使用期限内的"保留我登录"这是默认情况下与最多个 7 天 1 天。 否则，刷新令牌生存期等会话 SSO cookie 的生命周期是默认的 8 小时  
  
 如上所述，已注册设备上的用户将始终获得永久性 SSO 除非永久性 SSO 被禁用。 对于未注册的设备，可以通过启用"保留我登录"实现永久性 SSO (KMSI) 功能。 
 
 对于 Windows Server 2012 R2 启用 PSSO"保留我登录"方案中，你需要安装该版本[修补程序](https://support.microsoft.com/en-us/kb/2958298/)这也是的一部分的[2014 年 8 月更新汇总用于 Windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719)。   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>单一登录并验证的设备  
在首次让凭据，默认情况下使用已注册设备的用户获取单一登录 90 天内，最长前提是他们使用设备来访问至少运行一次每个 14 天内的广告 FS 资源。  如果他们等待 15 天后提供的凭据，用户将会提示您输入凭据再次。  

默认情况下，启用永久性 SSO。 如果它已禁用，将写入没有 PSSO cookie。|  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
设备使用情况窗口（默认情况下 14 天后）由广告 FS 属性**DeviceUsageWindowInDays**。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
最大单一登录时间（默认情况下 90 天内）由广告 FS 属性**PersistentSsoLifetimeMins**。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>保留我的身份验证的设备上登录 
对于非注册的设备单个登录句点由**保持我记录中 (KMSI)**功能设置。  KMSI 默认处于禁用状态，并且可通过广告 FS 属性 KmsiEnabled 设置为 True。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

禁用 KMSI，使用默认单一登录周期是 8 小时。  可使用属性 SsoLifetime 配置此。  因此其默认值 480 几分钟内测量属性。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

启用 KMSI，使用默认单一登录时段为 24 小时。  可使用属性 KmsiLifetimeMins 配置此。  使其默认值为 1440 年几分钟内测量属性。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多重身份验证 (MFA) 行为  
请务必注意，尽管提供较长一段单一登录、广告 FS 将提示进行额外的身份验证（多因素身份验证）以前登录基于主要凭据和不 MFA，但当前登录时要求 MFA。  这是无论 SSO 配置。 广告 FS，当接收身份验证请求，首先确定是否有 SSO 上下文（如 cookie)，然后，是否需要 MFA (如如果请求即将推出的功能从之外) 它还将评估是否 SSO 上下文包含 MFA。  如果没有，MFA 提示。  


  
## <a name="psso-revocation"></a>PSSO 吊销  
 为了保护安全，广告 FS 将拒绝之前颁发满足以下条件时任何永久性 SSO cookie。 这将需要提供他们的凭据即可验证与广告 FS 再次用户。 
  
-   用户的更改密码  
  
-   广告 FS 中禁用永久性 SSO 设置  
  
-   设备已丢失或被盗以防万一管理员禁用  
  
-   广告 FS 接收是为其发出注册的用户但用户永久性 SSO cookie 或再未注册设备  
  
-   广告 FS 接收注册用户永久性 SSO cookie，但用户重新注册  
  
-   广告 FS 接收永久性 SSO cookie 定"保留我登录"，但"保留我登录"颁发广告 FS 中禁用设置  
  
-   广告 FS 接收颁发的注册用户永久性 SSO cookie，但设备证书身份验证过程已丢失或更改  
  
-   广告 FS 管理员已设置为永久性 SSO 的截断的时间。 当此配置时，广告 FS 将拒绝此之前颁发任何永久性 SSO cookie  
  
 若要设置截断的时间，运行以下 PowerShell cmdlet:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>启用 PSSO Office 365 用户访问 SharePoint Online  
 后 PSSO 启用，并且在广告 FS 配置，广告 FS 将写入持久的 cookie 后用户身份验证。 下一次用户就可以发挥作用，永久性 cookie 仍然有效，如果用户不必不提供再次进行身份验证的凭据。 也可以通过在广告 FS 配置以下两种索赔规则，以在 Microsoft Azure AD 和 SharePoint Online 触发器保持避免额外的身份验证的 Office 365 和 SharePoint Online 用户提示。  若要启用的 Office 365 用户联机访问 SharePoint PSSO，你需要安装该版本[修补程序](https://support.microsoft.com/en-us/kb/2958298/)这也是的一部分的[2014 年 8 月更新汇总用于 Windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719)。  
  
 若要通过 InsideCorporateNetwork 索赔颁发转换规则  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
  
    


