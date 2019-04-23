---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: 目录服务组件更新
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f3e0553b1919a7f9129d47616d0ffb66b6ff48f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874438"
---
# <a name="directory-services-component-updates"></a>目录服务组件更新

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
本课程介绍了 Windows Server 2012 R2 中的目录服务组件更新。  
  
## <a name="what-you-will-learn"></a>你将学习的内容  
介绍了以下新的目录服务组件更新：  
  
-   介绍了以下新的目录服务组件更新：  
  
    -   [域和林功能级别](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [弃用 NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [LDAP 查询优化程序更改](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [1644 事件改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Active Directory 复制吞吐量改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>域和林功能级别  
  
### <a name="overview"></a>概述  
部分提供了简要介绍了域和林功能级别的更改。  
  
### <a name="new-dfl-and-ffl"></a>新 DFL 和 FFL  
版本中，有新的域和林功能级别：  
  
-   林功能级别：Windows Server 2012 R2  
  
-   域功能级别：Windows Server 2012 R2  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 域功能级别启用了以下支持：  
  
1.  为 DC 端保护*受保护的用户*  
  
    *受保护的用户*对 Windows Server 2012 R2 域进行身份验证可以**不再**:  
  
    -   使用 NTLM 身份验证进行身份验证  
  
    -   在 Kerberos 预身份验证中使用 DES 或 RC4 密码套件  
  
    -   使用不受约束或约束委派进行委派  
  
    -   超出初始 4 小时生存期后续订用户票证 (Tgt)  
  
2.  身份验证策略  
  
    基于新林的 Active Directory 策略可应用于 Windows Server 2012 R2 域来控制哪些主机中的帐户的一个帐户可以从登录和身份验证的访问控制条件应用到运行的帐户服务  
  
3.  身份验证策略接收器  
  
    基于新林的 Active Directory 对象可以创建用户、 托管的服务和计算机帐户，用于将分类的身份验证策略或进行身份验证隔离帐户之间的关系。  
  
请参阅如何配置受保护的帐户的详细信息。  
  
除了上述功能，Windows Server 2012 R2 域功能级别可确保域中的所有域控制器都运行 Windows Server 2012 R2。  
Windows Server 2012 R2 林功能级别不提供任何新功能，但它可以确保在林中创建的任何新域将自动运行在 Windows Server 2012 R2 域功能级别。  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>创建新的域上强制执行的最小 DFL  
Windows Server 2008 dfl 上是在创建新的域上受支持的最低功能级别。  
  
> [!NOTE]  
> 不推荐使用的 FRS 通过删除功能具有域功能级别低于 Windows Server 2008 使用服务器管理器或通过 Windows PowerShell 安装新的域。  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低林和域功能级别  
林和域功能级别设置为 Windows Server 2012 R2 上创建新的域和新林的默认情况下，但可以使用 Windows PowerShell 已降低。  
  
若要提高或降低林功能级别使用 Windows PowerShell，请使用**Set-adforestmode** cmdlet。  
  
**若要将 contoso.com FFL 设置为 Windows Server 2008 模式：**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
若要提高或降低域功能级别使用 Windows PowerShell，使用 Set-addomainmode cmdlet。  
  
**若要将 contoso.com dfl 上设置为 Windows Server 2008 模式：**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
升级到运行 2003 dfl 上的现有域中运行 Windows Server 2012 R2 作为其他副本的 DC 的工作原理。  
  
在现有林中创建新的域  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
没有任何新的林或域操作在此版本中。  
  
这些.ldf 文件包含的架构更改**设备注册服务**。  
  
1.  Sch59  
  
2.  Sch61  
  
3.  Sch62  
  
4.  Sch63  
  
5.  Sch64  
  
6.  Sch65  
  
7.  Sch67  
  
**工作文件夹：**  
  
1.  Sch66  
  
**MSODS:**  
  
1.  Sch60  
  
**身份验证策略和接收器**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>弃用 NTFRS  
  
### <a name="overview"></a>概述  
FRS 是 Windows Server 2012 R2 中不推荐使用。  不推荐使用的 FRS 通过强制执行 Windows Server 2008 的最小域功能级别 (DFL)。  仅当使用服务器管理器或 Windows PowerShell 创建新的域存在这种强制措施。  
  
与 Install-addsforest 或 Install-addsdomain cmdlet 使用的域模式参数来指定域功能级别。  支持此参数的值可以是一个有效的整数或相应的枚举的字符串值。 例如，若要将域模式级别设置为 Windows Server 2008 R2 中，可以指定的值为 4 或"Win2008R2"。  从 Server 2012 R2 有效执行这些 cmdlet 时的值包括与 Windows Server 2008 (3，Win2008) Windows Server 2008 R2 (4，Win2008R2) Windows Server 2012 (5，Win2012) 和 Windows Server 2012 R2 (6，Win2012R2)。 域功能级别不能低于林控制级别，但可以高于林控制级别。  由于 FRS 已弃用在此版本中，Windows Server 2003 (2，Win2003) 不是使用这些 cmdlet 执行从 Windows Server 2012 R2 时识别的参数。  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>LDAP 查询优化器更改  
  
### <a name="overview"></a>概述  
LDAP 查询优化器算法已重新计算，并进一步优化。  结果是 LDAP 搜索效率和 LDAP 搜索时间的复杂查询的性能改进。  
  
> [!NOTE]  
> **从开发人员：** ESE 查询到查询中通过 LDAP 从映射中的改进搜索性能的改进。  一定程度的复杂性之外的 LDAP 筛选器阻止经过优化的索引选择，从而导致性能显著降低 （1000 x 或以上）。 此更改会改变在其中我们选择索引 LDAP 查询，以避免此问题的方法。  
  
> [!NOTE]  
> LDAP 查询优化器算法，从而导致了彻底的革新：  
>   
> -   缩短搜索时间  
> -   效率提升允许域控制器来执行更多操作  
> -   有关 AD 性能问题更少电话支持  
> -   后移植到 Windows Server 2008 R2 (KB 2862304)  
  
### <a name="background"></a>后台  
搜索 Active Directory 的功能为核心服务提供的域控制器。  其他服务和业务线应用程序依赖于 Active Directory 搜索。  未提供此功能时，可以停止的业务操作。  作为核心和常用的服务，它是命令性，域控制器将有效地处理 LDAP 搜索流量。  LDAP 查询优化器算法尝试通过将 LDAP 搜索筛选器映射到可以满足通过记录在数据库中已编制索引的结果集，使 LDAP 搜索尽可能高效。  此算法已重新计算，并进一步优化。  结果是 LDAP 搜索效率和 LDAP 搜索时间的复杂查询的性能改进。  
  
### <a name="details-of-change"></a>更改的详细信息  
LDAP 搜索包含：  
  
-   若要开始搜索的层次结构内的位置 （NC 头，OU，对象）  
  
-   搜索筛选器  
  
-   要返回的特性列表  
  
搜索过程可总结如下：  
  
1.  尽可能简化搜索筛选器。  
  
2.  选择将返回的最小的集已覆盖的索引键的一组。  
  
3.  执行索引键，以减少涵盖的集的交集的一个或多个。  
  
4.  对于在涵盖的集中每个记录，评估筛选器表达式，以及安全性。 如果筛选器的计算结果为 TRUE，并且授予访问权限，然后返回此记录到客户端。  
  
LDAP 查询优化工作修改步骤 2 和 3，以减少涵盖集的大小。 具体而言，当前实现选择重复的索引键，并执行冗余的交集。  
  
### <a name="comparison-between-old-and-new-algorithm"></a>旧的和新算法之间的比较  
在此示例中效率低下的 LDAP 搜索的目标是 Windows Server 2012 域控制器。  由于找不到更高效的索引大约 44 秒钟即可完成搜索。  
  
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
此示例与上面完全相同的搜索将重复，但面向 Windows Server 2012 R2 域控制器。  由于 LDAP 查询优化器算法中的改进小于一秒内完成相同的搜索。  
  
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
  
    -   例如： 在树中的表达式是通过未编制索引的列  
  
    -   记录阻止优化的索引的列表  
  
    -   通过 ETW 跟踪和事件 ID 1644 公开  
  
        ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>若要启用在 LDP 中的统计信息控件  
  
1.  打开 LDP.exe，并连接并绑定到域控制器。  
  
2.  上**选项**菜单上，单击**控件**。  
  
3.  在控件对话框中，展开**预定义的负载**下拉菜单，单击**搜索 Stats** ，然后单击**确定**。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  上**浏览**菜单上，单击**搜索**  
  
5.  在搜索对话框中，选择**选项**按钮。  
  
6.  请确保**扩展**复选框被选中的搜索选项对话框并选择**确定**。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>请尝试此操作：使用 LDP 返回查询统计信息  
在域控制器上，或从已加入域的客户端或安装了 AD DS 工具的服务器，请执行以下步骤。  重复以下面向 Windows Server 2012 DC 和 Windows Server 2012 R2 DC。  
  
1.  审阅["创建更高效 Microsoft AD 启用应用程序"](https://msdn.microsoft.com/library/ms808539.aspx)一文，根据需要回头参考它。  
  
2.  使用 LDP 启用搜索的统计信息 (请参阅[若要启用在 LDP 中的统计信息控件](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  执行多个 LDAP 搜索，并观察结果顶部的统计信息。  将重复相同的搜索中其他活动因此将它们记录在记事本文本文件中。  
  
4.  执行 LDAP 搜索的查询优化器应该能够优化由于属性索引  
  
5.  尝试构造搜索需要很长时间才能完成 (可能需要增加**时间限制**选项，使搜索不会超时)。  
  
### <a name="additional-resources"></a>其他资源  
[Active Directory 搜索有哪些？](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Active Directory 搜索的工作方式](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[创建更高效的 Microsoft Active Directory 启用应用程序](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581)速度低于预期在 AD 中执行 LDAP 查询，或可能记录 LDS/ADAM 目录服务和事件 ID 1644  
  
## <a name="BKMK_1644"></a>1644 事件改进  
  
### <a name="overview"></a>概述  
此更新将附加的 LDAP 搜索结果统计信息添加到事件 ID 1644 以帮助进行故障排除。  此外，没有可用于基于时间的阈值上启用日志记录的新注册表值。  这些改进了可在 Windows Server 2012 和 Windows Server 2008 R2 SP1 通过 KB [2800945](https://support.microsoft.com/kb/2800945)和将可供 Windows Server 2008 SP2。  
  
> [!NOTE]  
> -   附加的 LDAP 搜索统计信息添加到事件 ID 1644 以帮助解决效率降低或昂贵的 LDAP 搜索  
> -   现在，您可以指定搜索时间阈值 （例如。 搜索时间超过 100 毫秒的日志事件 1644年） 而不是指定的昂贵和 Inefficient 的搜索结果的阈值  
  
### <a name="background"></a>后台  
Active Directory 性能问题进行故障排除，时变得明显，LDAP 搜索活动可能会帮助解决问题。  您决定启用日志记录，以便您可以看到由域控制器处理的成本高昂或效率低下 LDAP 查询。  若要启用日志记录，必须设置现场工程诊断值并可以选择指定昂贵 / 效率低下的搜索结果的阈值。  启用现场工程的值为 5 到日志记录级别，时满足这些条件的任何搜索记录事件 ID 1644 在目录服务事件日志中。  
  
该事件包含：  
  
-   客户端 IP 和端口  
  
-   起始节点  
  
-   Filter  
  
-   搜索范围  
  
-   属性选择  
  
-   服务器控件  
  
-   访问过的条目  
  
-   返回的条目  
  
但是，关键数据缺少事件等 （如果有），在搜索操作和上花费的时间量已使用索引。  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>其他搜索统计信息添加到的事件 1644  
  
-   使用的索引  
  
-   页引用  
  
-   从磁盘读取页  
  
-   从磁盘 preread 页  
  
-   修改的干净页  
  
-   修改的脏页  
  
-   搜索时间  
  
-   属性阻止优化  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>新的基于时间的阈值注册表值，用于日志记录的事件 1644  
而不是指定的昂贵和 Inefficient 的搜索结果的阈值，可以指定搜索时间阈值。  如果您想花费 50 毫秒的所有搜索结果都记录或更高版本，则会指定十进制 50 / 32 十六进制 （除了设置现场工程值）。  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>旧的和新的事件 ID 1644 的比较  
旧  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
新增  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>请尝试此操作：使用事件日志以返回查询统计信息  
  
1.  重复以下面向 Windows Server 2012 DC 和 Windows Server 2012 R2 DC。 每个搜索后观察两个域控制器上的事件 ID 1644s。  
  
2.  使用注册表编辑器，启用的事件 ID 1644 日志记录在 Windows Server 2012 R2 DC 和 Windows Server 2012 DC 上的旧方法上使用基于时间的阈值。  
  
3.  执行多个 LDAP 搜索，超过阈值，并且观察结果顶部的统计信息。  使用前面所述的 LDAP 查询并重复相同的搜索。  
  
4.  执行 LDAP 搜索的查询优化器不能优化因为一个或多个属性未编制索引。  
  
## <a name="BKMK_ADRepl"></a>Active Directory 复制吞吐量改进  
  
### <a name="overview"></a>概述  
AD 复制进行复制传输使用 RPC。 默认情况下，RPC 使用的 8k 的传输缓冲区和 5 个 K 数据包大小。 这具有其中发送实例将传输三个数据包 (大约 15 K 值得的数据），然后必须在等待网络往返行程发送更多的净效果。 假设 3 毫秒往返时间 40mbps 调整，甚至在 1Gbps 或 10 Gbps 网络上大约是最高的吞吐量。  
  
> [!NOTE]  
> -   此更新调整最大 AD 复制吞吐量从 40mbps 调整到 600 Mbps 左右。  
>   
>     -   它会增加 RPC 的发送缓冲区大小，它可以减少网络往返行程  
> -   因此效果将显著上高速度、 高延迟网络。  
  
此更新通过从 8 K 的 RPC 发送缓冲区大小更改为 256 KB 增加到 600 Mbps 左右的最大吞吐量。  此更改允许 TCP 窗口大小以增加到超出 8 K、 减少网络往返。  
  
> [!NOTE]  
> 没有可配置设置来修改此行为。  
  
### <a name="additional-resources"></a>其他资源  
[Active Directory 复制模型的工作原理](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


