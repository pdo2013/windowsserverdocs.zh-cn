---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: "广告 FS 快速恢复工具"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd1cc8dab07288ba73c507bc551f089bb79502bc
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2018
---
# <a name="ad-fs-rapid-restore-tool"></a>广告 FS 快速恢复工具

>适用于：Windows Server 2016，Windows Server 2012 R2

## <a name="overview"></a>概述
今天广告 FS 可高度通过广告 FS 场设置。 某些组织想要的方法有一台服务器广告 FS 部署，无需多个广告 FS 服务器和网络负载平衡时仍有一些基础结构，服务的保障可以快速恢复时问题。
新的广告 FS 快速恢复工具提供了还原广告 FS 数据，而无需完全备份和还原操作系统或系统状态的一种方法。 你可以使用新的工具广告 FS 配置导出到 Azure 或本地位置。  然后你可以对全新广告 FS 安装、 应用导出的数据重新创建或复制广告 FS 环境。 

## <a name="scenarios"></a>方案
在以下情况中，可以使用该广告 FS 快速恢复工具：

1. 快速后问题还原广告 FS 功能
    - 使用该工具创建要快速部署来替代联机广告 FS 服务器的广告 FS 冷待机安装
2. 部署相同测试和生产环境
    - 若要快速测试环境，创建生产广告 FS 准确复制或快速部署到生产验证的测试配置使用该工具

## <a name="what-is-backed-up"></a>什么被备份
该工具备份以下广告 FS 配置
    
- 广告 FS 配置数据库 （SQL 或 WID）
- 配置文件 （位于广告 FS 文件夹中）
- 自动生成令牌签名和解密证书和专用键 （从 Active Directory DKM 容器）
- SSL 证书以及任何外部登记证书 （令牌签名、 令牌解密和服务通信） 和相应专用键 (请注意： 必须导出专用键和运行脚本用户必须权限来访问它们)
- 自定义身份验证的提供商、 特性存储和本地索赔提供商列表信任安装。

## <a name="how-to-use-the-tool"></a>如何使用工具
首先，[下载](https://go.microsoft.com/fwlink/?LinkId=825646)并安装到你的广告 FS 服务器 MSI。  

>[!NOTE]
>广告 FS 快速恢复工具 fips 不兼容。

从 PowerShell 提示符运行以下命令：

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果你使用的 Windows 集成数据库 (WID)，该工具需要广告 FS 主服务器上运行。  你可以使用`Get-SyncProperties`PowerShell cmdlet 要确定是否服务器您正在为主服务器。

### <a name="system-requirements"></a>系统要求

- 此工具适用于广告 FS 在 Windows Server 2012 R2 及更高版本。 
- 所需的.NET framework 为至少 4.0。 
- 还原必须完成备份版本相同的广告 FS 服务器上，与广告 FS 服务帐户使用同一 Active Directory 帐户。

## <a name="create-a-backup"></a>创建的备份
若要创建的备份，请使用备份 ADFS cmdlet。 此 cmdlet 备份广告 FS 配置、 数据库、 SSL 证书等。 

用户必须至少为运行此 cmdlet 的本地管理员。 若要备份 Active Directory DKM 容器 （必需的默认广告 FS 配置中），在用户必须域管理员以及，或需要通过在广告 FS 服务帐户凭据。

备份将命名根据模式"adfsBackup_ID_Date 时间"。 它会包含版本号、 日期和时间备份所做的。
Cmdlet 参数如下：
    
参数设置

![广告 FS 快速恢复工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>详细的说明

- **BackupDKM** -备份 Active Directory DKM 容器包含广告 FS 键在默认配置 （自动生成令牌签名和解密证书）。 这只会消耗广告工具 ldifde' 导出广告容器和所有子树。

- -**StorageType&lt;字符串&gt;** -存储用户想要使用的类型。 "文件"表示用户想要将其存储在本地文件夹或"Azure"指示用户想要将其存储在 Azure 存储容器用户执行备份时，他们可以选择备份的位置，文件系统网络或在云中。 若要使用的 azure，应将 Azure 存储凭据传递到 cmdlet。 存储凭据包含的帐户名称和键。 除此之外，容器名称还必须传递中。 如果不存在容器，创建备份时。 选择要使用的文件系统，必须提供的存储路径。 在该目录，将为每个备份创建一个新的目录。 每个目录创建包含备份的文件。 

- **EncryptionPassword&lt;字符串&gt;** -要用于之前将其存储加密备份的所有文件的密码

- **AzureConnectionCredentials &lt;pscredential&gt; ** -的帐户名称和 Azure 存储帐户键

- **AzureStorageContainer&lt;字符串&gt;** -存储容器备份 Azure 中保存的位置

- **StoragePath&lt;字符串&gt;** -备份将存储在该位置

- **ServiceAccountCredential &lt;pscredential&gt; ** -指定用于广告 FS 服务当前运行的服务帐户。 此参数，才需要用户想要备份 DKM，它不是域管理员。

- **BackupComment&lt;字符串 [&gt; ** -的备份中还原类似于 HYPER-V 检查点命名的概念期间将显示有关以信息性字符串。 默认值为空字符串

 
## <a name="backup-examples"></a>备份示例
下面是有关使用该广告 FS 快速恢复工具备份示例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-while-running-as-the-domain-admin"></a>备份广告 FS 配置 DKM，文件系统，时域管理员以运行

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>具有 DKM，文件系统与服务帐户凭据，以本地管理员身份运行备份广告 FS 配置

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>备份到 Azure 存储容器 DKM 无广告 FS 配置。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>备份文件系统 DKM 无广告 FS 配置

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>从备份中还原
若要将应用到新的广告 FS 安装使用备份 ADFS 创建一个配置，请使用还原 ADFS cmdlet。

此 cmdlet 创建新的广告 FS 场使用 cmdlet`Install-AdfsFarm`并还原广告 FS 配置、 数据库、 证书，等等。如果在服务器上未安装广告 FS 角色，cmdlet 会进行安装。  Cmdlet 检查现有备份还原的位置，并提示用户选择相应的备份根据拍摄日期/时间和用户可能会有附加备份到的任何备份注释。 如果有多个广告 FS 配置使用不同的联合身份验证服务的名称，然后被提示用户首先选择相应的广告 FS 配置。
用户必须本地和域运行此 cmdlet 的管理员。


>[!NOTE] 
>使用之前广告 FS 快速恢复工具，确保服务器已加入域前还原备份。 

Cmdlet 参数如下： 

![广告 FS 快速恢复工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>详细的说明

- **StorageType&lt;字符串&gt;** -存储用户想要使用的类型。
 "文件"表示用户想要将其存储在本地文件夹或在网络"Azure"指示用户想要将其存储在 Azure 存储容器

- **DecryptionPassword&lt;字符串&gt;** -的密码，可以被用来加密备份的所有文件 

- **AzureConnectionCredentials &lt;pscredential&gt; ** -的帐户名称和 Azure 存储帐户键

- **AzureStorageContainer&lt;字符串&gt;** -存储容器备份 Azure 中保存的位置

- **StoragePath&lt;字符串&gt;** -备份将存储在该位置

- **ADFSName&lt;字符串&gt; ** -联盟备份并要还原的名称。 如果不提供该并且只有一个联合身份验证服务的名称，然后将会使用。 如果有多个联合身份验证服务备份到的位置，然后提示用户可选择其中一个备份联合身份验证服务。

- **ServiceAccountCredential &lt; pscredential &gt; ** -指定的服务的帐户，将使用新广告 FS 服务还原 

- **GroupServiceAccountIdentifier&lt;字符串&gt;** -用户想要使用新广告 FS 服务还原的 GMSA。 默认情况下，如果不提供备份向上帐户名使用的 GMSA，其他提示用户置于服务帐户时

- **DBConnectionString&lt;字符串&gt;** -如果用户想要使用不同的数据库进行还原，然后他们应传入 SQL 连接字符串或类型 WID WID.为

- **强制&lt;布尔&gt;** -跳过选择备份后，可能必须工具提示

- **RestoreDKM&lt;布尔&gt;** -还原到 AD DKM 容器、 如果转到新的广告应设置并 DKM 最初备份。

## <a name="restore-examples"></a>还原示例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>从 Azure 存储容器还原 DKM 无广告 FS 配置

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>从系统文件还原 DKM 无广告 FS 配置
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>还原文件系统 DKM 与广告 FS 配置 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>还原到 WID 广告 FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>还原到 SQL 广告 FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>将恢复指定 GMSA 广告 FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>恢复与指定的服务帐户凭据设置广告 FS 配置

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密信息
之前将其推到云或将其存储文件系统，已加密备份的所有数据。  

使用 aes-256 加密每个作为备份创建的文档。 作为 pass 短语使用传递到工具密码生成使用 Rfc2898DeriveBytes 类新密码。 

编程方法用于生成 AES 和 Rfc2898DeriveBytes 类使用盐。 

## <a name="log-files"></a>日志文件
每次执行备份或恢复时创建一个日志文件。 可以在这些找到在以下位置：

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 当执行 PostRestore_Instructions 文件可能包含的额外的身份验证提供概述创建的还原，属性存储，并且本地索赔提供商信任启动广告 FS 服务之前手动安装。
