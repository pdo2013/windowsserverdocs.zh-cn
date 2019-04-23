---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: AD FS 快速还原工具
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6fb023529ac8857f7c2eb35586be497f0c809a51
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874168"
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速还原工具

>适用于：Windows Server 2016, Windows Server 2012 R2

## <a name="overview"></a>概述
现在 AD FS 是实现的高可用性设置 AD FS 场。 某些组织希望只有一台服务器的 AD FS 部署的方法，无需多个 AD FS 服务器和网络负载平衡的基础结构，同时使某些服务的保障可以快速还原是否存在问题。
新的 AD FS 快速还原工具提供了一种方法，而无需完整备份和还原操作系统或系统状态还原 AD FS 数据。 新的工具可用于导出 AD FS 配置到 Azure 或本地位置。  然后可以将导出的数据应用到新的 AD FS 安装，重新创建或复制 AD FS 环境。 

## <a name="scenarios"></a>方案
可以在以下方案中使用 AD FS 快速还原工具：

1. 短时间内出现问题后恢复 AD FS 功能
    - 该工具用于创建可以快速部署来代替 online 的 AD FS 服务器的 AD FS 的冷备用安装
2. 部署相同的测试和生产环境
    - 使用该工具，快速在测试环境中，创建生产 AD FS 的准确副本或快速部署到生产环境的已验证的测试配置

## <a name="what-is-backed-up"></a>备份的内容
该工具将备份以下 AD FS 配置
    
- AD FS 配置数据库 （SQL 或 WID）
- 配置文件 （位于 AD FS 文件夹中）
- 自动生成的令牌签名和解密证书和私钥 （从 Active Directory DKM 容器中）
- SSL 证书和任何外部注册的证书 （令牌签名、 令牌解密和服务通信） 和相应的私钥 (注意： 必须是可导出私钥并运行该脚本的用户必须有权对其进行访问)
- 列表的自定义身份验证提供程序、 属性存储和本地声明提供方信任的安装。

## <a name="how-to-use-the-tool"></a>如何使用该工具
首先，[下载](https://go.microsoft.com/fwlink/?LinkId=825646)和 AD FS 服务器上安装 MSI。  

在 PowerShell 提示符下运行以下命令：

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果使用 Windows 集成数据库 (WID)，此工具需要在主 AD FS 服务器上运行。  可以使用`Get-AdfsSyncProperties`PowerShell cmdlet 来确定是否在服务器位于是主服务器。

### <a name="system-requirements"></a>系统要求

- 此工具适用于 Windows Server 2012 R2 及更高版本的 AD FS。 
- 所需的.NET framework 为至少 4.0。 
- 必须与备份相同的版本的 AD FS 服务器上完成还原并使用同一个 Active Directory 帐户作为 AD FS 服务帐户。

## <a name="create-a-backup"></a>创建备份
若要创建备份，使用备份 ADFS cmdlet。 此 cmdlet 将备份的 AD FS 配置、 数据库、 SSL 证书，等等。 

用户必须至少是本地管理员才能运行此 cmdlet。 若要备份 Active Directory DKM 容器 （需要在默认的 AD FS 配置中），用户必须是域管理员，需将传入的 AD FS 服务帐户凭据，或有权访问 DKM 容器。  如果使用 gMSA 帐户，用户必须是域管理员或具有到容器; 的权限不能提供的 gMSA 凭据。 

备份将根据"adfsBackup_ID_Date 时间"的模式进行命名。 它将包含版本号、 日期和时间执行备份的。
该 cmdlet 将接受以下参数：
    
参数集

![AD FS 快速还原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>详细的说明

- **BackupDKM** -备份包含的默认配置 （自动生成的令牌签名和解密证书） 中的 AD FS 键的 Active Directory DKM 容器。 这可以使用 AD 工具 ldifde 要导出的 AD 容器和所有子树。

- -**StorageType&lt;字符串&gt;** -用户想要使用的存储的类型。 "FileSystem"指示用户想要将其存储在一个文件夹中，本地或在"Azure"指示用户想要将其存储在 Azure 存储容器中，当用户执行备份时，他们选择备份位置，文件系统的网络或中云。 若要使用的 azure，Azure 存储凭据应传递给 cmdlet。 存储凭据包含帐户名称和密钥。 除此之外，容器名称也必须传递中。 如果该容器不存在，它在备份期间创建。 若要使用的文件系统，必须提供存储路径。 在该目录中，将为每个备份创建新目录。 创建每个目录将包含备份的文件。 

- **EncryptionPassword&lt;字符串&gt;** -要用于存储在存储之前加密备份的所有文件的密码

- **AzureConnectionCredentials &lt;pscredential&gt;**  -帐户名称和 Azure 存储帐户密钥

- **AzureStorageContainer&lt;字符串&gt;** -在 Azure 中存储备份的存储容器

- **StoragePath&lt;字符串&gt;** -备份将存储在该位置

- **ServiceAccountCredential &lt;pscredential&gt;**  -指定要用于当前运行的 AD FS 服务的服务帐户。 此参数时才需要用户想要备份 DKM 并不是域管理员或不能访问到容器的内容。 

- **BackupComment &lt;string []&gt;**  -有关备份还原过程中，类似于 HYPER-V 检查点命名的概念将显示一个信息性字符串。 默认值为空字符串

 
## <a name="backup-examples"></a>备份示例
以下是使用 AD FS 快速还原工具备份示例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>备份 AD FS 配置，使用 DKM，到文件系统和有权访问 DKM 容器内容 (域管理员或委派的)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>备份 AD FS 配置，使用 DKM，到使用服务帐户凭据，以本地管理员身份运行文件系统

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>备份 AD FS 配置，而不到 Azure 存储容器 DKM。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>备份 AD FS 配置，而不到文件系统 DKM

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>从备份中还原
若要将应用到新的 AD FS 安装使用 ADFS 备份创建的配置，使用还原 ADFS cmdlet。

此 cmdlet 将创建新的 AD FS 场使用 cmdlet`Install-AdfsFarm`和还原 AD FS 配置、 数据库、 证书等。如果未在服务器上安装 AD FS 角色，该 cmdlet 将安装它。  该 cmdlet 将检查现有备份的还原位置，并提示用户选择适当的备份基于它创建的日期/时间和用户可能已附加到备份任何备份注释。 如果有多个 AD FS 配置了不同的联合身份验证服务名称，然后被提示用户首先选择适当的 AD FS 配置。
用户必须是本地和域管理员才能运行此 cmdlet。


>[!NOTE] 
>使用 AD FS 快速恢复工具之前，请确保服务器已加入域之前还原备份。 

该 cmdlet 将接受以下参数： 

![AD FS 快速还原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>详细的说明

- **StorageType&lt;字符串&gt;** -用户想要使用的存储的类型。
 "FileSystem"指示用户想要将其存储在本地文件夹或网络中"Azure"指示用户想要将其存储在 Azure 存储容器

- **DecryptionPassword&lt;字符串&gt;** -用于加密备份的所有文件的密码 

- **AzureConnectionCredentials &lt;pscredential&gt;**  -帐户名称和 Azure 存储帐户密钥

- **AzureStorageContainer&lt;字符串&gt;** -在 Azure 中存储备份的存储容器

- **StoragePath&lt;字符串&gt;** -备份将存储在该位置

- **ADFSName&lt;字符串&gt;**  -备份和要还原的联合的名称。 如果不提供该参数，并且没有将使用只有一个联合身份验证服务名称就在那时。 如果有多个联合身份验证服务备份到位置，则系统会提示用户选择一个备份联合身份验证服务。

- **ServiceAccountCredential &lt; pscredential &gt;**  -指定将用于要还原的新 AD FS 服务的服务帐户 

- **GroupServiceAccountIdentifier&lt;字符串&gt;** -用户想要使用新的 AD FS 服务正在还原的 GMSA。 默认情况下，如果没有提供的备份会使用帐户名是否 GMSA，其他系统会提示用户将放在服务帐户

- **DBConnectionString&lt;字符串&gt;** -如果用户想要使用其他数据库还原，则他们应传入的 SQL 连接字符串或类型 WID 作为 WID.

- **强制&lt;bool&gt;**  -跳过该工具可能选择在备份后的提示

- **RestoreDKM &lt;bool&gt;**  -还原到 AD DKM 容器，如果转到新的 AD，则应设置并且 DKM 最初备份。

## <a name="restore-examples"></a>Restore 示例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>从 Azure 存储容器中还原 AD FS 配置，而不 DKM

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>从文件系统还原的 AD FS 配置，而无需 DKM
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>还原到文件系统使用 DKM 的 AD FS 配置 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>还原到 WID 的 AD FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>将 AD FS 配置还原到 SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>还原与指定的 GMSA 的 AD FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>还原与指定的服务帐户凭据在 AD FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密信息
所有备份数据进行加密之前将其推送到云，或将其存储在文件系统。  

使用 AES-256 加密的备份一部分创建的每个文档。 传递到该工具的密码作为通行短语用于生成使用 Rfc2898DeriveBytes 类的新密码。 

RngCryptoServiceProvider 用于生成 AES 和 Rfc2898DeriveBytes 类使用的 salt。 

## <a name="log-files"></a>日志文件
每次执行备份或还原时被创建一个日志文件。 可以在以下位置找到这些：

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 当执行的还原 PostRestore_Instructions 文件可能会创建包含附加身份验证提供程序的概述，属性存储和本地声明提供方信任，若要启动的 AD FS 服务之前手动安装。

## <a name="version-release-history"></a>版本发行历史记录

### <a name="version-10750"></a>版本：1.0.75.0
版本：2018 年 8 月

**修复了问题：**
* 更新备份 ADFS 时使用-BackupDKM 开关。  该工具将确定当前上下文是否有权 DKM 容器。  如果是这样，它将需要域管理员权限或服务帐户凭据。  这允许发生这种情况而无需显式提供凭据，或作为域管理员帐户运行的自动的备份。

### <a name="version-10730"></a>版本：1.0.73.0
版本：2018 年 8 月

**修复了问题：**
* 更新，以便应用程序符合 FIPS 的加密算法
    
    >[!NOTE]
    > 旧备份不会使用由于根据 FIPS 符合性加密算法中进行了更改的新版本
    
* 添加对使用合并复制的 SQL 群集的支持

### <a name="version-10720"></a>版本：1.0.72.0
版本：2018 年 7 月

**修复了问题：**

   - Bug 修复：已修复。MSI 安装程序以支持就地升级 

### <a name="10180"></a>1.0.18.0
版本：2018 年 7 月

**修复了问题：**

   - 修复了 bug： 处理中都有特殊字符的服务帐户密码 (即，&)
   - Bug 修复： 还原会失败，因为 Microsoft.IdentityServer.Servicehost.exe.config 正由另一个进程


### <a name="1000"></a>1.0.0.0
发布日期：2016 年 10 月

初始版本的 AD FS 快速还原工具
