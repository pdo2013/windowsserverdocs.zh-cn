---
title: 复合身份验证和 Active Directory 域服务在 Active Directory 联合身份验证服务中的声明
description: 以下文档介绍了复合身份验证和 AD FS 中的 AD DS 声明。
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2380060894ff2f365451bbabfd41b8aa7e6792a0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445292"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>复合身份验证和 AD FS 中的 AD DS 声明
Windows Server 2012 引入了复合身份验证，从而增强了 Kerberos 身份验证。  复合身份验证，包括两个身份的 Kerberos 票证授予服务 (TGS) 请求： 

- 用户的标识
- 用户的设备的标识。  

Windows 通过扩展来实现复合身份验证[Kerberos 灵活身份验证安全隧道 (FAST)，或 Kerberos 保护](https://technet.microsoft.com/library/hh831747.aspx)。 

AD FS 2012 及更高版本允许使用的 AD DS 颁发驻留在 Kerberos 身份验证票证的用户或设备声明。 在以前版本的 AD FS 中，的声明引擎无法用于从 Kerberos 只能读取用户和组安全 Id (Sid)，但无法读取任何声明的 Kerberos 票证中包含的信息。

可以使用 Active Directory 域服务 (AD DS) 来启用更丰富的联合应用程序的访问控制-在一起，通过 Active Directory 联合身份验证服务 (AD FS) 颁发的用户和设备声明。

## <a name="requirements"></a>要求
1.  访问联合应用程序的计算机必须进行身份验证使用 AD FS **Windows 集成身份验证**。 
    - 连接到后端的 AD FS 服务器时，Windows 集成身份验证才可用。
    - 计算机必须能够访问联合身份验证服务名称后端的 AD FS 服务器
    - AD FS 服务器必须作为在其 Intranet 设置中的主要身份验证方法提供 Windows 集成身份验证。

2.  策略**声明复合身份验证和 Kerberos 保护的 Kerberos 客户端支持**必须应用于所有计算机访问受复合身份验证的联合应用程序。 这是在单个林或跨林方案适用。

3.  其中包含 AD FS 服务器的域必须具有**KDC 支持声明复合身份验证和 Kerberos 保护**策略设置应用于域控制器。

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 中配置 AD FS 的步骤
使用以下步骤配置复合身份验证和声明 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>第 1 步：启用 KDC 支持声明、 复合身份验证和 Kerberos 保护的默认域控制器策略
1.  在服务器管理器中，选择工具**组策略管理**。
2.  向下导航**默认域控制器策略**，右键单击并选择**编辑**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**依次展开**系统**，然后选择**KDC**。
4.  在右窗格中，双击**KDC 支持声明、 复合身份验证和 Kerberos 保护**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  在新的对话框窗口中，设置 KDC 支持声明为**已启用**。
6.  在选项下，选择**支持**从下拉列表菜单，然后单击**应用**并**确定**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步骤 2：启用 Kerberos 客户端支持声明、 复合身份验证和 Kerberos 保护访问联合应用程序的计算机上

1.  在组策略应用到在中访问联合应用程序的计算机上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**Kerberos**。
2.  在组策略管理编辑器窗口的右窗格中，双击**Kerberos 客户端支持声明、 复合身份验证和 Kerberos 保护。**
3.  在新的对话框窗口中，将 Kerberos 客户端支持设置为**已启用**然后单击**应用**并**确定**。
![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  关闭“组策略管理编辑器”。

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>步骤 3:请确保已将 AD FS 服务器更新。
您需要确保在 AD FS 服务器上安装以下更新。

|Update|描述|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|累积性安全更新 （包括 KB2919355、 KB2932046、 KB2934018、 KB2937592、 KB2938439）|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|适用于 Server 2012 R2 更新|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|此更新 Active Directory 联合身份验证服务中添加支持复合 ID 声明。|

### <a name="step-4-configure-the-primary-authentication-provider"></a>步骤 4：配置主要身份验证提供程序

1. 设置为主要身份验证提供程序**Windows 身份验证**为 AD FS Intranet 设置。
2. 在 AD FS 管理下**身份验证策略**，选择**主要身份验证**并在**全局设置**单击**编辑**。
3. 上**编辑全局身份验证策略**下**Intranet**选择**Windows 身份验证**。
4. 单击**应用**并**确定**。

![组策略管理](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. 使用的 PowerShell 可以使用**集 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>基础之上的 WID 场中，必须在主 AD FS 服务器执行命令的 PowerShell。
>在基于 SQL 场中，可能是服务器场的成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>步骤 5：添加到 AD FS 声明说明
1. 向场中添加以下声明说明。 此声明描述不存在默认情况下 ADFS 2012 R2 中，并需要手动添加。
2. 在 AD FS 管理下**服务**，右键单击**索赔说明**，然后选择**添加声明说明**
3. 声明说明中输入以下信息
   - 显示名称：Windows 设备组 
   - 声明说明:<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>'
4. 选中这两个框。
5. 单击 **“确定”** 。

![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. 使用的 PowerShell 可以使用**添加 AdfsClaimDescription** cmdlet。
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>基础之上的 WID 场中，必须在主 AD FS 服务器执行命令的 PowerShell。
>在基于 SQL 场中，可能是服务器场的成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步骤 6：启用 Msds-supportedencryptiontypes 属性上的复合身份验证位

1.  启用复合身份验证指定为运行 AD FS 服务使用的帐户的 Msds-supportedencryptiontypes 属性位**Set-adserviceaccount** PowerShell cmdlet。  

>[!NOTE]
>如果你更改服务帐户，则你必须手动启用复合身份验证，通过运行**Set-aduser compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新启动 ADFS 服务。

>[!NOTE]
>一旦 CompoundIdentitySupported 设置为 true，安装新服务器 (2012R2/2016) 失败并出现以下错误 – 在同一个 gMSA **Install-adserviceaccount:无法安装服务帐户。错误消息：所提供的上下文不匹配目标。** .
>
>**解决方案**：暂时将 CompoundIdentitySupported 设置为 $false。 此步骤导致 ADFS 停止发出 WindowsDeviceGroup 声明。 Set-adserviceaccount-标识 ADFS 服务帐户 CompoundIdentitySupported: $false 新服务器上安装 gMSA，然后返回到 $True 启用 CompoundIdentitySupported。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步骤 7：Active directory 更新 AD FS 声明提供方信任

1. 更新 AD FS 声明提供方信任的 Active Directory WindowsDeviceGroup 声明中包含以下直通声明规则。
2.  在中**AD FS 管理**，单击**声明提供方信任**并在右窗格中，右键单击**Active Directory** ，然后选择**编辑声明规则**.
3.  上**编辑声明规则的 Active Director**单击**添加规则**。
4.  上**添加转换声明规则向导**选择**传递或筛选传入声明**然后单击**下一步**。
5.  添加显示名称，然后选择**Windows 设备组**从**传入声明类型**下拉列表。
6.  单击 **“完成”** 。  单击**应用**并**确定**。 
![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步骤 8：在信赖方需要 WindowsDeviceGroup 声明的地方，添加类似直通或者转换声明规则。
2. 在**AD FS 管理**，单击**信赖方信任**并在右窗格中，右键单击你的 RP 然后选择**编辑声明规则**。
3. 上**颁发转换规则**单击**添加规则**。
4. 上**添加转换声明规则向导**选择**传递或筛选传入声明**然后单击**下一步**。
5. 添加显示名称，然后选择**Windows 设备组**从**传入声明类型**下拉列表。
6. 单击 **“完成”** 。  单击**应用**并**确定**。
   ![声明说明](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>在 Windows Server 2016 中配置 AD FS 的步骤
以下情况将详细介绍适用于 Windows Server 2016 AD FS 上配置复合身份验证的步骤。

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>第 1 步：启用 KDC 支持声明、 复合身份验证和 Kerberos 保护的默认域控制器策略
1.  在服务器管理器中，选择工具**组策略管理**。
2.  向下导航**默认域控制器策略**，右键单击并选择**编辑**。
3.  上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**依次展开**系统**，然后选择**KDC**。
4.  在右窗格中，双击**KDC 支持声明、 复合身份验证和 Kerberos 保护**。
5.  在新的对话框窗口中，设置 KDC 支持声明为**已启用**。
6.  在选项下，选择**支持**从下拉列表菜单，然后单击**应用**并**确定**。


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>步骤 2：启用 Kerberos 客户端支持声明、 复合身份验证和 Kerberos 保护访问联合应用程序的计算机上

1.  在组策略应用到在中访问联合应用程序的计算机上**组策略管理编辑器**下**计算机配置**，展开**策略**，展开**管理模板**，展开**系统**，然后选择**Kerberos**。
2.  在组策略管理编辑器窗口的右窗格中，双击**Kerberos 客户端支持声明、 复合身份验证和 Kerberos 保护。**
3.  在新的对话框窗口中，将 Kerberos 客户端支持设置为**已启用**然后单击**应用**并**确定**。
4.  关闭“组策略管理编辑器”。

### <a name="step-3-configure-the-primary-authentication-provider"></a>步骤 3:配置主要身份验证提供程序

1. 设置为主要身份验证提供程序**Windows 身份验证**为 AD FS Intranet 设置。
2. 在 AD FS 管理下**身份验证策略**，选择**主要身份验证**并在**全局设置**单击**编辑**。
3. 上**编辑全局身份验证策略**下**Intranet**选择**Windows 身份验证**。
4. 单击**应用**并**确定**。
5. 使用的 PowerShell 可以使用**集 AdfsGlobalAuthenticationPolicy** cmdlet。

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>基础之上的 WID 场中，必须在主 AD FS 服务器执行命令的 PowerShell。
>在基于 SQL 场中，可能是服务器场的成员的任何 AD FS 服务器上执行 PowerShell 命令。

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>步骤 4：启用 Msds-supportedencryptiontypes 属性上的复合身份验证位

1.  启用复合身份验证指定为运行 AD FS 服务使用的帐户的 Msds-supportedencryptiontypes 属性位**Set-adserviceaccount** PowerShell cmdlet。  

>[!NOTE]
>如果你更改服务帐户，则你必须手动启用复合身份验证，通过运行**Set-aduser compoundIdentitySupported: $true** Windows PowerShell cmdlet。

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. 重新启动 ADFS 服务。

>[!NOTE]
>一旦 CompoundIdentitySupported 设置为 true，安装新服务器 (2012R2/2016) 失败并出现以下错误 – 在同一个 gMSA **Install-adserviceaccount:无法安装服务帐户。错误消息：所提供的上下文不匹配目标。** .
>
>**解决方案**：暂时将 CompoundIdentitySupported 设置为 $false。 此步骤导致 ADFS 停止发出 WindowsDeviceGroup 声明。 Set-adserviceaccount-标识 ADFS 服务帐户 CompoundIdentitySupported: $false 新服务器上安装 gMSA，然后返回到 $True 启用 CompoundIdentitySupported。
禁用 CompoundIdentitySupported，然后重新启用不需要重新启动 ADFS 服务。

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>步骤 5：Active directory 更新 AD FS 声明提供方信任

1. 更新 AD FS 声明提供方信任的 Active Directory WindowsDeviceGroup 声明中包含以下直通声明规则。
2.  在中**AD FS 管理**，单击**声明提供方信任**并在右窗格中，右键单击**Active Directory** ，然后选择**编辑声明规则**.
3.  上**编辑声明规则的 Active Director**单击**添加规则**。
4.  上**添加转换声明规则向导**选择**传递或筛选传入声明**然后单击**下一步**。
5.  添加显示名称，然后选择**Windows 设备组**从**传入声明类型**下拉列表。
6.  单击 **“完成”** 。  单击**应用**并**确定**。 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>步骤 6：在信赖方需要 WindowsDeviceGroup 声明的地方，添加类似直通或者转换声明规则。
2. 在**AD FS 管理**，单击**信赖方信任**并在右窗格中，右键单击你的 RP 然后选择**编辑声明规则**。
3. 上**颁发转换规则**单击**添加规则**。
4. 上**添加转换声明规则向导**选择**传递或筛选传入声明**然后单击**下一步**。
5. 添加显示名称，然后选择**Windows 设备组**从**传入声明类型**下拉列表。
6. 单击 **“完成”** 。  单击**应用**并**确定**。

## <a name="validation"></a>验证
若要验证版本的 WindowsDeviceGroup 声明，请创建测试的声明感知应用程序使用.Net 4.6。 使用 WIF SDK 4.0。
配置为在 ADFS 信赖方应用程序和使用声明规则更新为前面步骤中指定。
对使用 Windows 集成身份验证提供程序的 ADFS 的应用程序进行身份验证，以下声明时，强制转换。
![验证](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

计算机/设备的声明现在能够用于更丰富的访问控制。

例如 – 下面**AdditionalAuthenticationRules**告知 AD FS 以如果 – 进行身份验证的用户不是安全组的成员调用 MFA"-1-5-21-2134745077-1211275016-3050530490-1117"和计算机 (其中是用户是从进行身份验证） 不是安全组"S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"的成员

但是，如果满足上述条件的任何不调用 MFA。

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
