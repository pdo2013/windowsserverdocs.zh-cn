---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: 使用 Windows PowerShell 的高级 Active Directory 复制和拓扑管理（级别 200）
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d56bc89189c3b17367549aeb076633a6ea0e1007
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442742"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>使用 Windows PowerShell 的高级 Active Directory 复制和拓扑管理（级别 200）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题更详细地解释了新的 AD DS 复制和拓扑管理 cmdlet，并提供了其他示例。 有关的介绍，请参阅[Active Directory 复制和拓扑管理使用 Windows PowerShell 简介&#40;级别 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)。  
  
1.  [简介](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [复制和元数据](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-adreplicationqueueoperation 和 Get-adreplicationuptodatenessvectortable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sync-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [拓扑](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>简介  
Windows Server 2012 使用 25 个新 cmdlet 扩展用于 Windows PowerShell 的 Active Directory 模块，以管理复制和林拓扑。 在此之前，你必须使用泛型 **\*-AdObject**名词或调用.NET 函数。  
  
与所有 Active Directory Windows PowerShell cmdlet 一样，此新功能需要在至少一个域控制器（最好是所有域控制器）上安装 [Active Directory 管理网关服务](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) 。  
  
下表列出了添加到 Active Directory Windows PowerShell 模块的新复制和拓扑 cmdlet。  
  
|||  
|-|-|  
|Cmdlet|说明|  
|Get-ADReplicationAttributeMetadata|返回对象的属性复制元数据|  
|Get-ADReplicationConnection|返回域控制器连接对象详细信息|  
|Get-ADReplicationFailure|返回域对象的最新复制故障|  
|Get-ADReplicationPartnerMetadata|返回域控制器的复制配置|  
|Get-ADReplicationQueueOperation|返回当前复制队列积压工作|  
|Get-ADReplicationSite|返回站点信息|  
|Get-ADReplicationSiteLink|返回站点链接信息|  
|Get-ADReplicationSiteLinkBridge|返回站点链接桥信息|  
|Get-ADReplicationSubnet|返回 AD 子网信息|  
|Get-ADReplicationUpToDatenessVectorTable|返回域控制器的 UTD 向量|  
|Get-ADTrust|返回有关域间和林间信任的信息|  
|New-ADReplicationSite|创建新站点|  
|New-ADReplicationSiteLink|创建新站点链接|  
|New-ADReplicationSiteLinkBridge|创建新站点链接桥|  
|New-ADReplicationSubnet|创建新 AD 子网|  
|Remove-ADReplicationSite|删除站点|  
|Remove-ADReplicationSiteLink|删除站点链接|  
|Remove-ADReplicationSiteLinkBridge|删除站点链接桥|  
|Remove-ADReplicationSubnet|删除 AD 子网|  
|Set-ADReplicationConnection|修改连接|  
|Set-ADReplicationSite|修改站点|  
|Set-ADReplicationSiteLink|修改站点链接|  
|Set-ADReplicationSiteLinkBridge|修改站点链接桥|  
|Set-ADReplicationSubnet|修改 AD 子网|  
|Sync-ADObject|强制复制单个对象|  
  
其中大部分 cmdlet 在 Repadmin.exe 中都具有其基础。 其他 cmdlet（未列出）处理诸如动态访问控制和组托管服务帐户的功能。  
  
要获取所有 Active Directory Windows PowerShell cmdlet 的完整列表，请运行：  
  
```  
Get-command -module ActiveDirectory  
```  
  
要获取所有 Active Directory Windows PowerShell cmdlet 参数的完整列表，请参考帮助。 例如：  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
使用 `Update-Help` cmdlet 下载和安装帮助文件  
  
### <a name="BKMK_Repl"></a>复制和元数据  
Repadmin.exe 验证 Active Directory 复制的运行状况和一致性。 Repadmin.exe 提供简单的数据操作选项（例如，某些参数支持 CSV 输出），但自动化通常需要通过文本文件输出进行分析。 为了提供允许真正控制返回的数据的选项，将首先尝试 Windows PowerShell 的 Active Directory 模块；在此之前，你必须创建脚本或使用第三方工具。  
  
此外，以下 cmdlet 实现 **Target**、**Scope** 和 **EnumerationServer** 的新参数集：  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
**Target** 参数接受以逗号分隔的字符串列表，这些字符串可标识由 **Scope** 参数指定的目标服务器、站点、域或林。 一个星号 (\*) 也是允许，表示指定范围内的所有服务器。 如果没有指定范围，则意味着当前用户的林中的所有服务器。 **Scope** 参数指定搜索的范围。 可接受的值是 **Server**、**Site**、**Domain** 和 **Forest**。 **EnumerationServer** 指定用于枚举在 **Target** 和 **Scope**中指定的域控制器列表的服务器。 它的操作方式与 **Server** 参数相同，并且要求指定的服务器运行 Active Directory Web 服务。  
  
若要引入新的 cmdlet，下面提供了一些用于显示不适用于 repadmin.exe 的功能的示例方案；有了这些图示，可能实现的管理功能便显而易见。 查看有关特定用法要求的 cmdlet 帮助。  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
此 cmdlet 类似于 **repadmin.exe /showobjmeta**。 它允许你返回复制元数据（例如在属性发生更改时）、原始域控制器、版本和 USN 信息以及属性数据。 此 cmdlet 对于审核发生更改的位置和时间非常有用。  
  
与 Repadmin 不同，Windows PowerShell 提供了灵活搜索和输出控制。 例如，可以输出以可读列表形式排列的 Domain Admins 对象的元数据：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
或者，可以在表中将该数据排列为类似于 repadmin 的形式：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
或者，你可以使用筛选器（例如所有组）通过管道传送 **Get-Adobject** cmdlet 来为整个对象类获取元数据，然后再加上一个特定日期。 管道是在多个 cmdlet 之间用于传递数据的一个通道。 若要查看在 2012 年 1 月 13 日以某种方式修改的所有组：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
有关管道的更多 Windows PowerShell 操作的详细信息，请参阅 [Windows PowerShell 中的管道系统和管道](https://technet.microsoft.com/library/ee176927.aspx)。  
  
或者，若要了解每个包含 Tony Wang 这一成员的组，以及该组的最后修改时间：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
或者，若要基于人为高版本，在域中查找所有使用系统状态备份授权还原的对象：  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
或者，将所有用户元数据发送到 CSV 文件，以供以后在 Microsoft Excel 中进行检查：  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
此 cmdlet 返回有关域控制器复制的配置和状态信息，从而允许你监视、盘存或解决问题。 与 Repadmin.exe 不同，使用 Windows PowerShell 意味着你仅能以所需格式看到对你而言非常重要的数据。  
  
例如，单个域控制器的可读复制状态：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
或者，域控制器最后一次复制入站及其合作伙伴的时间（采用表格式）：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
或者，联系林中的所有域控制器，并显示上次尝试复制失败（出于任何原因）的任何域控制器：  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
此 cmdlet 可用于返回有关复制中最近出现的错误的信息。 它类似于 **Repadmin.exe /showreplsum**，但同样由于 Windows PowerShell，它可以更好地进行控制。  
  
例如，可以返回域控制器最近发生的故障以及联系失败的合作伙伴：  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
或者，返回特定 AD 逻辑站点中的所有服务器的对应表视图，已对该视图排序以便进行查看，它仅包含最关键的数据：  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-adreplicationqueueoperation 和 Get-adreplicationuptodatenessvectortable  
这两个 cmdlet 将返回域控制器“更新程度”的其他方面，其中包括挂起的复制和版本矢量信息。  
  
### <a name="BKMK_Sync"></a>Sync-ADObject  
此 cmdlet 类似于运行 **Repadmin.exe /replsingleobject**。 当你进行需要带外复制的更改（特别是修复某个问题）时，它将非常有用。  
  
例如，如果某人删除了 CEO 的用户帐户，然后使用 Active Directory 回收站还原它，你可能希望将它立即复制到所有域控制器。 你可能还希望在不强制复制所做的所有其他对象更改的情况下执行此操作；毕竟这是你制定复制计划的原因 - 避免重载 WAN 链接。  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>拓扑  
虽然 Repadmin.exe 能够很好地返回有关复制拓扑（例如站点、站点链接、站点链接桥和连接）的信息，但它没有可进行更改的完整参数集。 事实上，从来没有任何可编写脚本的内部 Windows 实用工具可专供管理员创建和修改 AD DS 拓扑。 由于 Active Directory 在数以百万计的客户环境中日益成熟，因此批量修改 Active Directory 逻辑信息的需求也日益凸显。  
  
例如，在快速扩张新的分支机构以及合并其他机构之后，你可能需要基于物理位置、网络更改和新容量要求来更改上百个站点。 可自动进行更改，而不是使用 Dssites.msc 和 Adsiedit.msc。 当你使用由网络和设施团队提供的数据电子表格开始操作时，这一点尤为引人注目。  
  
**Get Adreplication\\** * cmdlet 返回有关复制拓扑的信息，并可用于为通过管道**集 Adreplication\\** * 中大容量的 cmdlet。 **获取**cmdlet 不会更改数据，它们仅显示数据或要创建 Windows PowerShell 会话对象，可以通过管道传输到**集 Adreplication\\** * cmdlet。 **New** 和 **Remove** cmdlet 用于创建或删除 Active Directory 拓扑对象。  
  
例如，你可以使用 CSV 文件创建新站点：  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
或者，使用自定义的复制时间间隔和站点成本在两个现有站点之间创建新站点链接：  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
或者，在林中找到每个站点，并将其 **Options** 属性替换为该标志以启用站点间更改通知，从而通过压缩以最大速度进行复制：  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> 也可以设置 **-bor 5** 以在这些站点链接上禁用压缩。  
  
或者，查找缺少子网分配的所有站点，以便协调该列表与这些位置的实际子网：  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![使用 powershell 的高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>请参阅  
[Active Directory 复制和拓扑管理使用 Windows PowerShell 简介&#40;级别 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

