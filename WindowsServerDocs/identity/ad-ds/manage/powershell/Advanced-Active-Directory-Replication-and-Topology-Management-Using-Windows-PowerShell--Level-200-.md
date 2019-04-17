---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: "高级 Active Directory 复制和使用 Windows PowerShell (级别 200) 拓扑管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>高级 Active Directory 复制和使用 Windows PowerShell (级别 200) 拓扑管理

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍的新广告 DS 复制和拓扑管理 cmdlet 更详细地介绍，并提供其他示例。 有关说明，请参阅[Active Directory 复制和拓扑管理使用 Windows PowerShell & #40; 简介级别 100 & #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [简介](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [复制和元数据](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [获取 ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [获取 ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [获取 ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [获取 ADReplicationQueueOperation 和获取 ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [同步 ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [拓扑](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>简介  
Windows Server 2012 扩展 Active Directory 的 Windows PowerShell 模块 25 个新 cmdlet 管理复制和森林拓扑。 在此之前，你被迫使用通用**\*-AdObject**名词或.NET 调用的函数。  
  
所有 Active Directory 的 Windows PowerShell cmdlet，诸如此新功能需要安装[Active Directory 管理网关服务](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852)至少一个和域控制器上 （最好，所有域控制器）。  
  
下表列出了新的复制和拓扑 cmdlet 添加到 Active Directory 的 Windows PowerShell 模块。  
  
|||  
|-|-|  
|Cmdlet|说明|  
|获取 ADReplicationAttributeMetadata|退货特性对象复制元数据|  
|获取 ADReplicationConnection|返回域控制器连接对象详细信息|  
|获取 ADReplicationFailure|返回最复制最近故障域控制器|  
|获取 ADReplicationPartnerMetadata|返回复制配置的域控制器|  
|获取 ADReplicationQueueOperation|返回当前复制队列积压工作|  
|获取 ADReplicationSite|返回站点的信息|  
|获取 ADReplicationSiteLink|返回站点链接信息|  
|获取 ADReplicationSiteLinkBridge|返回站点链接大桥信息|  
|获取 ADReplicationSubnet|返回广告子网信息|  
|获取 ADReplicationUpToDatenessVectorTable|返回 UTD 矢量域控制器|  
|获取 ADTrust|返回域间或林间信任的信息|  
|新 ADReplicationSite|创建新的网站|  
|新 ADReplicationSiteLink|创建一个新站点链接|  
|新 ADReplicationSiteLinkBridge|创建新的站点链接大桥|  
|新 ADReplicationSubnet|创建新的广告子网|  
|删除 ADReplicationSite|删除站点|  
|删除 ADReplicationSiteLink|删除站点链接|  
|删除 ADReplicationSiteLinkBridge|删除站点链接大桥|  
|删除 ADReplicationSubnet|删除广告子网|  
|设置 ADReplicationConnection|修改的连接|  
|设置 ADReplicationSite|修改站点|  
|设置 ADReplicationSiteLink|修改站点链接|  
|设置 ADReplicationSiteLinkBridge|修改站点链接大桥|  
|设置 ADReplicationSubnet|修改广告子网|  
|同步 ADObject|强制复制的单个对象|  
  
大部分这些 cmdlet Repadmin.exe 提供其基础。 （未列出） 其他 cmdlet 处理动态访问控制和组托管服务帐户等功能。  
  
对于所有 Active Directory 的 Windows PowerShell cmdlet 的完整列表，运行：  
  
```  
Get-command -module ActiveDirectory  
```  
  
有关所有 Active Directory 的 Windows PowerShell cmdlet 参数的完整列表，参考帮助。 例如：  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
使用`Update-Help`cmdlet 下载和安装的帮助文件  
  
### <a name="BKMK_Repl"></a>复制和元数据  
Repadmin.exe 验证的 Active Directory 复制一致性和正常运行。 Repadmin.exe 提供简单数据操作选项-某些参数支持 CSV 输出，例如，但自动化通常需要通过文本文件输出分析。 Active Directory 的 Windows PowerShell 模块是第一次提供一个选项，可使真正返回的数据控制尝试在此之前，你必须要创建脚本或使用第三方工具。  
  
此外，以下 cmdlet 实现了新的参数一**目标**，**范围**，并**EnumerationServer**:  
  
-   **获取 ADReplicationFailure**  
  
-   **获取 ADReplicationPartnerMetadata**  
  
-   **获取 ADReplicationUpToDatenessVectorTable**  
  
**目标**参数接受逗号分隔识别目标服务器、 网站、 域中或由指定森林的字符串的列表**范围**参数。 星号 (\ *)，还允许和意味着指定范围内的所有服务器。 如果没有指定范围，这意味着当前用户森林中的所有服务器。 **范围**参数指定搜索的纬度。 可接受值为**服务器**，**站点**，**域**，并**森林**。 **EnumerationServer**指定枚举列表中指定的域控制器服务器**目标**和**范围**。 它的工作相同**服务器**参数网络且需要指定运行 Active Directory Web 服务的服务器。  
  
引入的新 cmdlet，下面是一些示例方案显示功能不可能给 repadmin.exe;有了这些说明，管理的可能性变得更加明显。 查看有关特定使用量要求 cmdlet 帮助。  
  
### <a name="BKMK_ReplAttrMD"></a>获取 ADReplicationAttributeMetadata  
此 cmdlet 是类似于**repadmin.exe /showobjmeta**。 它可以让你恢复复制元数据，如特性发生更改时，原始的域控制器、 版本和 USN 信息，以及属性数据。 此 cmdlet 是有用的审核位置和发生了更改。  
  
与 Repadmin，不同的 Windows PowerShell 为灵活搜索和输出控制。 例如，你可以输出域管理员对象，为较高的可读性列表排序的元数据：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
或者，你可以排列看上去就像 repadmin，表中的数据：  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
或者，你可以获取元数据的整个类物体，通过管道**获取 Adobject**筛选，如所有组-cmdlet 然后再加上特定的日期。 管道是使用多个 cmdlet 来通过数据之间的频道。 若要查看所有组在 2012 年 1 月 13 日修改某些的方式：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
有关更多的 Windows PowerShell 操作管道的详细信息，请参阅[管道，并且在 Windows PowerShell 管道](https://technet.microsoft.com/library/ee176927.aspx)。  
  
或者，若要了解每组具有 Tony 王安作为成员，上次修改组：  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
或者，若要查找的所有对象授权使用还原系统状态备份在域中，根据他们较高的版本：  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
或者，将所有用户的元数据都发送到 CSV 以供以后在 Microsoft Excel 中的检查文件：  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>获取 ADReplicationPartnerMetadata  
此 cmdlet 返回配置和的域控制器，这允许你监视、 库存，或解决复制状态的信息。 与不同 Repadmin.exe，使用 Windows PowerShell 意味着你将看到仅对您很重要，你希望的格式的数据。  
  
例如，单个域控制器较高的可读性复制状态：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
或者，归巢的上一次域控制器复制和合作伙伴协作，在表格中的格式化：  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
或者，联系森林中的所有域控制器同时显示任何其尝试的最后一个复制失败因任何原因：  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>获取 ADReplicationFailure  
退货有关的信息复制中最近的错误，可以使用此 cmdlet。 相当于**Repadmin.exe /showreplsum**，但再次，其中许多更多控制 Windows PowerShell 感谢。  
  
例如，你可以返回和合作伙伴他失败联系，域控制器的最新的失败情况：  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
或者，在某个特定广告逻辑网站，以便更轻松查看和包含至关重要的数据有序返回表视图中的所有服务器：  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>获取 ADReplicationQueueOperation 和获取 ADReplicationUpToDatenessVectorTable  
这两个这些 cmdlet 返回进一步方面的域控制器"最多 dateness"，其中包括待处理的复制和版本矢量信息。  
  
### <a name="BKMK_Sync"></a>同步 ADObject  
此 cmdlet 是类似于运行**Repadmin.exe /replsingleobject**。 当您需要退出 band 复制，尤其是，若要解决问题的更改时，它会非常有用。  
  
例如，如果某人删除 CEO 的用户帐户，然后使用 Active Directory 回收站还原它时，你可能需要它立即复制到所有域控制器。 你可能还想要执行此操作不强制所有其他对象所做的更改; 复制毕竟，这是已复制日程安排-以避免重载 wan 的原因。  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>拓扑  
尽管 Repadmin.exe 精通退回复制拓扑等站点、 网站的链接、 站点链接桥和连接有关的信息，不具有一组全面的参数进行更改。 事实上，有从未如此脚本控制收件箱 Windows 实用程序管理员以创建和修改广告 DS 拓扑专门设计。 随着 Active Directory 具有成熟数以百万计的客户环境中，需要批量修改 Active Directory 的逻辑信息显而易见。  
  
例如，新分支机构，加上的其他应用，合并了快速扩展后可能会有 100 网站更改，以使根据上的物理位置，网络更改或新容量要求。 而不是通过 Dssites.msc Adsiedit.msc 进行更改，你可以自动进行。 当你使用的数据由你的网络和功能团队的电子表格启动时，这是尤其是引人注目。  
  
**获取-Adreplication\ *** cmdlet 返回复制拓扑有关的信息，并可用于为管道**设置-Adreplication\ *** cmdlet 批量。 **获取**cmdlet 不更改的数据，它们仅显示的数据或创建 Windows PowerShell 会话对象，可以向管道**设置-Adreplication\ *** cmdlet。 **新建**和**删除**cmdlet 可用于创建或删除 Active Directory 拓扑对象。  
  
例如，你可以创建新站点使用 CSV 文件：  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
或者，创建新的自定义复制的时间间隔和站点成本较两个现有的站点之间的站点链接：  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
或者，查找林中每站点并替换它们**选项**特性与标记，可启用站点间更改通知，以便在最高速度，使用压缩复制：  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> 设置**为-5**禁用这些网站上的压缩。  
  
或者，找到所有站点缺少子网分配，以便协调具有实际子网这些位置的列表：  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![与 powershell 高级的管理](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>请参阅  
[Active Directory 复制和使用 Windows PowerShell 和 #40; 拓扑管理简介级别 100 & #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

