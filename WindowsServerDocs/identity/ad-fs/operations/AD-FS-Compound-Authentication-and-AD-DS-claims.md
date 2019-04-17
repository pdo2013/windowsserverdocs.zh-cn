---
title: "在 Active Directory 联合身份验证服务复合身份验证和 Active Directory 域服务索赔"
description: "下面的文档讨论复合身份验证和广告 FS 中的广告 DS 索赔。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>身份验证和广告 FS 中的广告 DS 索赔复合
Windows Server 2012 引入复合身份验证，从而增强了 Kerberos 身份验证。  复合身份验证，包括两种身份一个 Kerberos 票证许可服务上 (TGS) 请求： 

- 用户的身份
- 用户的设备的身份。  

Windows 实现了复合身份验证，通过扩展[Kerberos 灵活身份验证安全隧道 (FAST)，或 Kerberos 程度](https://technet.microsoft.com/library/hh831747.aspx)。 

广告金融服务 2012 年和更高版本允许消耗位于 Kerberos 身份验证票证的广告 DS 颁发用户或设备索赔。 在以前版本的广告 FS，的索赔引擎可用于从 Kerberos 仅阅读用户和组安全 Id (Sid)，但无法阅读任何索赔 Kerberos 中包含的信息票证。

你可以通过使用 Active Directory 域服务 (广告 DS) 启用了更丰富的联合应用访问控制-颁发的用户和设备索赔在一起使用 Active Directory 联合身份验证服务 (AD FS)。

## <a name="requirements"></a>要求
1.  访问联盟应用程序，这两台计算机必须为广告 FS 使用进行身份验证**Windows 的集成身份验证**。 
    - 连接到的后端广告 FS 服务器时，Windows 的集成身份验证才可用。
    - 计算机必须无法来触及距离较联合身份验证服务名称的后端广告 FS 服务器
    - 广告 FS 服务器必须其 Intranet 设置中的主身份验证方法为提供 Windows 的集成身份验证。

2.  该策略**Kerberos 客户端支持声明复合身份验证和 Kerberos 程度**必须适用于访问联合的应用的所有计算机程序受复合身份验证。 这是一个森林情形或跨森林方案适用。

3.  必须具有住房广告 FS 服务器的域**KDC 支持索赔复合身份验证和 Kerberos 程度**策略设置应用到域控制器。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 配置广告 FS 步骤
使用以下步骤配置复合身份验证和索赔 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>第 1 步：启用 KDC 索赔，复合身份验证、Kerberos 程度上默认域控制器策略的支持
1.  在服务器管理器中，选择工具**组策略管理**。
2.  向下导航**默认域控制器策略**、右键单击，然后选择**编辑**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  在**组策略编辑器中管理**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**KDC**。
4.  右侧窗格中，双击**KDC 支持索赔，复合身份验证、Kerberos 程度**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在新的对话框窗口中，将设置为索赔 KDC 支持**启用**。
6.  在选项选择**支持**从下拉菜单中，然后单击**应用**和**确定**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>第 2 步：启用 Kerberos 客户端支持索赔、复合身份验证、和 Kerberos 程度访问联盟应用程序的计算机上

1.  组策略应用到访问联盟应用程序中的计算机上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**Kerberos**。
2.  右窗格的组策略编辑器中管理窗口中，双击**Kerberos 客户端支持索赔、复合身份验证、和 Kerberos 程度。**
3.  在新的对话框窗口中，设置为 Kerberos 客户端支持**启用**单击**应用**和**确定**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  关闭组策略管理编辑器。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步骤 3：确保已更新广告 FS 服务器。
你需要确保下列更新已安装在你的广告 FS 服务器上。

|更新|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|（包括 KB2919355 KB2932046、KB2934018、KB2937592、KB2938439）的累积安全更新|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Server 2012 R2 的更新|
|[修复程序 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|此更新中的 Active Directory 联合身份验证服务添加复合 ID 索赔的支持。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>第 4 步：配置的主要身份验证提供

1. 设置为主要身份验证提供**Windows 身份验证**广告 FS Intranet 设置。
2. 在广告 FS 管理下**认证策略**，选择**主要身份验证**下**全球设置**单击**编辑**。
3. 在**编辑策略全球的身份验证**下**Intranet**选择**Windows 身份验证**。
4. 单击**应用**和**确定**。

![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用你可以使用 PowerShell**组 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在基于 WID 电场的日落，必须执行 PowerShell 命令主广告 FS 服务器上。
>在基于 SQL 电场的日落，可能是农场里的成员任何广告 FS 服务器上执行 PowerShell 命令。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>第 5 步：添加到广告 FS 索赔描述
1.  添加到场以下索赔描述。 此声明描述在不是默认 ADFS 2012 R2，需要手动添加。
2.  广告 FS 管理，在下**服务**，右键单击**声称描述**选择**添加声称描述**
3.  输入以下信息中的索赔说明
    - 显示名称: Windows 设备组 
    - 索赔说明: https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup'
4. 在这两个框打勾。
5. 单击**确定**。

![声称描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用你可以使用 PowerShell**添加 AdfsClaimDescription** cmdlet。
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>在基于 WID 电场的日落，必须执行 PowerShell 命令主广告 FS 服务器上。
>在基于 SQL 电场的日落，可能是农场里的成员任何广告 FS 服务器上执行 PowerShell 命令。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>第 6 步：启用 msDS SupportedEncryptionTypes 属性上的复合身份验证位

1.  启用复合身份验证 msDS SupportedEncryptionTypes 特性命名运行广告 FS 服务使用的帐户上位**组 ADServiceAccount** PowerShell cmdlet。  

>[!NOTE]
>如果你更改服务帐户，然后通过运行必须手动启用复合身份验证**组 ADUser compoundIdentitySupported: $true** Windows PowerShell cmdlet.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  重新启动 ADFS 服务。

>[!NOTE]
>一旦 'CompoundIdentitySupported 设置为相同的新服务器 (2012R2/2016) 失败，错误如下–gMSA 如此，安装**安装 ADServiceAccount：无法安装服务的帐户。错误消息: 提供的上下文不匹配目标。**.
>
>**解决方案**：暂时 $false 设置 CompoundIdentitySupported。 此步骤导致 ADFS 以停止发出 WindowsDeviceGroup 索赔。 Set-ADServiceAccount-身份 ADFS 服务帐户-CompoundIdentitySupported: $false 安装在新的服务器上 gMSA，然后返回到 $True 启用 CompoundIdentitySupported。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>第 7 步：为 Active Directory 信任广告 FS 索赔提供商的更新

1. 更新广告 FS 索赔提供商信任 Active Directory 的 WindowsDeviceGroup' 索赔包含以下 '直通索赔规则。
2.  在**广告 FS 管理**，单击**索赔提供商信任**右窗格中，在 righ 单击**Active Directory**选择**编辑索赔规则**.
3.  在**编辑索赔规则的有源主管**单击**添加规则**。
4.  在**添加转换索赔规则向导**选择**穿透或筛选器传入的声明**单击**下一步**。
5.  添加的显示名称，然后选择**Windows 设备组**从**传入声称类型**下拉列表。
6.  单击**完成**。  单击**应用**和**确定**。 
![声称描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>第 8 步：在依赖方地方 'WindowsDeviceGroup 索赔，添加类似 '直通或者将' 索赔规则。
2.  在**广告 FS 管理**，单击**信赖方信任**右窗格中，在 righ 单击 RP，然后选择**编辑索赔规则**。
3.  在**颁发转换规则**单击**添加规则**。
4.  在**添加转换索赔规则向导**选择**穿透或筛选器传入的声明**单击**下一步**。
5.  添加的显示名称，然后选择**Windows 设备组**从**传入声称类型**下拉列表。
6.  单击**完成**。  单击**应用**和**确定**。
![声称描述](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 配置广告 FS 步骤
下面将详细广告 FS 上的复合身份验证配置 Windows Server 2016 的步骤。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>第 1 步：启用 KDC 索赔，复合身份验证、Kerberos 程度上默认域控制器策略的支持
1.  在服务器管理器中，选择工具**组策略管理**。
2.  向下导航**默认域控制器策略**、右键单击，然后选择**编辑**。
3.  在**组策略编辑器中管理**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**KDC**。
4.  右侧窗格中，双击**KDC 支持索赔，复合身份验证、Kerberos 程度**。
5.  在新的对话框窗口中，将设置为索赔 KDC 支持**启用**。
6.  在选项选择**支持**从下拉菜单中，然后单击**应用**和**确定**。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>第 2 步：启用 Kerberos 客户端支持索赔、复合身份验证、和 Kerberos 程度访问联盟应用程序的计算机上

1.  组策略应用到访问联盟应用程序中的计算机上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**Kerberos**。
2.  右窗格的组策略编辑器中管理窗口中，双击**Kerberos 客户端支持索赔、复合身份验证、和 Kerberos 程度。**
3.  在新的对话框窗口中，设置为 Kerberos 客户端支持**启用**单击**应用**和**确定**。
4.  关闭组策略管理编辑器。

### <a name="step-3-configure-the-primary-authentication-provider"></a>第 3 步：配置的主要身份验证提供

1. 设置为主要身份验证提供**Windows 身份验证**广告 FS Intranet 设置。
2. 在广告 FS 管理下**认证策略**，选择**主要身份验证**下**全球设置**单击**编辑**。
3. 在**编辑策略全球的身份验证**下**Intranet**选择**Windows 身份验证**。
4. 单击**应用**和**确定**。
5. 使用你可以使用 PowerShell**组 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在基于 WID 电场的日落，必须执行 PowerShell 命令主广告 FS 服务器上。
>在基于 SQL 电场的日落，可能是农场里的成员任何广告 FS 服务器上执行 PowerShell 命令。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>第 4 步：启用 msDS SupportedEncryptionTypes 属性上的复合身份验证位

1.  启用复合身份验证 msDS SupportedEncryptionTypes 特性命名运行广告 FS 服务使用的帐户上位**组 ADServiceAccount** PowerShell cmdlet。  

>[!NOTE]
>如果你更改服务帐户，然后通过运行必须手动启用复合身份验证**组 ADUser compoundIdentitySupported: $true** Windows PowerShell cmdlet.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  重新启动 ADFS 服务。

>[!NOTE]
>一旦 'CompoundIdentitySupported 设置为相同的新服务器 (2012R2/2016) 失败，错误如下–gMSA 如此，安装**安装 ADServiceAccount：无法安装服务的帐户。错误消息: 提供的上下文不匹配目标。**.
>
>**解决方案**：暂时 $false 设置 CompoundIdentitySupported。 此步骤导致 ADFS 以停止发出 WindowsDeviceGroup 索赔。 Set-ADServiceAccount-身份 ADFS 服务帐户-CompoundIdentitySupported: $false 安装在新的服务器上 gMSA，然后返回到 $True 启用 CompoundIdentitySupported。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>第 5 步：为 Active Directory 信任广告 FS 索赔提供程序的更新

1. 更新广告 FS 索赔提供商信任 Active Directory 的 WindowsDeviceGroup' 索赔包含以下 '直通索赔规则。
2.  在**广告 FS 管理**，单击**索赔提供商信任**右窗格中，在 righ 单击**Active Directory**选择**编辑索赔规则**.
3.  在**编辑索赔规则的有源主管**单击**添加规则**。
4.  在**添加转换索赔规则向导**选择**穿透或筛选器传入的声明**单击**下一步**。
5.  添加的显示名称，然后选择**Windows 设备组**从**传入声称类型**下拉列表。
6.  单击**完成**。  单击**应用**和**确定**。 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>第 6 步：在依赖方地方 'WindowsDeviceGroup 索赔，添加类似 '直通或者将' 索赔规则。
2.  在**广告 FS 管理**，单击**信赖方信任**右窗格中，在 righ 单击 RP，然后选择**编辑索赔规则**。
3.  在**颁发转换规则**单击**添加规则**。
4.  在**添加转换索赔规则向导**选择**穿透或筛选器传入的声明**单击**下一步**。
5.  添加的显示名称，然后选择**Windows 设备组**从**传入声称类型**下拉列表。
6.  单击**完成**。  单击**应用**和**确定**。

## <a name="validation"></a>验证
若要验证 WindowsDeviceGroup' 索赔发布，创建一个测试索赔感知使用.Net 4.6 的应用程序。 与 WIF SDK 4.0。
配置为依赖 ADFS 云中方应用程序，并对其进行更新索赔规则规定上述步骤。
使用 Windows 的集成身份验证的 ADFS 的提供商的应用进行身份验证时, 以下索赔是强制转换。
![验证](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

更丰富的访问权限控件可能现在消耗计算机设备提起的索赔。

例如，以下**AdditionalAuthenticationRules**告诉广告 FS 调用 MFA 如果–进行身份验证的用户不安全的组成员的"-1-5-21-2134745077-1211275016-3050530490-1117"和计算机（用户在哪里是从进行身份验证）不成员的安全组"S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

但是，如果满足任一上述情形，不调用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```