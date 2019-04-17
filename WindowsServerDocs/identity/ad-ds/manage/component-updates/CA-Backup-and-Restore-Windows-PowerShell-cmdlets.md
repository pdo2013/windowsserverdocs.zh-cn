---
ms.assetid: 7e195f5b-b194-40f3-a26d-5cf4ade5fc4d
title: "加拿大备份和还原 Windows PowerShell cmdlet"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aba86ed080cc0b4043805531f0a2138b1b1d3cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ca-backup-and-restore-windows-powershell-cmdlets"></a>加拿大备份和还原 Windows PowerShell cmdlet

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012
>
**作者**：与 Windows 组索旋转器高级支持升级工程师  
  
> [!NOTE]  
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。  
  
## <a name="overview"></a>概述  
在窗口 Server 2012 引入了 ADCSAdministration Windows PowerShell 模块。  两个新 cmdlet 已添加到此模块窗口 Server 2012 R2 备份和还原 CA 的支持中。  
  
-   Backup-CARoleService  
  
-   Restore-CARoleService  
  
## <a name="backup-caroleservice"></a>Backup-CARoleService  
**表用 \\\ * 阿拉伯语 17：备份和还原 Windows PowerShell Cmdlet**  
  
**ADCSAdministration Cmdlet: Backup-CARoleService**  
  
|参数-**加粗**参数均是必需|描述|  
|------------------------------------------------|---------------|  
|**-路径**|-字符串-保存备份的位置<br />-这是唯一未命名的参数<br />-位置参数<br /><br />**示例：**<br /><br />Backup-CARoleService。-路径 c:\adcsbackup1<br /><br />Backup-CARoleService c:\adcsbackup2|  
|-KeyOnly|备份 CA 证书没有数据库<br /><br />**示例：**<br /><br />Backup-CARoleService c:\adcsbackup3-KeyOnly|  
|密码|-指定的密码，以保护 CA 证书和专用键<br />-必须安全字符串<br />-与-DatabaseOnly 参数不有效<br /><br />示例：<br /><br />Backup-CARoleService c:\adcsbackup4 的密码 (Read-Host 的提示"密码:"-AsSecureString)<br /><br />Backup-CARoleService c:\adcsbackup5 的密码 (ConvertTo-SecureString"Pa55w0rd！" -AsPlainText-强制）|  
|-DatabaseOnly|备份不 CA 证书数据库<br /><br />Backup-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-强制|1.允许你覆盖 preexists 中指定的位置的备份中的路径参数<br /><br />Backup-CARoleService c:\adcsbackup1-强制|  
|-增量|-执行增量备份<br /><br />Backup-CARoleService c:\adcsbackup7-增量|  
|-KeepLog|1.指示命令以使日志文件。 如果不指定开关，日志文件将被截断默认除在增量方案<br /><br />Backup-CARoleService c:\adcsbackup7-KeepLog|  
  
### <a name="-password-secure-string"></a>密码 <Secure String>  
如果-密码参数，提供的密码必须是安全的字符串。  使用**读取主机**cmdlet 启动交互式提示符的安全密码项，或使用**ConvertTo SecureString** cmdlet 指定中行密码。  
  
查看以下示例  
  
**指定为密码参数使用 Read-Host 安全字符串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup4 -Password (Read-Host -prompt "Password:" -AsSecureString)  
```  
  
**指定为密码参数使用 ConvertTo-SecureString 安全字符串**  
  
```powershell  
Backup-CARoleService c:\adcsbackup5 -Password (ConvertTo-SecureString "Pa55w0rd!" -AsPlainText -Force)  
```  
  
## <a name="restore-caroleservice"></a>Restore-CARoleService  
**ADCSAdministration Cmdlet: Restore-CARoleService**  
  
|参数-**加粗**参数均是必需|描述|  
|------------------------------------------------|---------------|  
|**-路径**|-字符串-位置从备份还原<br />-这是唯一未命名的参数<br />-位置参数<br /><br />**示例：**<br /><br />Restore-CARoleService。-路径 c:\adcsbackup1-强制<br /><br />Restore-CARoleService c:\adcsbackup2-强制|  
|-KeyOnly|-还原没有数据库 CA 证书<br />-如果在创建备份的-KeyOnly 选项必须指定<br /><br />**示例：**<br /><br />Restore-CARoleService 的 c:\adcsbackup3 KeyOnly-强制|  
|密码|-指定的密码，CA 证书和专用键<br />-必须安全字符串<br /><br />**示例：**<br /><br />Restore-CARoleService c:\adcsbackup4 的密码 (阅读主机的提示"密码:"-AsSecureString)-强制<br /><br />Restore-CARoleService c:\adcsbackup5 的密码 (ConvertTo-SecureString"Pa55w0rd！" -AsPlainText-强制）-强制|  
|-DatabaseOnly|-还原不 CA 证书数据库<br /><br />Restore-CARoleService c:\adcsbackup6-DatabaseOnly|  
|-强制|-允许你覆盖现有键<br />-可选参数但当还原就地，也可能需要<br /><br />Restore-CARoleService c:\adcsbackup1-强制|  
  
### <a name="issues"></a>问题  
如果同时使用 Backup-CARoleService 失败 ConvertTo-SecureString 函数拍摄非密码受保护的备份与-密码参数。  
  
![加拿大备份和还原](media/CA-Backup-and-Restore-Windows-PowerShell-cmdlets/GTR_ADDS_BackupCARole.gif)  
  
**表用 \\\ * 阿拉伯语 18：常见错误**  
  
|操作|错误|评论|  
|----------|---------|-----------|  
|**Restore-CARoleServiceC:\ADCSBackup**|Restore-CARoleService: 进程无法访问文件，因为正由另一个进程。 (来自 HRESULT 异常：<br /><br />0x80070020)|停止运行 Restore-CARoleService cmdlet 之前 Active Directory 证书服务服务|  
|**Restore-CARoleServiceC:\ADCSBackup**|Restore-CARoleService: 目录不为空。 (来自 HRESULT 异常：0x80070091)|使用-强制参数覆盖现有键|  
|**Backup-CARoleServiceC:\ADCSBackup 的密码 (Read-Host 的提示"密码:"-AsSecureString)-DatabaseOnly**|Backup-CARoleService：无法使用指定名为参数解决参数集。|参数是-密码仅使用密码保护专用键，因此无效时不备份它们|  
|**Restore-CARoleServiceC:\ADCSBack15 的密码 (Read-Host 的提示"密码:"-AsSecureString)-DatabaseOnly**|Restore-CARoleService：无法使用指定名为参数解决参数集。|参数是-密码仅使用密码保护专用键，因此无效时你无法还原|  
|**Restore-CARoleServiceC:\ADCSBack14 的密码 (Read-Host 的提示"密码:"-AsSecureString)**|Restore-CARoleService：系统找不到指定文件。 (来自 HRESULT 异常：0x80070002)|指定的路径不包含有效的数据备份。  或许路径无效或备份的-KeysOnly 选项？|  
  
## <a name="additional-resources"></a>更多资源  
[Active Directory 证书服务迁移指南](https://technet.microsoft.com/library/ee126170(v=ws.10).aspx)  
  
[备份的 CA 数据库和专用键](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_BackUpDB)  
  
[还原 CA 数据库和配置目标服务器上](https://technet.microsoft.com/library/ee126140(v=ws.10).aspx#BKMK_RestoreCA)  
  
## <a name="try-this-backup-the-ca-in-your-lab-using-windows-powershell"></a>请尝试： 备份在实验中，使用 Windows PowerShell CA  
  
1.  在本课程使用命令备份 CA 数据库和使用密码保护的专用键。  
  
2.  按 CA 还原，这次。  
  


