---
title: 复合身份验证和 Active Directory 域服务声明 Active Directory 联合身份验证服务
description: 以下文档讨论了 AD FS 中的复合身份验证和 AD DS 声明。
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b67177c8bf0ce9869aa51c3012d57f3208ac02f5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866289"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>复合身份验证和 AD FS 中的 AD DS 声明
Windows Server 2012 引入了复合身份验证，从而增强了 Kerberos 身份验证。  复合身份验证使 Kerberos 票证授予服务（TGS）请求包含两个标识： 

- 用户的标识
- 用户设备的标识。  

Windows 通过扩展[Kerberos 灵活身份验证安全隧道（FAST）或 Kerberos](https://technet.microsoft.com/library/hh831747.aspx)保护来完成复合身份验证。 

AD FS 2012 及更高版本允许 AD DS 颁发的用户或传入 Kerberos 身份验证票证的设备声明。 在以前版本的 AD FS 中，声明引擎只能读取 Kerberos 中的用户和组安全 Id （Sid），但无法读取任何包含在 Kerberos 票证中的声明信息。

你可以通过使用 Active Directory 域服务（AD DS）的用户和设备声明以及 Active Directory 联合身份验证服务（AD FS）为联合应用程序启用更丰富的访问控制。

## <a name="requirements"></a>要求
1.  访问联合应用程序的计算机必须使用**Windows 集成身份验证**对 AD FS 进行身份验证。 
    - Windows 集成身份验证仅在连接到后端 AD FS 服务器时可用。
    - 计算机必须能够访问联合身份验证服务名称的后端 AD FS 服务器
    - AD FS 服务器必须在其 Intranet 设置中提供 Windows 集成身份验证作为主要身份验证方法。

2.  **对于声明复合身份验证和 kerberos 保护的策略，kerberos 客户端支持**必须应用于访问由复合身份验证保护的联合应用程序的所有计算机。 这适用于单个林或跨林方案。

3.  承载 AD FS 服务器的域必须具有应用于域控制器的 "**声明复合身份验证和 Kerberos**保护" 策略设置的 KDC 支持。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 中配置 AD FS 的步骤
使用以下步骤来配置复合身份验证和声明 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步骤 1：在默认域控制器策略上启用 KDC 支持声明、复合身份验证和 Kerberos 保护
1.  在服务器管理器中，选择 "工具"、"**组策略管理**"。
2.  向下导航到 "**默认域控制器策略**"，右键单击并选择 "**编辑**"。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  在**组策略管理编辑器**上的 "**计算机配置**" 下，展开 "**策略**"，展开 "**管理模板**"，展开 "**系统**"，然后选择 " **KDC**"。
4.  在右侧窗格中，双击 " **KDC 支持声明、复合身份验证和 Kerberos**保护"。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在 "新建" 对话框窗口中，将 "KDC 支持" 设置为 "**已启用**"。
6.  在 "选项" 下，从下拉菜单中选择 "**支持**"，然后单击 "**应用** **" 和 "确定"** 。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步骤 2：在访问联合应用程序的计算机上启用 Kerberos 客户端支持声明、复合身份验证和 Kerberos 保护

1.  在应用到访问联合应用程序的计算机的组策略上，在**组策略管理编辑器**的 "**计算机配置**" 下，展开 "**策略**"，展开 "**管理模板**，展开**系统**，然后选择 " **Kerberos**"。
2.  在 "组策略管理编辑器" 窗口的右窗格中，双击 " **kerberos 客户端支持声明、复合身份验证和 kerberos**保护"。
3.  在 "新建" 对话框窗口中，将 "Kerberos 客户端支持" 设置为 "**已启用**"，然后单击 "**应用** **"**
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  关闭“组策略管理编辑器”。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步骤 3：确保已更新 AD FS 服务器。
你需要确保在 AD FS 服务器上安装下列更新。

|Update|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|累积安全更新（包括 KB2919355、KB2932046、KB2934018、KB2937592、KB2938439）|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|服务器 2012 R2 更新|
|[修补程序3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|此更新在 Active Directory 联合身份验证服务中添加了对复合 ID 声明的支持。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>步骤 4：配置主身份验证提供程序

1. 将主身份验证提供程序设置为**Windows 身份验证**AD FS Intranet 设置。
2. 在 AD FS 管理 "下的"**身份验证策略**"下，选择"**主要身份验证**"，然后**在"** **全局设置**"下
3. 在**Intranet**下**编辑全局身份验证策略**选择**Windows 身份验证**。
4. 单击 "**应用**"，然后单击 **"确定"** 。

![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用 PowerShell 时，可以使用**AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在基于 WID 的服务器场中，必须在主 AD FS 服务器上执行 PowerShell 命令。
>在基于 SQL 的服务器场中，可以在作为场成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>步骤 5：将声明说明添加到 AD FS
1. 将以下声明说明添加到场中。 默认情况下，ADFS 2012 R2 中不存在此声明说明，需要手动添加。
2. 在 AD FS 管理 "下的"**服务**"下，右键单击"**声明说明**"，然后选择"**添加声明说明**"
3. 在声明说明中输入以下信息
   - 显示名称："Windows 设备组" 
   - 声明说明： "<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>"
4. 选中两个框。
5. 单击 **“确定”** 。

![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用 PowerShell 时，可以使用**AdfsClaimDescription** cmdlet。
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>在基于 WID 的服务器场中，必须在主 AD FS 服务器上执行 PowerShell 命令。
>在基于 SQL 的服务器场中，可以在作为场成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步骤 6：对 Msds-supportedencryptiontypes 属性启用复合身份验证位

1.  使用**Uninstall-adserviceaccount** PowerShell cmdlet 在指定用于运行 AD FS 服务的帐户上的 msds-supportedencryptiontypes 属性上启用复合身份验证位。  

>[!NOTE]
>如果更改服务帐户，则必须通过运行**new-aduser-compoundIdentitySupported： $true** Windows PowerShell cmdlet 来手动启用复合身份验证。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新启动 ADFS 服务。

>[!NOTE]
>如果 "CompoundIdentitySupported" 设置为 true，则在新服务器上安装相同的 gMSA （2012R2/2016）会失败，并出现以下**错误：安装-uninstall-adserviceaccount：无法安装服务帐户。错误消息："提供的上下文与目标不匹配。"** .
>
>**解决方案**：暂时将 CompoundIdentitySupported 设置为 $false。 此步骤会导致 ADFS 停止发出 WindowsDeviceGroup 声明。 Uninstall-adserviceaccount-Identity "ADFS Service Account"-CompoundIdentitySupported： $false 在新服务器上安装 gMSA，然后将 CompoundIdentitySupported 重新启用为 $True。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步骤 7：更新 Active Directory 的 AD FS 声明提供程序信任

1. 更新 Active Directory 的 AD FS 声明提供方信任，以便为 "WindowsDeviceGroup" 声明包含以下 "传递" 声明规则。
2.  在**AD FS 管理**"中，单击"**声明提供方信任**"，然后在右侧窗格中，右键单击" **Active Directory** "，然后选择"**编辑声明规则**"。
3.  在 "**编辑活动控制器的声明规则**" 中，单击 "**添加规则**"。
4.  在 "**添加转换声明规则向导**" 中选择 "**传递或筛选传入声明**"，然后单击 "**下一步**"。
5.  从 "**传入声明类型**" 下拉添加显示名称并选择 " **Windows 设备组**"。
6.  单击 **“完成”** 。  单击 "**应用**"，然后单击 **"确定"** 。 
![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步骤 8：在需要 "WindowsDeviceGroup" 声明的信赖方上，添加类似的 "传递" 或 "转换" 声明规则。
2. 在**AD FS 管理**"中，单击"**信赖方信任**"，然后在右侧窗格中，单击" 右键 "，并选择"**编辑声明规则**"。
3. 在 "**颁发转换规则**" 中，单击 "**添加规则**"。
4. 在 "**添加转换声明规则向导**" 中选择 "**传递或筛选传入声明**"，然后单击 "**下一步**"。
5. 从 "**传入声明类型**" 下拉添加显示名称并选择 " **Windows 设备组**"。
6. 单击 **“完成”** 。  单击 "**应用**"，然后单击 **"确定"** 。
   ![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 中配置 AD FS 的步骤
下面将详细说明在 Windows Server 2016 AD FS 上配置复合身份验证的步骤。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>步骤 1：在默认域控制器策略上启用 KDC 支持声明、复合身份验证和 Kerberos 保护
1.  在服务器管理器中，选择 "工具"、"**组策略管理**"。
2.  向下导航到 "**默认域控制器策略**"，右键单击并选择 "**编辑**"。
3.  在**组策略管理编辑器**上的 "**计算机配置**" 下，展开 "**策略**"，展开 "**管理模板**"，展开 "**系统**"，然后选择 " **KDC**"。
4.  在右侧窗格中，双击 " **KDC 支持声明、复合身份验证和 Kerberos**保护"。
5.  在 "新建" 对话框窗口中，将 "KDC 支持" 设置为 "**已启用**"。
6.  在 "选项" 下，从下拉菜单中选择 "**支持**"，然后单击 "**应用** **" 和 "确定"** 。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步骤 2：在访问联合应用程序的计算机上启用 Kerberos 客户端支持声明、复合身份验证和 Kerberos 保护

1.  在应用到访问联合应用程序的计算机的组策略上，在**组策略管理编辑器**的 "**计算机配置**" 下，展开 "**策略**"，展开 "**管理模板**，展开**系统**，然后选择 " **Kerberos**"。
2.  在 "组策略管理编辑器" 窗口的右窗格中，双击 " **kerberos 客户端支持声明、复合身份验证和 kerberos**保护"。
3.  在 "新建" 对话框窗口中，将 "Kerberos 客户端支持" 设置为 "**已启用**"，然后单击 "**应用** **"**
4.  关闭“组策略管理编辑器”。

### <a name="step-3-configure-the-primary-authentication-provider"></a>步骤 3：配置主身份验证提供程序

1. 将主身份验证提供程序设置为**Windows 身份验证**AD FS Intranet 设置。
2. 在 AD FS 管理 "下的"**身份验证策略**"下，选择"**主要身份验证**"，然后**在"** **全局设置**"下
3. 在**Intranet**下**编辑全局身份验证策略**选择**Windows 身份验证**。
4. 单击 "**应用**"，然后单击 **"确定"** 。
5. 使用 PowerShell 时，可以使用**AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>在基于 WID 的服务器场中，必须在主 AD FS 服务器上执行 PowerShell 命令。
>在基于 SQL 的服务器场中，可以在作为场成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步骤 4：对 Msds-supportedencryptiontypes 属性启用复合身份验证位

1.  使用**Uninstall-adserviceaccount** PowerShell cmdlet 在指定用于运行 AD FS 服务的帐户上的 msds-supportedencryptiontypes 属性上启用复合身份验证位。  

>[!NOTE]
>如果更改服务帐户，则必须通过运行**new-aduser-compoundIdentitySupported： $true** Windows PowerShell cmdlet 来手动启用复合身份验证。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新启动 ADFS 服务。

>[!NOTE]
>如果 "CompoundIdentitySupported" 设置为 true，则在新服务器上安装相同的 gMSA （2012R2/2016）会失败，并出现以下**错误：安装-uninstall-adserviceaccount：无法安装服务帐户。错误消息："提供的上下文与目标不匹配。"** .
>
>**解决方案**：暂时将 CompoundIdentitySupported 设置为 $false。 此步骤会导致 ADFS 停止发出 WindowsDeviceGroup 声明。 Uninstall-adserviceaccount-Identity "ADFS Service Account"-CompoundIdentitySupported： $false 在新服务器上安装 gMSA，然后将 CompoundIdentitySupported 重新启用为 $True。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步骤 5：更新 Active Directory 的 AD FS 声明提供程序信任

1. 更新 Active Directory 的 AD FS 声明提供方信任，以便为 "WindowsDeviceGroup" 声明包含以下 "传递" 声明规则。
2.  在**AD FS 管理**"中，单击"**声明提供方信任**"，然后在右侧窗格中，右键单击" **Active Directory** "，然后选择"**编辑声明规则**"。
3.  在 "**编辑活动控制器的声明规则**" 中，单击 "**添加规则**"。
4.  在 "**添加转换声明规则向导**" 中选择 "**传递或筛选传入声明**"，然后单击 "**下一步**"。
5.  从 "**传入声明类型**" 下拉添加显示名称并选择 " **Windows 设备组**"。
6.  单击 **“完成”** 。  单击 "**应用**"，然后单击 **"确定"** 。 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步骤 6：在需要 "WindowsDeviceGroup" 声明的信赖方上，添加类似的 "传递" 或 "转换" 声明规则。
2. 在**AD FS 管理**"中，单击"**信赖方信任**"，然后在右侧窗格中，单击" 右键 "，并选择"**编辑声明规则**"。
3. 在 "**颁发转换规则**" 中，单击 "**添加规则**"。
4. 在 "**添加转换声明规则向导**" 中选择 "**传递或筛选传入声明**"，然后单击 "**下一步**"。
5. 从 "**传入声明类型**" 下拉添加显示名称并选择 " **Windows 设备组**"。
6. 单击 **“完成”** 。  单击 "**应用**"，然后单击 **"确定"** 。

## <a name="validation"></a>验证
若要验证 "WindowsDeviceGroup" 声明的版本，请使用 .Net 4.6 创建测试声明感知应用程序。 With WIF SDK 4.0。
在 ADFS 中将应用程序配置为信赖方，并使用上述步骤中指定的声明规则对其进行更新。
使用 ADFS 的 Windows 集成身份验证提供程序对应用程序进行身份验证时，以下声明为强制转换。
![验证](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

现在可以使用计算机/设备的声明进行更丰富的访问控制。

例如，以下**AdditionalAuthenticationRules**告知 AD FS 在–身份验证用户不是安全组 "-1-5-21-2134745077-1211275016-3050530490-1117" 和计算机（其中，用户为身份验证来自）不是安全组 "S-1-5-21-2134745077-1211275016-3050530490-1115 （WindowsDeviceGroup）" 的成员

但是，如果满足上述任一条件，则不会调用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
