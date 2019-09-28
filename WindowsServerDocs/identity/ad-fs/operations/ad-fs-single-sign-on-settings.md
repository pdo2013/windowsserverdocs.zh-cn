---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: AD FS 2016 单一登录设置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 311789fdec160faeeeba0ecf26491d1e0cd6105d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407399"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 单一登录设置

单一登录（SSO）允许用户通过一次身份验证和访问多个资源，而不会提示你提供其他凭据。  本文介绍了适用于 SSO 的默认 AD FS 行为以及允许您自定义此行为的配置设置。  

## <a name="supported-types-of-single-sign-on"></a>支持的单一登录类型

AD FS 支持多种类型的单一登录体验：  
  
-   **会话 SSO**  
  
     会话 SSO cookie 是为经过身份验证的用户编写的，当用户在特定会话中切换应用程序时，此 cookie 将消除进一步的提示。 不过，如果某个会话结束，则会再次提示用户输入其凭据。  
  
     如果未注册用户的设备，AD FS 将默认设置会话 SSO cookie。 如果浏览器会话已结束并重新启动，则此会话 cookie 会被删除，并且不再有效。  
  
-   **永久性 SSO**  
  
     永久性 SSO cookie 是为经过身份验证的用户编写的，在用户切换应用程序时，只要该永久性 SSO cookie 有效，就可以消除进一步的提示。 永久性 SSO 与会话 SSO 之间的区别在于可以在不同的会话之间维护持久性 SSO。  
  
     如果设备已注册，AD FS 将设置持久性 SSO cookie。 如果用户选择 "使我保持登录" 选项，AD FS 还将设置持久性 SSO cookie。 如果永久性 SSO cookie 无效，则会被拒绝并删除。  
  
-   **特定于应用程序的 SSO**  
  
     在 OAuth 情况下，刷新令牌用于维护特定应用程序范围内用户的 SSO 状态。  
  
     如果注册了某个设备，则 AD FS 将基于已注册设备的永久 SSO cookie 生存期设置刷新令牌的过期时间，默认情况下，对于 AD FS 2012R2 为7天 2016 AD FS 90，如果使用其设备访问14天窗口中 AD FS 的资源。 

如果设备未注册，但用户选择 "使我保持登录状态" 选项，则刷新令牌的过期时间将等于 "使我保持登录状态" 的持久 SSO cookie 生存期，默认值为1天，最大值为7天。 否则，刷新令牌生存期等于会话 SSO cookie 生存期，默认情况下为8小时  
  
 如上所述，已注册设备上的用户将始终获取永久性 SSO，除非禁用永久性 SSO。 对于未注册的设备，可通过启用 "使我保持登录" （KMSI）功能来实现持久的 SSO。 
 
 对于 Windows Server 2012 R2，若要为 "使我保持登录" 方案启用 PSSO，需要安装此[修补程序](https://support.microsoft.com/en-us/kb/2958298/)，它也是[windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2 的8月2014更新汇总](https://support.microsoft.com/en-us/kb/2975719)的一部分。   

任务 | PowerShell | 描述
------------ | ------------- | -------------
启用/禁用永久性 SSO | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| 默认情况下启用持久性 SSO。 如果已禁用，则不会写入任何 PSSO cookie。
"启用/禁用" 使我保持登录 " | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | 默认情况下禁用 "使我保持登录状态" 功能。 如果已启用，最终用户将在 AD FS 登录页上看到 "使我保持登录状态" 选项



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-单一登录和经过身份验证的设备
AD FS 2016 会在从已注册设备进行身份验证时，将更改 PSSO 更改为最多90天，但在14天内要求身份验证（设备使用时段）。
首次提供凭据后，默认情况下，具有已注册设备的用户将在90天的最长期限内进入单一登录状态，前提是它们使用设备每14天至少访问一次 AD FS 资源。  如果在提供凭据之后等待15天，将再次提示用户输入凭据。  

默认情况下启用持久性 SSO。 如果已禁用，则不会写入任何 PSSO cookie。 |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
"设备使用情况" 窗口（默认为14天）由 AD FS 属性**DeviceUsageWindowInDays**控制。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
最大单一登录时间段（默认为90天）受 AD FS 属性**PersistentSsoLifetimeMins**的控制。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>为未经身份验证的设备使我保持登录 
对于非注册设备，单一登录期间由 "**使我保持登录（KMSI）** " 功能设置来确定。  默认情况下，KMSI 处于禁用状态，可以通过将 AD FS 属性 KmsiEnabled 设置为 True 来启用。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

禁用 KMSI 时，默认的单一登录时间为8小时。  这可以使用属性 SsoLifetime 进行配置。  以分钟为单位度量属性，因此，其默认值为480。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

如果启用了 KMSI，则默认的单一登录时间为24小时。  这可以使用属性 KmsiLifetimeMins 进行配置。  以分钟为单位度量属性，因此，其默认值为1440。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多重身份验证（MFA）行为  
请务必注意，尽管提供相对较长的单一登录时间段，但当上一次登录时，AD FS 会提示进行附加身份验证（多重身份验证）。需要 MFA。  这与 SSO 配置无关。 AD FS，当接收到身份验证请求时，首先确定是否有 SSO 上下文（如 cookie），然后，如果需要 MFA （例如，如果请求来自外部），它将评估 SSO 上下文是否包含 MFA。  否则，系统会提示 MFA。  


  
## <a name="psso-revocation"></a>PSSO 吊销  
 为了保护安全性，在满足以下条件时，AD FS 将拒绝以前颁发的任何永久性 SSO cookie。 这将要求用户提供其凭据，以便再次向 AD FS 进行身份验证。 
  
- 用户更改密码  
  
- 已禁用永久性 SSO 设置 AD FS  
  
- 设备在丢失或被盗的情况下被管理员禁用  
  
- AD FS 收到一个永久性 SSO cookie，此 cookie 将颁发给注册的用户，但用户或设备不再注册  
  
- AD FS 收到已注册用户的永久性 SSO cookie，但用户已重新注册  
  
- AD FS 收到永久性 SSO cookie，此 cookie 作为 "使我保持登录状态"，但在 AD FS 中禁用 "使我保持登录状态" 设置。  
  
- AD FS 收到为注册用户颁发的永久性 SSO cookie，但在身份验证过程中缺少或更改设备证书  
  
- AD FS 管理员为永久性 SSO 设置了截止时间。 配置此配置后，AD FS 将拒绝在此时间之前颁发的任何永久性 SSO cookie  
  
  若要设置截止时间，请运行以下 PowerShell cmdlet：  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>为 Office 365 用户启用 PSSO 以访问 SharePoint Online  
 在 AD FS 中启用并配置 PSSO 后，AD FS 将在用户进行身份验证后写入永久性 cookie。 用户下次进入时，如果持久性 cookie 仍然有效，则用户无需提供凭据即可再次进行身份验证。 还可以通过在 AD FS 中配置以下两个声明规则，以便在 Microsoft Azure AD 和 SharePoint Online 上触发持久性，来避免 Office 365 和 SharePoint Online 用户的其他身份验证提示。  若要启用 PSSO for Office 365 用户访问 SharePoint online，需要安装此[修补程序](https://support.microsoft.com/en-us/kb/2958298/)，它也是[windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2 的8月2014更新汇总](https://support.microsoft.com/en-us/kb/2975719)的一部分。  
  
 要传递 InsideCorporateNetwork 声明的颁发转换规则  
  
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
  
汇总：
<table>
  <tr>
    <th colspan="1">单一登录体验</th>
    <th colspan="3">ADFS 2012 R2 <br> 设备是否已注册？</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> 设备是否已注册？</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>否</th>
    <th>否，但 KMSI</th>
    <th>是</th>
    <th></th>
    <th>否</th>
    <th>否，但 KMSI</th>
    <th>是</th>
  </tr>
 <tr align="center">
    <td>SSO =&gt;设置刷新令牌 =&gt;</td>
    <td>8小时</td>
    <td>不可用</td>
    <td>不可用</td>
    <th></th>
    <td>8小时</td>
    <td>不可用</td>
    <td>不可用</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;设置刷新令牌 =&gt;</td>
    <td>不可用</td>
    <td>24小时</td>
    <td>7天</td>
    <th></th>
    <td>不可用</td>
    <td>24小时</td>
    <td>最长90天，最长14天</td>
  </tr>

 <tr align="center">
    <td>令牌生存期</td>
    <td>1小时</td>
    <td>1小时</td>
    <td>1小时</td>
    <th></th>
    <td>1小时</td>
    <td>1小时</td>
    <td>1小时</td>
  </tr>
</table>

**已注册设备？** 获取 PSSO/持久性 SSO <br>
**未注册设备？** 获取 SSO <br>
**未注册的设备，但 KMSI？** 获取 PSSO/持久性 SSO <p>
如果
 - [x] 管理员已启用 KMSI 功能 [和]
 - [x] 用户单击窗体登录页上的 "KMSI" 复选框
 
**好了解：** <br>
未同步**LastPasswordChangeTimestamp**属性的联合用户将发出会话 cookie，并且**最大期限值为12小时的**刷新令牌。<br>
之所以发生这种情况，是因为 Azure AD 无法确定何时吊销与旧凭据（例如已更改的密码）相关的令牌。 因此，Azure AD 必须更频繁地进行检查，以确保用户和关联的令牌仍处于良好地位。
