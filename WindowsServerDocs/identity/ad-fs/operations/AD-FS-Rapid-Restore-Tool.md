---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: AD FS 快速还原工具
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 77655ab414f83f2c74873b12719f9718c6fb59e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358520"
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速还原工具

## <a name="overview"></a>概述
今天 AD FS 通过设置 AD FS 场实现高度可用。 某些组织希望通过一种方法 AD FS 部署, 从而无需使用多个 AD FS 服务器和网络负载平衡基础结构, 同时仍有一些保证, 如果出现问题, 可以快速还原服务。
新的 AD FS 快速还原工具提供了一种方法来还原 AD FS 的数据, 无需完全备份和还原操作系统或系统状态。 可以使用新工具将 AD FS 配置导出到 Azure 或本地位置。  然后, 可以将导出的数据应用于全新 AD FS 安装, 重新创建或复制 AD FS 环境。 

## <a name="scenarios"></a>方案
可以在以下方案中使用 AD FS 快速还原工具:

1. 问题后快速还原 AD FS 功能
    - 使用工具创建 AD FS 的冷备用安装, 这些安装可以快速部署以代替联机 AD FS 服务器
2. 部署相同的测试环境和生产环境
    - 使用此工具可在测试环境中快速创建生产 AD FS 的精确副本, 或快速将验证的测试配置部署到生产环境

>[!NOTE] 
>如果使用的是 SQL 合并复制或 Alwayson 可用性组, 则不支持快速还原工具。 建议使用基于 SQL 的备份和 SSL 证书的备份作为替代方法。

## <a name="what-is-backed-up"></a>备份的内容
该工具备份以下 AD FS 配置
    
- AD FS 配置数据库 (SQL 或 WID)
- 配置文件 (位于 AD FS 文件夹中)
- 自动生成的令牌签名和解密证书和私钥 (来自 Active Directory DKM 容器)
- SSL 证书和任何外部注册的证书 (令牌签名、令牌解密和服务通信) 以及相应的私钥 (注意: 私钥必须是可导出的, 并且运行脚本的用户必须有权访问这些密钥)
- 安装的自定义身份验证提供程序、属性存储和本地声明提供程序信任的列表。

## <a name="how-to-use-the-tool"></a>如何使用该工具
首先, 将 MSI[下载](https://go.microsoft.com/fwlink/?LinkId=825646)并安装到 AD FS 服务器。  

在 PowerShell 提示符下运行以下命令:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果使用的是 Windows 集成数据库 (WID), 则需要在主 AD FS 服务器上运行此工具。  你可以使用`Get-AdfsSyncProperties` PowerShell cmdlet 来确定你所在的服务器是否是主服务器。

### <a name="system-requirements"></a>系统要求

- 此工具适用于 Windows Server 2012 R2 和更高版本中的 AD FS。 
- 所需的 .NET framework 至少为4.0。 
- 必须在与备份版本相同的 AD FS 服务器上执行还原, 并使用与 AD FS 服务帐户相同的 Active Directory 帐户。

## <a name="create-a-backup"></a>创建备份
若要创建备份, 请使用 Backup-ADFS cmdlet。 此 cmdlet 备份 AD FS 配置、数据库、SSL 证书等。 

用户至少必须是本地管理员才能运行此 cmdlet。 若要备份 Active Directory DKM 容器 (默认 AD FS 配置中需要), 用户必须是域管理员, 需传入 AD FS 服务帐户凭据, 或者有权访问 DKM 容器。  如果你使用的是 gMSA 帐户, 则该用户必须是域管理员或具有容器的权限;不能提供 gMSA 凭据。 

将按照 "adfsBackup_ID_Date" 模式对备份进行命名。 它将包含完成备份的日期和时间。
Cmdlet 采用以下参数:
    
参数集

![AD FS 快速还原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>详细说明

- **BackupDKM** -备份 Active Directory 包含默认配置 (自动生成的令牌签名和解密证书) 中 AD FS 密钥的 DKM 容器。 这将使用 AD 工具 "ldifde" 导出 AD 容器及其所有子树。

- -**StorageType&lt;string&gt;** -用户要使用的存储类型。 "文件系统" 表示用户要将其存储在本地或网络 "Azure" 中, 表示用户要在执行备份时将其存储在 Azure 存储容器中, 请选择备份位置, 无论是文件系统还是形成. 要使用 Azure, 应将 Azure 存储凭据传递给 cmdlet。 存储凭据包含帐户名和密钥。 除此之外, 还必须传入容器名称。 如果该容器不存在，则在备份过程中创建它。 对于要使用的文件系统, 必须指定存储路径。 在该目录中, 将为每个备份创建一个新目录。 创建的每个目录都将包含备份的文件。 

- **EncryptionPassword&lt;字符串&gt;** -用于在存储之前加密所有已备份文件的密码

- **AzureConnectionCredentials&lt;pscredential&gt;** -Azure 存储帐户的帐户名称和密钥

- **New-azurestoragecontainer&lt;string&gt;** -将在 Azure 中存储备份的存储容器

- **存储路径&lt;string&gt;**  -将存储备份的位置

- **ServiceAccountCredential&lt;pscredential&gt;** -指定当前正在运行的 AD FS 服务所使用的服务帐户。 仅当用户想要备份 DKM 而不是域管理员或无权访问容器的内容时, 才需要此参数。 

- **BackupComment &lt;string [] &gt;** -有关将在还原期间显示的备份的信息性字符串，类似于 hyper-v 检查点命名的概念。 默认值为空字符串

 
## <a name="backup-examples"></a>备份示例
下面是使用 AD FS 快速还原工具的备份示例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>使用 DKM 将 AD FS 配置备份到文件系统, 并有权访问 DKM 容器内容 (域管理员或委托)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>将具有 DKM 的 AD FS 配置备份到具有服务帐户凭据的文件系统, 以本地管理员身份运行

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>将不包含 DKM 的 AD FS 配置备份到 Azure 存储容器。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>将不带 DKM 的 AD FS 配置备份到文件系统

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>从备份还原
若要将使用 Backup-ADFS 创建的配置应用到新的 AD FS 安装, 请使用 Restore-ADFS cmdlet。

此 cmdlet 使用 cmdlet `Install-AdfsFarm`创建新的 AD FS 场, 并还原 AD FS 配置、数据库、证书等。如果服务器上尚未安装 AD FS 角色, 则该 cmdlet 将安装它。  该 cmdlet 检查现有备份的还原位置, 并提示用户根据所采用的日期/时间以及用户可能已附加到备份的任何备份注释来选择适当的备份。 如果有多个 AD FS 配置具有不同的联合身份验证服务名称, 则系统会提示用户首先选择适当的 AD FS 配置。
用户必须是本地管理员和域管理员才能运行此 cmdlet。


>[!NOTE] 
>使用 AD FS 快速恢复工具之前, 请先确保服务器已加入域, 然后再还原备份。 

Cmdlet 采用以下参数: 

![AD FS 快速还原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>详细说明

- **StorageType&lt;string&gt;** -用户要使用的存储类型。
 "文件系统" 指示用户要将其存储在本地或网络 "Azure" 中, 表示用户要将其存储在 Azure 存储容器中

- **DecryptionPassword&lt;字符串&gt;** -用于加密所有已备份文件的密码 

- **AzureConnectionCredentials&lt;pscredential&gt;** -Azure 存储帐户的帐户名称和密钥

- **New-azurestoragecontainer&lt;string&gt;** -将在 Azure 中存储备份的存储容器

- **存储路径&lt;string&gt;**  -将存储备份的位置

- **ADFSName&lt; 字符串&gt;** -已备份并将还原的联合的名称。 如果未提供此项并且只有一个联合身份验证服务名称, 则将使用。 如果有多个备份到该位置的联合身份验证服务, 则会提示用户选择一个已备份的联合身份验证服务。

- **ServiceAccountCredential&lt; pscredential&gt;** -指定将用于要还原的新 AD FS 服务的服务帐户 

- **GroupServiceAccountIdentifier&lt;string&gt;** -用户要用于要还原的新 AD FS 服务的 GMSA。 默认情况下, 如果未提供任何一个, 则会在 GMSA 时使用备份的帐户名称, 否则, 系统会提示用户将其置于服务帐户中

- **DBConnectionString&lt;字符串&gt;** -如果用户想要使用不同的数据库进行还原, 则应在 wid 中传递 SQL 连接字符串或为 wid 键入。

- **强制&lt;执行布尔&gt;**  -跳过工具在选择备份后可能会出现的提示

- **RestoreDKM&lt;bool&gt;** -若要转到新的 ad 并且最初备份 dkm, 应设置为 ad 的 dkm 容器。

## <a name="restore-examples"></a>还原示例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>从 Azure 存储容器还原不含 DKM 的 AD FS 配置

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>从文件系统中还原不含 DKM 的 AD FS 配置
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>将 AD FS 配置与 DKM 一起还原到文件系统 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>将 AD FS 配置还原到 WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>将 AD FS 配置还原到 SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>用指定的 GMSA 还原 AD FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>用指定的服务帐户凭据还原 AD FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密信息
在将所有备份数据推送到云之前对其进行加密, 或将其存储在文件系统中。  

作为备份的一部分创建的每个文档都使用 AES-256 进行加密。 传递给工具的密码用作通行短语, 以使用 Rfc2898DeriveBytes 类生成新密码。 

RngCryptoServiceProvider 用于生成 AES 和 Rfc2898DeriveBytes 类使用的 salt。 

## <a name="log-files"></a>日志文件
每次执行备份或还原时, 都会创建一个日志文件。 可在以下位置找到这些内容:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 执行还原时，可能会创建一个 PostRestore_Instructions 文件，其中包含附加身份验证提供程序的概述、属性存储和本地声明提供程序信任，以便在启动 AD FS 服务之前手动安装。

## <a name="version-release-history"></a>版本发行历史记录

### <a name="version-10820"></a>版本1.0.82。0
拆卸2019 年 7 月

**已修复的问题:**
- AD FS 包含 LDAP 转义字符的服务帐户名称的 Bug 修复


### <a name="version-10810"></a>版本：1.0.81.0
拆卸2019 年 4 月

**已修复的问题:**


- 证书备份和还原的 Bug 修复
- 日志文件的附加跟踪信息


### <a name="version-10750"></a>版本：1.0.75.0
拆卸2018 年 8 月

**已修复的问题:**
* 当使用-BackupDKM 开关时, 更新备份-ADFS。  该工具将确定当前上下文是否有权访问 DKM 容器。  如果是这样, 则不需要域管理员权限或服务帐户凭据。  这允许自动备份, 而无需显式提供凭据或作为域管理员帐户运行。

### <a name="version-10730"></a>版本：1.0.73.0
拆卸2018 年 8 月

**已修复的问题:**
* 更新加密算法, 使应用程序符合 FIPS
    
    >[!NOTE]
    > 旧备份不适用于新版本, 因为加密算法的更改是根据 FIPS 相容性的
    
* 添加对使用合并复制的 SQL 群集的支持

### <a name="version-10720"></a>版本：1.0.72.0
拆卸2018 年 7 月

**已修复的问题:**

   - Bug 修复:修复了。用于支持就地升级的 MSI 安装程序 

### <a name="10180"></a>1.0.18.0
拆卸2018 年 7 月

**已修复的问题:**

   - Bug 修复：处理在其中包含特殊字符的服务帐户密码（即 "&"）
   - Bug 修复: 还原失败, 因为另一个进程正在使用 IdentityServer


### <a name="1000"></a>1.0.0.0
取出2016 年 10 月

AD FS 快速还原工具的初始版本
