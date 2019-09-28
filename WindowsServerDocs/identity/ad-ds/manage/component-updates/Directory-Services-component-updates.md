---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: 目录服务组件更新
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d79f31572bc30d0f4fa3af45671c58b799e40f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390025"
---
# <a name="directory-services-component-updates"></a>目录服务组件更新

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**：Justin Turner，具有 Windows 组的高级支持升级工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
本课介绍了 Windows Server 2012 R2 中的目录服务组件更新。  
  
## <a name="what-you-will-learn"></a>你将学习的内容  
说明以下新的目录服务组件更新：  
  
-   说明以下新的目录服务组件更新：  
  
    -   [域和林功能级别](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [弃用 NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [LDAP 查询优化程序更改](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [1644 事件改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Active Directory 复制吞吐量改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>域和林功能级别  
  
### <a name="overview"></a>概述  
本部分简要介绍域和林功能级别的更改。  
  
### <a name="new-dfl-and-ffl"></a>New DFL 和 FFL  
在此版本中，有新的域和林功能级别：  
  
-   林功能级别：Windows Server 2012 R2  
  
-   域功能级别：Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 域功能级别支持以下各项：  
  
1.  *受保护用户*的 DC 端保护  
  
    对 Windows Server 2012 R2 域进行身份验证的*受保护用户*将无法**再**执行以下操作：  
  
    -   通过 NTLM 身份验证进行身份验证  
  
    -   在 Kerberos 预身份验证中使用 DES 或 RC4 密码套件  
  
    -   通过不受约束或约束委派进行委派  
  
    -   续订超过初始4小时生存期的用户票证（Tgt）  
  
2.  身份验证策略  
  
    可应用于 Windows Server 2012 R2 域中的帐户的新的基于林的 Active Directory 策略，以控制帐户可以登录的主机，并将身份验证的访问控制条件应用到作为帐户运行的服务  
  
3.  身份验证策略接收器  
  
    新的基于林的 Active Directory 对象，它可以在用户、托管服务和计算机帐户之间创建关系，以用于对身份验证策略的帐户进行分类或用于身份验证隔离。  
  
有关详细信息，请参阅如何配置受保护的帐户。  
  
除了上述功能，Windows Server 2012 R2 域功能级别还确保域中的任何域控制器都运行 Windows Server 2012 R2。  
Windows Server 2012 R2 林功能级别不提供任何新功能，但可确保在林中创建的任何新域都自动在 Windows Server 2012 R2 域功能级别运行。  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>在创建新域时强制实施的最小 DFL  
Windows Server 2008 DFL 是创建新域时支持的最低功能级别。  
  
> [!NOTE]  
> 通过删除使用低于 Windows Server 2008 的域功能级别安装新域的功能，通过服务器管理器或通过 Windows PowerShell 来实现 FRS 的弃用。  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低林和域功能级别  
默认情况下，林和域功能级别设置为 Windows Server 2012 R2，在新域和新林创建时设置为，但可以使用 Windows PowerShell 降低。  
  
若要使用 Windows PowerShell 提升或降低林功能级别，请使用**set-adforestmode** cmdlet。  
  
**将 contoso.com FFL 设置为 Windows Server 2008 模式：**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
若要使用 Windows PowerShell 提升或降低域功能级别，请使用 Set-addomainmode cmdlet。  
  
**将 contoso.com DFL 设置为 Windows Server 2008 模式：**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
将运行 Windows Server 2012 R2 的 DC 作为附加副本升级到运行 2003 DFL 的现有域。  
  
在现有林中创建新域  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP.LOG  
此版本中没有新的林或域操作。  
  
这些 .ldf 文件包含**设备注册服务**的架构更改。  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**工作文件夹：**  
  
1.  Sch66  
  
**MSODS**  
  
1.  Sch60  
  
**身份验证策略和接收器**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>弃用了 NTFRS  
  
### <a name="overview"></a>概述  
FRS 在 Windows Server 2012 R2 中已弃用。  FRS 的弃用是通过强制实施 Windows Server 2008 的最小域功能级别（DFL）来实现的。  仅当使用服务器管理器或 Windows PowerShell 创建新域时，才会执行此强制。  
  
将-DomainMode 参数与 Install-addsforest 或 Install-addsdomain cmdlet 结合使用，以指定域功能级别。  此参数的支持值可以是有效的整数或相应的枚举字符串值。 例如，若要将域模式级别设置为 Windows Server 2008 R2，可以指定值4或 "Win2008R2"。  从服务器 2012 R2 中执行这些 cmdlet 时，有效值包括 Windows Server 2008 （3，Win2008） Windows Server 2008 R2 （4，Win2008R2） Windows Server 2012 （5，Win2012）和 Windows Server 2012 R2 （6，Win2012R2）。 域功能级别不能低于林控制级别，但可以高于林控制级别。  由于此版本中已弃用 FRS，因此从 Windows Server 2012 R2 执行时，Windows Server 2003 （2，Win2003）不是具有这些 cmdlet 的已识别参数。  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>LDAP 查询优化器更改  
  
### <a name="overview"></a>概述  
LDAP 查询优化器算法已重新评估并经过进一步优化。  结果是在 LDAP 搜索效率和复杂查询的 LDAP 搜索时间方面的性能改进。  
  
> [!NOTE]
> <strong>从开发人员：</strong>在从 LDAP 查询到 ESE 查询的映射中，对改进的搜索性能进行了改进。  超出特定级别的复杂性的 LDAP 筛选器会阻止优化索引选择，导致性能大幅降低（1000倍或更多）。 此更改将改变为 LDAP 查询选择索引的方式，以避免此问题。  
> 
> [!NOTE]
> 完全彻底降低 LDAP 查询优化器算法，导致：  
> 
> -   更快搜索时间  
> -   提高了效率，使 Dc 可以执行更多操作  
> -   对 AD 性能问题的更少支持调用  
> -   返回到 Windows Server 2008 R2 （KB 2862304）  
  
### <a name="background"></a>后台  
搜索 Active Directory 功能是由域控制器提供的核心服务。  其他服务和业务线应用程序依赖于 Active Directory 搜索。  如果此功能不可用，业务操作可能会停止停止。  作为核心和使用频繁的服务，域控制器必须有效地处理 LDAP 搜索流量。  LDAP 查询优化器算法尝试通过将 LDAP 搜索筛选器映射到可通过数据库中已建立索引的记录来满足的结果集，从而使 LDAP 搜索尽可能高效。  此算法已重新评估并经过进一步优化。  结果是在 LDAP 搜索效率和复杂查询的 LDAP 搜索时间方面的性能改进。  
  
### <a name="details-of-change"></a>更改详细信息  
LDAP 搜索包含：  
  
-   在层次结构中开始搜索的位置（NC 头、OU、对象）  
  
-   搜索筛选器  
  
-   要返回的属性的列表  
  
搜索过程可总结如下：  
  
1.  尽可能简化搜索筛选器。  
  
2.  选择一组将返回最小涵盖集的索引键。  
  
3.  执行索引键的一个或多个交集，以减少涵盖的集。  
  
4.  对于涵盖的集中的每个记录，请评估筛选器表达式以及安全性。 如果筛选器的计算结果为 TRUE 并且已授予访问权限，则将此记录返回到客户端。  
  
LDAP 查询优化工作会修改步骤2和步骤3，以减小覆盖集的大小。 更具体地说，当前实现选择重复的索引键并执行冗余的交集。  
  
### <a name="comparison-between-old-and-new-algorithm"></a>旧算法与新算法之间的比较  
在此示例中，低效 LDAP 搜索的目标是 Windows Server 2012 域控制器。  由于找不到更有效的索引，搜索完成时间大约为44秒。  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=justintu@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfind.txt  
  
Using server: WINSRV-DC1.blue.contoso.com:389  
  
<removed search results>  
  
Statistics  
=====  
Elapsed Time: 44640 (ms)  
Returned 324 entries of 553896 visited - (0.06%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 DNT_index:516615:N  
  
Pages Referenced          : 4619650  
Pages Read From Disk      : 973  
Pages Pre-read From Disk  : 180898  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
### <a name="sample-results-using-the-new-algorithm"></a>使用新算法的示例结果  
此示例重复相同的搜索，但目标为 Windows Server 2012 R2 域控制器。  由于 LDAP 查询优化器算法中的改进，同一搜索将在不到一秒内完成。  
  
```  
adfind -b dc=blue,dc=contoso,dc=com -f "(| (& (|(cn=justintu) (postalcode=80304) (userprincipalname=dhunt@blue.contoso.com)) (|(objectclass=person) (cn=justintu)) ) (&(cn=justintu)(objectclass=person)))" -stats >>adfindBLUE.txt  
  
Using server: winblueDC1.blue.contoso.com:389  
  
.<removed search results>  
  
Statistics  
=====  
Elapsed Time: 672 (ms)  
Returned 324 entries of 648 visited - (50.00%)  
  
Used Filter:  
 ( |  ( &  ( |  (cn=justintu)  (postalCode=80304)  (userPrincipalName=justintu@blue.contoso.com) )  ( |  (objectClass=person)  (cn=justintu) ) )  ( &  (cn=justintu)  (objectClass=person) ) )   
  
Used Indices:  
 idx_userPrincipalName:648:N  
 idx_postalCode:323:N  
 idx_cn:1:N  
  
Pages Referenced          : 15350  
Pages Read From Disk      : 176  
Pages Pre-read From Disk  : 2  
Pages Dirtied             : 0  
Pages Re-Dirtied          : 0  
Log Records Generated     : 0  
Log Record Bytes Generated: 0  
```  
  
-   如果无法优化树：  
  
    -   例如：树中的表达式在未建立索引的列上  
  
    -   记录阻止优化的索引列表  
  
    -   通过 ETW 跟踪和事件 ID 1644 公开  
  
        ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>启用 LDP 中的统计信息控件  
  
1.  打开 LDP.EXE，然后连接并绑定到域控制器。  
  
2.  在 "**选项**" 菜单上，单击 "**控件**"。  
  
3.  在 "控件" 对话框中，展开 "**加载预定义**的" 下拉菜单，单击 "**搜索统计**信息"，然后单击 **"确定"** 。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  在 "**浏览**" 菜单上，单击 "**搜索**"  
  
5.  在 "搜索" 对话框中，选择 "**选项**" 按钮。  
  
6.  确保在 "搜索选项" 对话框中选中了 "**扩展**" 复选框，然后选择 **"确定"** 。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>请尝试执行以下操作：使用 LDP 返回查询统计信息  
在域控制器上或在已加入域的客户端或安装了 AD DS 工具的服务器上执行以下。  对 Windows Server 2012 DC 和 Windows Server 2012 R2 DC 执行以下操作。  
  
1.  查看["创建更有效的 MICROSOFT AD 应用程序"](https://msdn.microsoft.com/library/ms808539.aspx)一文，并根据需要向后参考。  
  
2.  使用 LDP，启用搜索统计信息（请参阅[以在 LDP 中启用统计信息控件](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats)）  
  
3.  执行多个 LDAP 搜索，并观察结果顶部的统计信息。  你将在其他活动中重复相同的搜索，以便将其记录在记事本文本文件中。  
  
4.  执行 LDAP 搜索，因为属性索引，查询优化器应该能够优化该搜索  
  
5.  尝试构造一个需要很长时间才能完成的搜索（你可能想要增加**时间限制**选项，使搜索不会超时）。  
  
### <a name="additional-resources"></a>其他资源  
[什么是 Active Directory 搜索？](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Active Directory 搜索的工作方式](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[创建更有效的 Microsoft Active Directory 应用程序](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) LDAP 查询执行速度比 AD 或 LDS/ADAM 目录服务中的预期慢，并且可能会记录事件 ID 1644  
  
## <a name="BKMK_1644"></a>1644事件改进  
  
### <a name="overview"></a>概述  
此更新向事件 ID 1644 添加了更多 LDAP 搜索结果统计信息，以帮助进行故障排除。  此外，还有一个新的注册表值，可用于根据基于时间的阈值启用日志记录。  这些改进已在 Windows Server 2012 和 Windows Server 2008 R2 SP1 中通过 KB [2800945](https://support.microsoft.com/kb/2800945)提供，并可用于 windows SERVER 2008 SP2。  
  
> [!NOTE]  
> -   向事件 ID 1644 添加了其他 LDAP 搜索统计信息，以帮助排查低效或昂贵的 LDAP 搜索问题  
> -   你现在可以指定搜索时间阈值（例如 日志事件1644，搜索花费的时间超过100ms，而不是指定成本昂贵且低效的搜索结果阈值  
  
### <a name="background"></a>后台  
当对 Active Directory 性能问题进行故障排除时，LDAP 搜索活动可能导致问题。  您决定启用日志记录，以便您可以看到由域控制器处理的昂贵或低效的 LDAP 查询。  若要启用日志记录，必须设置 "字段工程诊断" 值，还可以选择指定昂贵/低效的搜索结果阈值。  启用字段工程日志记录级别为5后，满足这些条件的任何搜索都将记录在包含事件 ID 1644 的目录服务事件日志中。  
  
事件包含：  
  
-   客户端 IP 和端口  
  
-   正在启动节点  
  
-   Filter  
  
-   搜索范围  
  
-   属性选择  
  
-   服务器控件  
  
-   已访问条目  
  
-   返回的项  
  
不过，事件中缺少关键数据，如搜索操作所用的时间和使用的索引（如果有）。  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>添加到事件1644的其他搜索统计信息  
  
-   已用索引  
  
-   引用的页  
  
-   从磁盘读取的页面  
  
-   从磁盘 preread 页面  
  
-   清理页面已修改  
  
-   已修改脏页  
  
-   搜索时间  
  
-   禁止优化的属性  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>事件1644日志记录的新的基于时间的阈值注册表值  
你可以指定搜索时间阈值，而不是指定成本高昂且效率低下的搜索结果阈值。  如果你想要记录占用 50 ms 或更长时间的所有搜索结果，则应指定 50 decimal/32 十六进制（除了设置字段工程值外）。  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>旧的和新的事件 ID 1644 的比较  
原来  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
新增  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>请尝试执行以下操作：使用事件日志返回查询统计信息  
  
1.  对 Windows Server 2012 DC 和 Windows Server 2012 R2 DC 执行以下操作。 每次搜索后，在这两个 Dc 上观察事件 ID 1644s。  
  
2.  使用 regedit，在 Windows Server 2012 R2 DC 上使用基于时间的阈值启用事件 ID 1644 日志记录，并在 Windows Server 2012 DC 上使用旧方法启用事件 ID 日志记录。  
  
3.  执行若干超出阈值的 LDAP 搜索，并观察结果顶部的统计信息。  使用之前记录的 LDAP 查询，并重复相同的搜索。  
  
4.  执行 LDAP 搜索，因为没有为一个或多个属性建立索引，查询优化器无法优化。  
  
## <a name="BKMK_ADRepl"></a>Active Directory 复制吞吐量改进  
  
### <a name="overview"></a>概述  
AD 复制使用 RPC 进行复制传输。 默认情况下，RPC 使用8K 传输缓冲区和大量的数据包大小。 这将产生净效果，其中发送实例将传输三个数据包（大约为15K 的数据），然后必须等待网络往返，然后再发送更多数据包。 假设3ms 往返时间，最高吞吐量将绕40Mbps 调整，即使是在1Gbps 或 10 Gbps 网络上也是如此。  
  
> [!NOTE]  
> -   此更新将最大 AD 复制吞吐量从40Mbps 调整调整到 600 Mbps。  
>   
>     -   它增加了 RPC 发送缓冲区大小，从而减少了网络往返次数  
> -   这种效果最明显，最明显的是高速度、高延迟的网络。  
  
此更新通过将 RPC 发送缓冲区大小从8K 改为256，将最大吞吐量增加到 600 Mbps。  此更改允许 TCP 窗口大小增长到8K 以上，从而减少了网络往返次数。  
  
> [!NOTE]  
> 没有可配置的设置来修改此行为。  
  
### <a name="additional-resources"></a>其他资源  
[Active Directory 复制模型的工作方式](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


