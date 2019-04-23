---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: CA 备份和还原 Windows PowerShell cmdlet
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a4bbeeedfb40e789a799103f9a29a848a2b32324
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877348"
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>CA 备份和还原 Windows PowerShell cmdlet

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012
>
**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
在 Window Server 2012 中引入的 ADCSAdministration Windows PowerShell 模块。  两个新 cmdlet 已添加到此模块中 Window Server 2012 R2 以支持备份和还原 CA。  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**表 SEQ 表\\\*阿拉伯语 17:备份和还原 Windows PowerShell Cmdlet**  
  
**ADCSAdministration Cmdlet:Backup-CARoleService**  
  
|参数-**加粗**参数是必需参数|描述|  
|------------------------------------------------|---------------|  
|**-Path**|-字符串-用于保存备份位置<br />-这是唯一未命名参数<br />-位置参数<br /><br />**示例：**<br /><br />Backup-CARoleService.-Path c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|没有数据库备份 CA 证书<br /><br />**示例：**<br /><br />Backup-CARoleService c:\adcsbackup3 -KeyOnly|  
|-Password|-指定要保护的 CA 证书和私钥的密码<br />-必须是一个安全字符串<br />-不使用-DatabaseOnly 参数有效<br /><br />例如：<br /><br />Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)<br /><br />Backup-caroleservice c:\adcsbackup5-密码 (Convertto-securestring"Pa55w0rd ！" -AsPlainText -Force)|  
|-DatabaseOnly|备份数据库没有 CA 证书<br /><br />Backup-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|1.允许你覆盖指定的位置中预先存在的备份-Path 参数中<br /><br />Backup-CARoleService c:\adcsbackup1 -Force|  
|-Incremental|-执行增量备份<br /><br />Backup-CARoleService c:\adcsbackup7 -Incremental|  
|-KeepLog|1.指示要保留日志文件的命令。 如果未指定此开关，增量的方案中除默认情况下截断日志文件<br /><br />Backup-CARoleService c:\adcsbackup7 -KeepLog|  
  
### <a name="-password-secure-string"></a>-Password <Secure String>  
如果-密码参数，提供的密码必须是一个安全字符串。  使用**Read-host** cmdlet 来启动交互式提示输入安全的密码，或使用**Convertto-securestring** cmdlet 来指定密码中的行。  
  
查看下面的示例  
  
**指定的安全字符串，使用 Read-host 密码参数**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**指定的安全字符串，使用 Convertto-securestring 密码参数**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet:Restore-CARoleService**  
  
|参数-**加粗**参数是必需参数|描述|  
|------------------------------------------------|---------------|  
|**-Path**|-字符串-要还原的备份位置<br />-这是唯一未命名参数<br />-位置参数<br /><br />**示例：**<br /><br />Restore-CARoleService.-Path c:\adcsbackup1 -Force<br /><br />Restore-CARoleService c:\adcsbackup2 -Force|  
|-KeyOnly|-还原没有数据库的 CA 证书<br />-如果执行备份时使用-KeyOnly 选项必须指定<br /><br />**示例：**<br /><br />Restore-CARoleService c:\adcsbackup3 -KeyOnly -Force|  
|-Password|-指定 CA 证书和私钥的密码<br />-必须是一个安全字符串<br /><br />**示例：**<br /><br />Restore-CARoleService c:\adcsbackup4 -Password (read-host -prompt "Password:" -AsSecureString) -Force<br /><br />Restore-caroleservice c:\adcsbackup5-密码 (Convertto-securestring"Pa55w0rd ！" -AsPlainText -Force) -Force|  
|-DatabaseOnly|-还原数据库没有 CA 证书<br /><br />Restore-CARoleService c:\adcsbackup6 -DatabaseOnly|  
|-Force|-允许你覆盖预先存在的密钥<br />-是一个可选参数，但还原时就地，也可能需要<br /><br />Restore-CARoleService c:\adcsbackup1 -Force|  
  
### <a name="issues"></a>问题  
如果 Convertto-securestring 函数失败时使用 Backup-caroleservice 执行非密码受保护的备份使用-密码参数。  
  
![CA 备份和还原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**表 SEQ 表\\\*阿拉伯语 18:常见错误**  
  
|操作|错误|备注|  
|----------|---------|-----------|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-caroleservice:进程无法访问该文件，因为它正由另一个进程。 （HRESULT 中的异常：<br /><br />0x80070020)|停止运行 Restore-caroleservice cmdlet 之前的 Active Directory 证书服务服务|  
|**Restore-CARoleService C:\ADCSBackup**|Restore-caroleservice:目录不为空。 （HRESULT 中的异常：0x80070091)|使用-Force 参数覆盖预先存在的密钥|  
|**Backup-caroleservice C:\ADCSBackup-密码 (Read-host-Prompt"密码:"-AsSecureString)-DatabaseOnly**|Backup-caroleservice:解析参数集不能使用指定命名参数。|参数是-密码仅用于对密码保护私钥，因此无效时不备份这些|  
|**Restore-CARoleService C:\ADCSBack15 -Password (Read-Host -Prompt "Password:" -AsSecureString) -DatabaseOnly**|Restore-caroleservice:解析参数集不能使用指定命名参数。|参数是-密码仅用于对密码保护私钥，因此无效时您不还原|  
|**Restore-CARoleService C:\ADCSBack14 -Password (Read-Host -Prompt "Password:" -AsSecureString)**|Restore-caroleservice:系统找不到指定的文件。 （HRESULT 中的异常：0x80070002)|指定的路径不包含有效的数据库备份。  可能的路径无效或执行备份时使用-KeysOnly 选项？|  
  
## <a name="additional-resources"></a>其他资源  
[Active Directory 证书服务迁移指南](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[备份 CA 数据库和私钥](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[在目标服务器上还原 CA 数据库和配置](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>请尝试此操作：备份你的实验室使用 Windows PowerShell 中的 CA  
  
1.  使用本课程中的命令以备份 CA 数据库和私钥的密码保护。  
  
2.  在这一次还原 CA 拖延。  
  


