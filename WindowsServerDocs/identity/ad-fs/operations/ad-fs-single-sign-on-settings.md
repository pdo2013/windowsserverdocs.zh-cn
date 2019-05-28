---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: AD FS 2016 单一登录设置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3719277c80eae2bf2a4d923146920d17546601d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188728"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 单一登录设置

单一登录 (SSO) 允许用户一次身份验证并不会提示输入其他凭据访问多个资源。  本文描述了 SSO，以及可用于自定义此行为的配置设置的默认 AD FS 行为。  

## <a name="supported-types-of-single-sign-on"></a>支持的类型的单一登录

AD FS 支持几种类型的单一登录体验：  
  
-   **SSO 会话**  
  
     为经过身份验证的用户，从而消除了进一步的提示，当用户在特定会话期间切换应用程序所编写的 SSO 会话 cookie。 但是，如果特定会话结束时，用户将会再次提示输入其凭据。  
  
     AD FS 将会话的 SSO cookie 的默认设置未注册用户的设备。 如果浏览器会话已结束，并且重新启动时，此会话 cookie 被删除，且不任何更有效。  
  
-   **持续的 SSO**  
  
     持久性 SSO cookie 专为身份验证的用户而不进一步提示，当用户切换适用于应用程序，前提是持久的 SSO cookie 有效。 永久性 SSO 以及 SSO 会话之间的区别是，可在不同会话中维护持续的 SSO。  
  
     如果设备已注册，AD FS 将设置永久 SSO cookie。 如果用户选择"使我保持登录"选项，AD FS 还将设置持久性的 SSO cookie。 如果持久性的 SSO cookie 不再有效，它将拒绝并删除。  
  
-   **应用程序特定的 SSO**  
  
     在 OAuth 方案中，刷新令牌用于维护特定应用程序范围内的用户的 SSO 状态。  
  
     如果在注册设备时，AD FS 将基于为 7 天默认情况下为 AD FS 2012R2 和最大为使用 AD FS 2016 的 90 天，如果用户使用其设备的已注册设备的持久 SSO cookie 生存期的刷新令牌的过期时间访问 AD FS 的 14 天时间段内的资源。 

如果未注册设备，但用户选择"使我保持登录"选项，刷新令牌的过期时间将等于"使我保持登录"的持久性 SSO cookie 生存期这是默认情况下，最多为 7 天的 1 天。 否则，刷新令牌生存期等于会话 SSO cookie 生存期这是默认为 8 小时  
  
 如上所述，已注册的设备上的用户将始终会获得持续的 SSO，除非禁用持续的 SSO。 对于取消注册设备，可以通过启用"使我保持登录"来持续的 SSO (状态 KMSI) 功能。 
 
 对于 Windows Server 2012 R2，若要启用 PSSO"使我保持登录"方案中，需要安装此客户端[修补程序](https://support.microsoft.com/en-us/kb/2958298/)这也是属于的[2014 年 8 月 Windows RT 8.1、 Windows 8.1 和 Windows Server 2012 更新汇总R2](https://support.microsoft.com/en-us/kb/2975719)。   

任务 | PowerShell | 描述
------------ | ------------- | -------------
启用/禁用持续的 SSO | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| 默认情况下启用持续的 SSO。 如果已禁用，将不写入任何 PSSO cookie。
"启用/禁用"使我保持登录" | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | 默认情况下，"使我保持登录"功能处于禁用状态。 如果启用，最终用户将 AD FS 登录页上看到"使我保持登录"选项



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016 的单一登录和经过身份验证的设备
请求者增加到最大的 90 天，但需要在 14 天内 （设备使用情况窗口） authenticvation 已经注册的设备进行身份验证时，AD FS 2016 更改 PSSO。
提供凭据后第一次，默认情况下具有已注册的设备的用户获得单一登录最大时间段的 90 天，前提是它们使用设备来访问每 14 天至少一次的 AD FS 资源。  如果它们等待 15 天提供凭据后，用户将会再次提示输入凭据。  

默认情况下启用持续的 SSO。 如果已禁用，将写入任何 PSSO cookie。 |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
设备使用情况窗口 （默认为 14 天） 受 AD FS 属性**DeviceUsageWindowInDays**。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
最大单一登录期间 （默认为 90 天） 受 AD FS 属性**PersistentSsoLifetimeMins**。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>保留我的登录的未经身份验证的设备 
对于非注册设备，由单个登录句点**保持我签名中状态 (KMSI)** 功能设置。  KMSI 默认处于禁用状态，并且可以启用 AD FS 属性 KmsiEnabled 设置为 True。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

禁用 KMSI，与默认单一登录期限为 8 小时。  这可以配置使用 SsoLifetime 的属性。  因此其默认值为 480，以分钟为单位为单位属性。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

启用 KMSI，与默认单一登录周期为 24 小时。  这可以配置使用 KmsiLifetimeMins 的属性。  使其默认值为 1440年，以分钟为单位为单位属性。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多重身份验证 (MFA) 行为  
务必要注意，同时提供相对较长的单一登录，AD FS 将提示进行其他身份验证 （多重身份验证） 时在上一个登录已基于主要凭据和不 MFA，但当前登录需要进行 MFA。  这是而不考虑 SSO 配置。 AD FS 中，当它收到的身份验证请求，首先确定是否有 SSO 上下文 （如 cookie)，然后，如果需要进行 MFA 的 (例如，如果请求来自外部) 它将评估是否 SSO 上下文包含 MFA。  如果没有，MFA 提示。  


  
## <a name="psso-revocation"></a>PSSO 吊销  
 若要保护安全，AD FS 将拒绝以前当满足以下条件时，发出任何持久性 SSO cookie。 这将要求用户提供其凭据才能再次使用 AD FS 身份验证。 
  
-   用户更改密码  
  
-   在 AD FS 中禁用了持久性 SSO 设置  
  
-   设备已由管理员在丢失或被盗的情况下被禁用  
  
-   AD FS 接收持续的 SSO cookie 颁发的已注册的用户，但用户或设备未注册不再  
  
-   AD FS 收到的已注册用户的持久性 SSO cookie，但用户重新进行注册  
  
-   AD FS 接收因"使我保持登录"，但"使我保持登录"而发出的持久 SSO cookie 设置处于禁用状态的 AD fs  
  
-   AD FS 接收持久性的 SSO cookie，这针对已注册的用户发出的但设备证书缺失或更改身份验证过程  
  
-   AD FS 管理员已设置为持续的 SSO 的截止时间。 当此配置时，AD FS 将拒绝此时间之前发出任何持久性 SSO cookie  
  
 若要设置截止时间，请运行以下 PowerShell cmdlet:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>启用 Office 365 用户可以访问 SharePoint Online PSSO  
 一旦 PSSO 已启用并配置 AD FS 中，AD FS 将写入持久性 cookie 后用户已通过身份验证。 下次用户进入时，如果持久性 cookie 仍然有效，用户不必提供凭据再次进行身份验证。 此外可以避免额外的身份验证提示适用于 Office 365 和 SharePoint Online 用户通过配置以下两个声明规则在 AD FS 与 Microsoft Azure AD 和 SharePoint Online 触发器持久性。  若要启用 Office 365 用户访问 SharePoint online PSSO，需要安装此客户端[修补程序](https://support.microsoft.com/en-us/kb/2958298/)这也是属于的[2014 年 8 月更新汇总适用于 Windows RT 8.1、 Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 要通过 InsideCorporateNetwork 声明的颁发转换规则  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
下面汇总方式：
<table>
  <tr>
    <th colspan="1">单一登录体验</th>
    <th colspan="3">ADFS 2012 R2 <br> 注册设备？</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> 注册设备？</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>否</th>
    <th>但 KMSI 否</th>
    <th>是</th>
    <th></th>
    <th>否</th>
    <th>但 KMSI 否</th>
    <th>是</th>
  </tr>
 <tr align="center">
    <td>SSO=>set Refresh Token=></td>
    <td>8 小时</td>
    <td>不可用</td>
    <td>不可用</td>
    <th></th>
    <td>8 小时</td>
    <td>不可用</td>
    <td>不可用</td>
  </tr>

 <tr align="center">
    <td>PSSO=>set Refresh Token=></td>
    <td>不可用</td>
    <td>24 小时</td>
    <td>7 天</td>
    <th></th>
    <td>不可用</td>
    <td>24 小时</td>
    <td>最大 90 天，14 天窗口</td>
  </tr>

 <tr align="center">
    <td>令牌生存期</td>
    <td>1 小时</td>
    <td>1 小时</td>
    <td>1 小时</td>
    <th></th>
    <td>1 小时</td>
    <td>1 小时</td>
    <td>1 小时</td>
  </tr>
</table>

**已注册的设备？** 获取 PSSO / 持续的 SSO <br>
**未注册设备？** 获取一个 SSO <br>
**未注册设备，但 KMSI？** 获取 PSSO / 持续的 SSO <p>
如果：
 - [x] 管理员已启用 KMSI 功能 [AND]
 - [x] 用户单击窗体登录页上的 KMSI 复选框
 
**最好能了解：** <br>
联合用户不具有**LastPasswordChangeTimestamp**属性同步会话 cookie 和刷新令牌具有颁发**12 个小时的最大期限值**。<br>
这是因为 Azure AD 无法确定何时吊销旧凭据 （如已更改密码） 相关的令牌。 因此，Azure AD 必须更频繁地检查以确保用户和关联的令牌是仍在处于正常状态。
