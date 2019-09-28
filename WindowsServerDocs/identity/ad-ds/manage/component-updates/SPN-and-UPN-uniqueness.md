---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: SPN 和 UPN 唯一性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ded707276471fccd28f0ec17afef0a24015ff32f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390026"
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 唯一性

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

**作者**：Justin Turner，具有 Windows 组的高级支持升级工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
运行 Windows Server 2012 R2 的域控制器阻止创建重复的服务主体名称（SPN）和用户主体名称（UPN）。 这包括还原或恢复已删除的对象或重命名对象是否会导致重复。  
  
### <a name="background"></a>后台  
通常会出现重复的服务主体名称（SPN），并会导致身份验证失败，并可能导致过多的 LSASS CPU 使用率。 没有用于阻止添加重复 SPN 或 UPN 的内置方法。 *  
  
重复的 UPN 值中断本地 AD 与 Office 365 之间的同步。  
  
\* Setspn 通常用于创建新的 Spn，而功能已内置于随 Windows Server 2008 一起发布的版本中，用于添加对重复项的检查。  
  
**Table SEQ Table \\ @ no__t-2 阿拉伯1：UPN 和 SPN 的唯一性 @ no__t-0  
  
|功能|注释|  
|-----------|-----------|  
|UPN 唯一性|与基于 Windows Azure AD 的服务（如 Office 365）进行的本地 AD 帐户重复 Upn 中断同步。|  
|SPN 唯一性|Kerberos 需要用于相互身份验证的 Spn。  重复 Spn 导致身份验证失败。|  
  
有关 Upn 和 Spn 的唯一性要求的详细信息，请参阅[唯一性约束](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>症状  
错误代码8467或8468，或者其十六进制、符号或字符串等效项记录在目录服务事件日志中的各种屏幕上，事件 ID 为2974。 仅在以下情况下，才会阻止创建重复 UPN 或 SPN 的尝试：  
  
-   写入由 Windows Server 2012 R2 DC 处理  
  
**Table SEQ Table \\ @ no__t-2 阿拉伯2：UPN 和 SPN 唯一错误代码 @ no__t-0  
  
|Decimal|Hex|符号|字符串|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为提供的用于添加/修改的 SPN 值在林范围内不唯一。|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为提供的用于添加/修改的 UPN 值在林范围内不唯一。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果 UPN 不唯一，则新用户创建失败  
  
### <a name="dsamsc"></a>DSA.MSC  
你选择的用户登录名已在此企业中使用。 请选择另一个登录名，然后重试。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改现有帐户：  
  
企业中已存在指定的用户登录名。 可以通过更改前缀或从列表中选择其他后缀来指定一个新的后缀。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心（DSAC.EXE）  
尝试使用已存在的 UPN 在 Active Directory 管理中心中创建新用户将产生以下错误。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯语1当新用户创建因 UPN 而失败时，AD 管理中心中显示错误**  
  
### <a name="event-2974-source-activedirectory_domainservice"></a>事件2974源：ActiveDirectory_DomainService  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯2事件 ID 2974，出现错误8648**  
  
事件2974列出了已阻止的值以及一个或多个对象（最多10个）已包含该值的列表。  在下图中，可以看到 UPN 属性值 **<em>dhunt@blue.contoso.com</em>** 在其他四个对象上已存在。  由于这是 Windows Server 2012 R2 中的一项新功能，因此当下层 Dc 处理写入尝试时，在混合环境中意外创建重复的 UPN 和 Spn 仍会发生。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯3事件2974显示包含重复 UPN 的所有对象**  
  
> [!TIP]  
> 定期查看事件 ID 2974s：  
>   
> -   标识尝试创建重复的 UPN 或 Spn  
> -   标识已经包含重复项的对象  
  
8648 = "操作失败，因为提供的用于添加/修改的 UPN 值不是唯一的全林性。"  
  
### <a name="setspn"></a>SetSPN  
由于在 Windows Server 2008 版本中使用 **"-S"** 选项，Setspn 已内置了重复的 SPN 检测。  不过，您可以使用 **"-A"** 选项跳过重复的 SPN 检测。  在将 Windows Server 2012 R2 DC 与-A 选项一起使用时，将阻止创建重复的 SPN。  显示的错误消息与使用-S 选项时显示的错误消息相同："发现了重复的 SPN，正在中止操作！"  
  
### <a name="adsiedit"></a>ADSIEDIT  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**图 SEQ 图 @no__t 在添加重复 UPN 时，ADSIEdit 中显示的-1 @ no__t-2 阿拉伯4错误消息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
从面向 Windows Server 2012 R2 DC 的服务器2012运行的 PS：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
在 Windows Server 2012 上运行的 DSAC.EXE，面向 Windows Server 2012 R2 DC：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯 5 DSAC.EXE 用户在非 Windows Server 2012 R2 上创建错误，同时面向 Windows Server 2012 R2 DC**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯 6 DSAC.EXE 用户修改在非 Windows Server 2012 R2 上，同时面向 Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>还原会导致重复 UPN 的对象失败：  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
由于 UPN/SPN 重复而无法还原对象时，不会记录任何事件。  
  
对象的 UPN 必须是唯一的，才能还原。  
  
1.  标识回收站中对象上存在的 UPN  
  
2.  标识具有相同值的所有对象  
  
3.  删除重复的 UPN  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>标识已删除的 objectUsing repadmin 上的冲突 UPN  
  
```  
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname  
```  
  
```  
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname  
  
C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object  
s,DC=blue,DC=contoso,DC=com  
    1> userPrincipalName: dhunt@blue.contoso.com  
```  
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>标识具有相同 UPN 的所有对象：使用 Repadmin  
  
```  
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
  
C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com  
```  
  
> [!TIP]  
> Repadmin 中以前未记录的 **/deleted**参数用于在结果集中包含已删除的对象  
  
### <a name="using-global-search"></a>使用全局搜索  
  
-   打开 Active Directory 管理中心并导航到 "**全局搜索**"  
  
-   选择 "**转换为 LDAP** " 单选按钮  
  
-   类型 **（userPrincipalName =*ConflictingUPN*）**  
  
    -   将***ConflictingUPN***替换为发生冲突的实际 UPN  
  
-   选择**应用**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果需要还原对象，则需要从其他对象中删除重复的 Upn。  对于一个对象，只需使用 ADSIEdit 即可删除重复项。  如果有多个对象具有重复项，则 Windows PowerShell 可能是更好的工具。  
  
使用 Windows PowerShell 为 "UserPrincipalName" 属性 null：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> UserPrincipalName 属性是单值属性，因此此过程只会删除重复的 UPN。  
  
### <a name="duplicate-spn"></a>重复 SPN  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**图 SEQ 图 @no__t 在添加重复 SPN 时，ADSIEdit 中显示的-1 @ no__t-2 阿拉伯8错误消息**  
  
记录在目录服务事件日志中是**ActiveDirectory_DomainService**事件 ID **2974**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**图 SEQ 图 \\ @ no__t-2 阿拉伯9在创建重复 SPN 时记录的错误**  
  
### <a name="workflow"></a>工作流  
  
-   **如果 DC = = GC**  
  
    -   不需要 offbox 调用，可以在本地满足查询  
  
    -   ***UPN 大小写***  
  
        -   查询所提供 UPN 的本地林范围 UPN 索引（*userPrincipalName; 全局索引*）  
  
            -   如果返回的条目 = = 0，则 > 写入继续  
  
            -   如果返回的条目！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   还会返回扩展错误：  
  
                    -   **8648：**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 大小写***  
  
        -   查询提供的 SPN 的本地林范围 SPN 索引（*servicePrincipalName; 全局索引*）  
  
            -   如果返回的条目 = = 0，则 > 写入继续  
  
            -   如果返回的条目！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   还会返回扩展错误：  
  
                    -   **8647：**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **如果 DC！ = GC**  
  
    -   Offbox 调用**需要**但并不重要，也就是说，这是一项尽力的唯一性检查  
  
        -   仅当找不到 GC 时，才检查本地 DIT  
  
        -   记录的事件，以指示  
  
    -   ***UPN 大小写***  
  
        -   针对最近的 GC 提交 LDAP 查询？ 查询 GC 的林范围 UPN 索引，提供 UPN （*userPrincipalName; 全局索引*）  
  
            -   如果返回的条目 = = 0，则 > 写入继续  
  
            -   如果返回的条目！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   还会返回扩展错误：  
  
                    -   **8648：**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 大小写***  
  
        -   针对最近的 GC 提交 LDAP 查询？ 查询 GC 的林范围 SPN 索引，用于提供的 SPN （*servicePrincipalName; 全局索引*）  
  
            -   如果返回的条目 = = 0，则 > 写入继续  
  
            -   如果返回的条目！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   还会返回扩展错误：  
  
                    -   **8647：**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
如果删除了已删除的对象，则将检查 SPN 或 UPN 值的唯一性。 如果找到重复的，则请求失败。  
  
-   对于某些属性更改，如 DNS 主机名、SAM 帐户名等，进行修改后，会相应地更新 Spn。 在此过程中，将删除过时的 Spn，并构造新的 Spn 并将其添加到数据库中。 触发此路径所需的必要属性修改如下：  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果有任何新的 SPN 值为重复值，我们将无法进行修改。 在上面的列表中，重要属性是 ATT_DNS_HOST_NAME （计算机名称）和 ATT_SAM_ACCOUNT_NAME （SAM 帐户名）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>请尝试执行以下操作：探索 SPN 和 UPN 唯一性  
这是模块中的几个 "**尝试此**" 活动的第一项。  此模块没有单独的实验室指南。  **试用此**活动本质上是自由格式的活动，可用于浏览实验室环境中的课程资料。  您可以选择执行提示或退出脚本，并提供您自己的活动。  
  
> [!NOTE]  
> -   这是多个 "**尝试此**" 活动中的第一个。  
> -   此模块没有单独的实验室指南。  
> -   **试用此**活动本质上是自由格式的活动，可用于浏览实验室环境中的课程资料。  
> -   您可以选择执行提示或退出脚本，并提供您自己的活动。  
> -   虽然并非所有部分都有**尝试使用此**提示，但仍建议您在实验室中浏览相应的课程内容。  
  
试验 SPN 和 UPN 的唯一性。  按照这些提示操作，或完成自己的提示。  
  
1.  用 UPN 创建新用户  
  
2.  创建具有 Spn 的帐户  
  
3.  使用之前定义的 UPN 创建新用户或更改现有帐户的 UPN。  为其他帐户上的 SPN 执行相同操作  
  
    1.  使用已在使用中的 UPN 填充现有用户帐户  
  
        1.  使用 PowerShell、ADSIEDIT 或 Active Directory 管理中心（DSAC.EXE）  
  
    2.  使用已在使用中的 SPN 填充现有帐户  
  
        1.  使用 Windows PowerShell、ADSIEDIT 或 SetSPN  
  
4.  观察错误  
  
**同时**  
  
1.  向教室讲师验证是否可以在 Active Directory 管理中心中启用 *[AD 回收站](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* 。  如果是这样，请转到下一步。  
  
2.  在用户帐户上填充 UPN  
  
3.  删除帐户  
  
4.  使用与已删除帐户相同的 UPN 填充不同的帐户  
  
5.  尝试使用回收站 GUI 还原帐户  
  
6.  假设您刚刚显示了上一步中显示的错误。  （并且没有您刚刚执行的步骤的历史记录）你的目标是完成帐户的还原。  有关示例步骤，请参阅工作簿。  
  


