---
ms.assetid: 8a3cf2ae-2511-4eea-afd5-a43179a78613
title: "目录服务构件更新"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac42591450038240ced273555fb01e66b1ff5546
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="directory-services-component-updates"></a>目录服务构件更新

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

**作者**：与 Windows 组索旋转器高级支持升级工程师  
  
> [!NOTE]  
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。  
  
此课介绍在 Windows Server 2012 R2 的目录服务组件更新。  
  
## <a name="what-you-will-learn"></a>您将学会  
说明以下新目录服务组件更新：  
  
-   说明以下新目录服务组件更新：  
  
    -   [域和森林功能级别](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
    -   [NTFRS deprecation](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
    -   [LDAP 查询优化更改](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
    -   [1644 事件改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
    -   [Active Directory 复制吞吐量改进](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  
## <a name="BKMK_FL"></a>域和森林功能级别  
  
### <a name="overview"></a>概述  
部分提供了域和森林功能级别更改的简要介绍。  
  
### <a name="new-dfl-and-ffl"></a>新 DFL 和 FFL  
版本中，有新的域和森林功能级别：  
  
-   森林功能级别：Windows Server 2012 R2  
  
-   设置 Windows Server 2012 R2 域功能级别：  
  
### <a name="the-windows-server-2012-r2-domain-functional-level-enables-support-for-the-following"></a>Windows Server 2012 R2 域功能级别支持支持以下操作：  
  
1.  为 DC 侧保护*保护用户*  
  
    *保护用户*Windows Server 2012 R2 域对进行身份验证可以**不再**:  
  
    -   通过 NTLM 身份验证身份验证  
  
    -   使用 DES 或 RC4 密码套件中 Kerberos 预先身份验证  
  
    -   使用不受约束或受限制委派委派  
  
    -   续订初始 4 小时整个使用期限内之外的用户门票 (Tgt)  
  
2.  身份验证策略  
  
    新林基于 Active Directory 策略，这可用于 Windows Server 2012 R2 域来控制哪些主机中的帐户的帐户可以登录从和适用于运行的为某个帐户的服务的访问控制条件身份验证  
  
3.  身份验证策略思  
  
    基于新林 Active Directory 对象进行可以创建用于分类帐户身份验证的策略或身份验证隔离用户、托管的服务和计算机帐户之间的关系。  
  
查看如何配置受保护的帐户添加详细信息。  
  
除了上面的功能，Windows Server 2012 R2 域功能级别确保域中的任何域控制器可以运行 Windows Server 2012 R2。  
Windows Server 2012 R2 森林功能级别不提供任何新的功能，但它确保创建林中任何新域将自动能在 Windows Server 2012 R2 域功能级别。  
  
### <a name="minimum-dfl-enforced-on-new-domain-creation"></a>在创建新的域上强制执行最低 DFL  
Windows Server 2008 DFL 是支持上创建新的域的小功能级别。  
  
> [!NOTE]  
> FRS 弃用通过删除与 Windows Server 2008 使用服务器管理器，或通过 Windows PowerShell 低于域功能级别安装新的域的能力。  
  
### <a name="lowering-the-forest-and-domain-functional-levels"></a>降低林和域的功能级别  
森林和域的功能级别设置为 Windows Server 2012 R2 域和新林创建新的默认情况下，但可以使用 Windows PowerShell 降低。  
  
提高或降低使用 Windows PowerShell 森林功能级别，请使用**组 ADForestMode** cmdlet。  
  
**若要设置为 Windows Server 2008 模式 contoso.com FFL:**  
  
```sql  
Set-ADForestMode -ForestMode Windows2008Forest -Identity contoso.com  
```  
  
提高或降低使用 Windows PowerShell 域功能级别，请使用 Set-ADDomainMode cmdlet。  
  
**若要设置为 Windows Server 2008 模式 contoso.com DFL:**  
  
```powershell  
Set-ADDomainMode -DomainMode Windows2008Domain -Identity contoso.com  
```  
  
升级到运行 2003 DFL 现有域其他副本的运行 Windows Server 2012 R2 域控制器工作。  
  
现有的树林中创建新的域  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_FFL.gif)  
  
### <a name="adprep"></a>ADPREP  
没有新林或域此版本中的操作。  
  
这些.ldf 文件包含模式更改为**设备注册服务**。  
  
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
  
**身份验证的策略和思**  
  
1.  Sch68  
  
2.  Sch69  
  
## <a name="BKMK_NTFRS"></a>NTFRS deprecation  
  
### <a name="overview"></a>概述  
FRS 不再支持在 Windows Server 2012 R2。  FRS 弃用通过履行 Windows Server 2008 最低域功能级别 (DFL)。  此强制才会显示使用服务器管理器或 Windows PowerShell 创建新的域。  
  
使用与这些 Install-ADDSForest 或 Install-ADDSDomain cmdlet-DomainMode 参数，若要指定域功能级别。  受支持的值为此参数可以有效整数或相应的值枚举的字符串。 例如，若要设置与 Windows Server 2008 R2 域模式级别，你可以指定值为 4 或"Win2008R2"。  当从 Server 2012 R2 有效值执行这些 cmdlet 包括这些适用于 Windows Server 2008 (3 Win2008) Windows Server 2008 R2 (4 Win2008R2) (5，Win2012) 的 Windows Server 2012 和 Windows Server 2012 R2 (6，Win2012R2)。 域功能级别能小于林功能级别，但更高版本。  由于 FRS 弃用在此发布，Windows Server 2003 (2、Win2003) 不是已知与 Windows Server 2012 R2 从执行这些 cmdlet 参数。  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_Install2003DFL.gif)  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_PS_InstallDFL2.gif)  
  
## <a name="BKMK_LDAPQuery"></a>LDAP 查询优化更改  
  
### <a name="overview"></a>概述  
LDAP 查询优化算法已重新计算和更多优化。  但结果很 LDAP 搜索的效率和 LDAP 搜索查询时间的复杂方面的性能改进。  
  
> [!NOTE]  
> **开发人员：**改进的性能改进从 LDAP 映射中通过的搜索查询到 ESE 查询。  超过一定复杂性 LDAP filters 阻止优化的索引选择，导致大幅降低性能（1000 x 或更多）。 此更改会更改我们在其中选择 LDAP 查询，以避免此问题的指数的方式。  
  
> [!NOTE]  
> 导致的 LDAP 查询优化算法彻底检查：  
>   
> -   更快速地搜索的时间  
> -   效率允许 Dc 以执行更多操作  
> -   不支持呼叫太相关的广告性能问题  
> -   返回到 Windows Server 2008 R2 移植 (KB 2862304)  
  
### <a name="background"></a>背景  
搜索 Active Directory 的功能是核心服务提供的域控制器。  其他业务线应用程序和服务需要使用 Active Directory 搜索。  如果不提供此功能，可以停止的业务运营。  作为核心，并使用频繁服务，，则必须域控制器高效地处理 LDAP 搜索通信。  尝试通过可以通过记录，已经在数据库索引满足的结果组映射 LDAP 搜索筛选器，使 LDAP 搜索高效尽可能 LDAP 查询优化算法。  此算法已重新计算和更多优化。  但结果很 LDAP 搜索的效率和 LDAP 搜索查询时间的复杂方面的性能改进。  
  
### <a name="details-of-change"></a>更改详细信息  
包含 LDAP 搜索：  
  
-   中分层开始搜索某个位置（NC 头，OU，对象）  
  
-   搜索筛选器  
  
-   若要返回的特性列表  
  
搜索进程总结如下：  
  
1.  如果可能简化搜索筛选器。  
  
2.  选择一套指数键将返回最小的覆盖的集。  
  
3.  执行一个或多个交叉点指数键，以减少覆盖的组。  
  
4.  对于每项记录均已覆盖集中，评估筛选器 expression 以及安全。 如果筛选器的值为 TRUE 且被授予访问权限，然后返回到客户端此记录。  
  
LDAP 查询优化工作修改步骤 2 到 3 减少覆盖集的大小。 具体而言，当前实现选择重复指数键，并执行冗余交叉点。  
  
### <a name="comparison-between-old-and-new-algorithm"></a>之间旧和新算法比较  
在此示例中低效 LDAP 搜索的目标是 Windows Server 2012 域控制器。  搜索完成后大约 44 秒定无法找到更高效索引中。  
  
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
  
### <a name="sample-results-using-the-new-algorithm"></a>使用新的算法示例结果  
此示例重复确切与上述相同搜索，但面向 Windows Server 2012 R2 域控制器。  相同的搜索完成此过程小于根由于 LDAP 查询优化算法中的改进。  
  
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
  
    -   例如：树中的 expression 已通过未编入索引列  
  
    -   录制防止优化的指数的列表  
  
    -   可以通过 ETW 跟踪和事件 ID 1644  
  
        ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644.gif)  
  
### <a name="BKMK_EnableStats"></a>若要启用 LDP 中的统计数据控件  
  
1.  打开 LDP.exe，和连接到域控制器绑定。  
  
2.  在**选项**菜单上，单击**控件**。  
  
3.  在控制对话框中，展开**预定义加载**下拉菜单中，单击**搜索统计数据**，然后单击**确定**。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Controls.gif)  
  
4.  在**浏览**菜单上，单击**搜索**  
  
5.  在搜索对话框中，选择**选项**按钮。  
  
6.  确保**扩展**搜索选项对话框中，选择上选中复选框**确定**。  
  
    ![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_SearchOptions.gif)  
  
### <a name="try-this-use-ldp-to-return-query-statistics"></a>请尝试以下问题：使用 LDP 返回查询统计信息  
执行以下步骤，域控制器，或从已加入域的客户端或已安装的广告 DS 工具的服务器。  重复以下定位你的 Windows Server 2012 DC 和你的 Windows Server 2012 R2 DC。  
  
1.  查看["创建多个高效 Microsoft 广告启用应用程序"](https://msdn.microsoft.com/library/ms808539.aspx)文章并根据需要回头到它。  
  
2.  使用 LDP，使搜索统计数据 (请参阅[启用 LDP 中的统计数据控制](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_EnableStats))  
  
3.  开展多个 LDAP 搜索，然后观察结果顶部统计信息。  你将重复以其他相同的搜索活动因此在记事本文本文件文档它们。  
  
4.  执行查询优化应该能够由于属性指数优化 LDAP 搜索  
  
5.  尝试构建搜索采用很长时间才能完成 (你可能想要增加**时间限制**选项，因此搜索不超时)。  
  
### <a name="additional-resources"></a>更多资源  
[什么是 Active Directory 搜索？](https://technet.microsoft.com/library/cc783845(v=ws.10).aspx)  
  
[Active Directory 搜索的工作方式](https://technet.microsoft.com/library/cc755809(v=WS.10).aspx)  
  
[创建更高效 Microsoft Active Directory 启用应用程序](https://msdn.microsoft.com/library/ms808539.aspx)  
  
[951581](https://support.microsoft.com/kb/951581) LDAP 查询时执行缓慢比预期 ad 或 LDS/ADAM 目录服务和事件 ID 1644 可能会记录  
  
## <a name="BKMK_1644"></a>1644 事件改进  
  
### <a name="overview"></a>概述  
此更新将添加到事件 ID 1644 来帮助进行故障排除的其他 LDAP 搜索结果统计信息。  此外，是一个新的注册表值，可用于基于时间的 threshold 上启用日志记录。  适用于 Windows Server 2012 和 Windows Server 2008 R2 SP1 通过 KB 进行这些改进[2800945](https://support.microsoft.com/kb/2800945)将提供给进行 Windows Server 2008 SP2 和。  
  
> [!NOTE]  
> -   其他 LDAP 搜索统计信息添加到事件 ID 1644 可帮助诊断低效或便宜 LDAP 搜索  
> -   现在，你可以指定搜索时间 Threshold（例如 用于搜索的时间超过 100ms 以上日志事件 1644 年）而不是指定的昂贵和 Inefficient 搜索结果 threshold 值  
  
### <a name="background"></a>背景  
疑难解答 Active Directory 的性能问题，同时很明显 LDAP 搜索活动，可能导致问题。  你决定要启用日志记录，以便你可以看到便宜或低效 LDAP 查询域控制器处理。  启用日志记录，以便你必须设置字段工程诊断值，并且还可以选择指定便宜 / 低效搜索结果阈值。  在启用字段工程用于登录的值为 5 级别，符合这些条件的任何搜索登录事件 ID 1644 目录服务事件日志。  
  
包含的事件：  
  
-   客户端 IP 和端口  
  
-   开始节点  
  
-   筛选器  
  
-   搜索范围  
  
-   属性选择  
  
-   服务器控件  
  
-   访问的条目  
  
-   返回的项  
  
但是，重要数据从事件丢失了指数曾诸如花费的时间搜索操作和（如果有）。  
  
#### <a name="additional-search-statistics-added-to-event-1644"></a>更多搜索统计信息添加到 1644 年事件  
  
-   使用的索引  
  
-   引用的页面  
  
-   从磁盘读取的页面  
  
-   从磁盘 preread 页面  
  
-   修改干净页  
  
-   修改脏页  
  
-   搜索时间  
  
-   阻止优化属性  
  
#### <a name="new-time-based-threshold-registry-value-for-event-1644-logging"></a>新的基于时间的注册表阈值 1644 年事件日志记录  
而不是指定的昂贵和 Inefficient 搜索结果 threshold 值，你可以指定搜索时间阈值。  如果你需要记录拍摄 50 ms 的所有搜索结果或更高版本，你应指定 50 小数点 / 32 十六进制（例如，除了设置字段工程值）。  
  
```  
Windows Registry Editor Version 5.00  
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters]  
"Search Time Threshold (msecs)"=dword:00000032  
```  
  
#### <a name="comparison-of-the-old-and-new-event-id-1644"></a>旧和新事件 ID 1644 比较  
旧  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012.gif)  
  
新增功能  
  
![目录服务更新](media/Directory-Services-component-updates/GTR_ADDS_Event1644_2012R2.gif)  
  
#### <a name="try-this-use-the-event-log-to-return-query-statistics"></a>请尝试以下问题：使用事件日志返回查询统计信息  
  
1.  重复以下定位你的 Windows Server 2012 DC 和你的 Windows Server 2012 R2 DC。 在每次搜索后观察两个域控制器在事件 ID 1644s。  
  
2.  通过使用 regedit，使事件 ID 1644 日志记录使用基于时间的 threshold Windows Server 2012 R2 域控制器和 Windows Server 2012 DC 在旧的方法。  
  
3.  执行超过 threshold，然后观察结果顶部统计信息的几个 LDAP 搜索。  使用你之前记录的 LDAP 查询和重复相同搜索。  
  
4.  执行 LDAP 搜索查询优化不能优化由于未编入索引一个或多个属性。  
  
## <a name="BKMK_ADRepl"></a>Active Directory 复制吞吐量改进  
  
### <a name="overview"></a>概述  
广告复制使用其复制传输 RPC。 默认情况下，RPC 使用 8k 传输缓冲区，以及 5 K 数据包大小。 这效果网络发送实例将传输三个数据包 (大约 15 K 值得的数据)，然后必须发送更多之前往返等待的网络位置。 假设 3ms 往返时间的最大的吞吐量会周围 40Mbps，即使在 1Gbps 或 10 Gbps 网络上。  
  
> [!NOTE]  
> -   此更新调整到大约 600 Mbps 40Mbps 最大广告复制吞吐量。  
>   
>     -   它会增加减少的网络的 RPC 发送缓冲区大小往返  
> -   将最高速度，显著影响高延迟的网络。  
  
此更新增加最大的吞吐量，大约 600 Mbps 到改为 RPC 发送缓冲区大小从 8k 256 KB。  此更改可使 TCP 窗口大小增长超过 8k，减少了网络往返行程。  
  
> [!NOTE]  
> 没有可配置设置用于解决此问题。  
  
### <a name="additional-resources"></a>更多资源  
[Active Directory 复制型号的工作原理](https://technet.microsoft.com/library/cc772726(v=WS.10).aspx)  
  


