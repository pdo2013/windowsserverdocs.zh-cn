---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: SPN 和 UPN 唯一性
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c9a769fdd9fb7d13c47da465b25bc59e7f55237f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856738"
---
# <a name="spn-and-upn-uniqueness"></a>SPN 和 UPN 唯一性

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
域控制器运行 Windows Server 2012 R2 块创建重复的服务主体名称 (SPN) 和用户主体名称 (UPN)。 这包括如果还原或恢复已删除的对象的重命名会导致重复。  
  
### <a name="background"></a>后台  
重复服务主体名称 (SPN) 通常会出现并导致身份验证失败和 LSASS CPU 使用率过高可能导致。 没有内置方法来阻止添加重复的 SPN 或 UPN。 *  
  
重复 UPN 值中断同步之间的本地 AD 和 Office 365。  
  
*Setspn.exe 通常用于创建新 Spn 和功能上内置于随还会检查有重复项的 Windows Server 2008 一起发布的版本。  
  
**表 SEQ 表\\ \* ARABIC 1:UPN 和 SPN 唯一性**  
  
|功能|备注|  
|-----------|-----------|  
|UPN 唯一性|重复 Upn 中断同步本地 AD 帐户与 Windows Azure 基于 AD 的 Office 365 等服务。|  
|SPN 唯一性|Kerberos 相互身份验证需要 Spn。  重复的 Spn 导致身份验证失败。|  
  
关于 Upn 和 Spn 的唯一性要求的详细信息，请参阅[唯一性约束](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>症状  
错误代码 8467 或 8468 或其十六进制表示符号或等效的字符串记录在各种屏幕上经过和在目录服务事件日志中的事件 ID 2974 中。 只有在以下情况下，尝试创建重复的 UPN 或 SPN 将被阻止：  
  
-   写入处理的 Windows Server 2012 R2 DC  
  
**表 SEQ 表\\ \* ARABIC 2:UPN 和 SPN 唯一性错误代码**  
  
|Decimal|Hex|符号|字符串|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为添加/修改为提供的 SPN 值不是唯一的全林性。|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为添加/修改为提供的 UPN 值不是唯一的全林性。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果 UPN 不是唯一的新用户创建失败  
  
### <a name="dsamsc"></a>DSA.msc  
您已选择的用户登录名已在该企业中使用。 请选择另一个登录名，并再试一次。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改现有的帐户：  
  
在企业中已存在指定的用户登录名。 指定一个新的活动，通过更改的前缀或从列表中选择不同的后缀。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心 (DSAC.exe)  
尝试在已存在的 UPN 与 Active Directory 管理中心中创建新用户会产生以下错误。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**图 SEQ Figure \\ \* ARABIC 1 当由于重复 UPN 的新用户创建失败时显示在 AD 管理中心中的错误**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>事件 2974年源：ActiveDirectory_DomainService  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**图 SEQ Figure \\ \* ARABIC 2 事件 ID 2974 错误 8648**  
  
事件 2974年列出了已被阻止的值和一个或多个对象 （最多 10 个） 已包含此值的列表。  在下图中，您可以看到该 UPN 属性值***dhunt@blue.contoso.com***上四个其他对象已存在。  由于这是 Windows Server 2012 R2 中的新功能，在混合环境中的重复 UPN 和 Spn 意外创建仍将发生时向下级别 Dc 处理写入尝试。  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**图 SEQ Figure \\ \* ARABIC 3 事件 2974 显示所有对象包含重复 UPN**  
  
> [!TIP]  
> 为定期查看事件 ID 2974s:  
>   
> -   识别以下企图创建重复 UPN 或 Spn  
> -   确定已包含重复项的对象  
  
8648 ="操作失败，因为添加/修改为提供的 UPN 值不是唯一的全林性。"  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe 已时使用了重复的 SPN 检测内置到 Windows Server 2008 发布以来 **"-S"** 选项。  可以通过使用重复的 SPN 检测绕过 **"-A"** 但是选项。  面向使用 SetSPN-A 选项使用 Windows Server 2012 R2 DC，则会阻止创建重复的 SPN。  显示的错误消息是一个使用-S 选项时显示相同的："重复的 SPN 发现，正在中止操作 ！"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**图 SEQ Figure \\ \*添加重复 UPN 被阻止时显示在 ADSIEdit ARABIC 4 错误消息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
从面向 Windows Server 2012 R2 DC Server 2012 PS 运行：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe 面向 Windows Server 2012 R2 DC 的 Windows Server 2012 上运行：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**图 SEQ Figure \\ \* ARABIC 5 DSAC 用户创建错误上非-Windows Server 2012 R2 同时面向 Windows Server 2012 R2 DC**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**图 SEQ Figure \\ \* ARABIC 6 DSAC 用户修改错误上非-Windows Server 2012 R2 同时面向 Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>一个对象，将导致重复 UPN 还原失败：  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
会记录任何事件，如果对象无法还原由于重复 UPN / SPN。  
  
对象的 UPN 必须唯一，使其还原。  
  
1.  标识存在于回收站中的对象的 UPN  
  
2.  确定具有相同的值的所有对象  
  
3.  删除重复 UPN(s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>标识已删除的 objectUsing repadmin.exe 上冲突的 UPN  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>若要使用相同的 UPN 标识的所有对象： 使用 Repadmin.exe  
  
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
> 先前未记录 **/ 删除**在 repadmin.exe 中的参数用于在结果集中包含删除的对象  
  
### <a name="using-global-search"></a>使用全局搜索  
  
-   打开 Active Directory 管理中心内，并导航到**全局搜索**  
  
-   选择**转换为 LDAP**单选按钮  
  
-   Type **(userPrincipalName=*ConflictingUPN*)**  
  
    -   替换***ConflictingUPN***与实际发生冲突的 UPN  
  
-   选择**应用**  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果需要还原该对象，则将需要从其他对象中删除了重复 Upn。  对于只有一个对象，它非常简单，可以使用 ADSIEdit 来删除重复项。  如果有多个重复项的对象，Windows PowerShell 可能是更好的工具使用。  
  
为 null 出使用 Windows PowerShell 的 UserPrincipalName 属性：  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> UserPrincipalName 属性是单值属性，因此此过程只会删除重复的 UPN。  
  
### <a name="duplicate-spn"></a>重复的 SPN  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**图 SEQ Figure \\ \*添加重复的 SPN 被阻止时显示在 ADSIEdit 阿拉伯语 8 错误消息**  
  
在目录服务事件日志中记录**ActiveDirectory_DomainService**事件 ID **2974年**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 和 UPN 唯一性](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**图 SEQ Figure \\ \*阿拉伯语 9 错误记录时已阻止创建重复的 SPN**  
  
### <a name="workflow"></a>工作流  
  
-   **If DC == GC**  
  
    -   没有 offbox 调用需要，可以本地满足查询  
  
    -   ***UPN 用例***  
  
        -   查询本地全林性 UPN 索引，用于提供 UPN (*userPrincipalName; 全局索引*)  
  
            -   如果返回项 = = 0-> 将继续写入  
  
            -   如果返回项 ！ = 0-> 写入失败  
  
                -   记录事件  
  
                -   此外返回扩展的错误：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 用例***  
  
        -   查询本地全林性 SPN 索引，提供 spn (*servicePrincipalName; 全局索引*)  
  
            -   如果返回项 = = 0-> 将继续写入  
  
            -   如果返回项 ！ = 0-> 写入失败  
  
                -   记录事件  
  
                -   此外返回扩展的错误：  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC != GC**  
  
    -   Offbox 调用**理想**但不是那么重要，即这是最大努力唯一性检查  
  
        -   检查才会继续针对本地 DIT 仅当 GC 找不到  
  
        -   记录用于指示此类事件  
  
    -   ***UPN 用例***  
  
        -   提交对最接近的 GC 的 LDAP 查询？ 查询 GC 的全林性 UPN 索引提供的 UPN (*userPrincipalName; 全局索引*)  
  
            -   如果返回项 = = 0-> 将继续写入  
  
            -   如果返回项 ！ = 0-> 写入失败  
  
                -   记录事件  
  
                -   此外返回扩展的错误：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 用例***  
  
        -   提交对最接近的 GC 的 LDAP 查询？ 提供的 spn 查询 GC 的全林性 SPN 索引 (*servicePrincipalName; 全局索引*)  
  
            -   如果返回项 = = 0-> 将继续写入  
  
            -   如果返回项 ！ = 0-> 写入失败  
  
                -   记录事件  
  
                -   此外返回扩展的错误：  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
重新激活已删除的对象时，存在的 SPN 或 UPN 值检查的唯一性。 如果发现重复项时，则请求将失败。  
  
-   对于 DNS 主机名，SAM 帐户名称等，如某些属性的更改时进行修改，Spn 都会相应更新。 在过程中，会删除已过时的 Spn，新 Spn 了构造和添加到数据库。 是对其触发此路径的必要属性修改：  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果任何新的 SPN 值重复，我们使修改失败。 上述列表中的重要属性是 ATT_DNS_HOST_NAME （计算机名称） 和 ATT_SAM_ACCOUNT_NAME （SAM 帐户名）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>请尝试此操作：探索 SPN 和 UPN 唯一性  
这是多个第一个"**尝试此**"模块中的活动。  不是此模块的单独的实验室指南。  **尝试此**活动是实质上是允许你浏览在实验室环境中的课程材料的自由的活动。  可以选择以下提示，还是要关闭脚本，并附带自己的活动。  
  
> [!NOTE]  
> -   这是多个第一个"**尝试此**"活动。  
> -   不是此模块的单独的实验室指南。  
> -   **尝试此**活动是实质上是允许你浏览在实验室环境中的课程材料的自由的活动。  
> -   可以选择以下提示，还是要关闭脚本，并附带自己的活动。  
> -   而不是所有的部分介绍了**尝试此**提示符下，您是仍然鼓励浏览课程内容在实验室中的，在适当的位置。  
  
尝试使用 SPN 和 UPN 唯一性。  遵循这些提示，或你自己完成。  
  
1.  使用 UPN 创建新用户  
  
2.  创建帐户并 Spn  
  
3.  使用以前已定义的 UPN 中创建新用户，或者更改现有帐户的 UPN。  执行该操作在另一个帐户的 SPN  
  
    1.  填充具有 UPN 已在使用现有的用户帐户  
  
        1.  使用 PowerShell、 ADSIEDIT 或 Active Directory 管理中心 (DSAC.exe)  
  
    2.  填充将现有帐户与已在使用 SPN  
  
        1.  使用 Windows PowerShell、 ADSIEDIT 或 SetSPN  
  
4.  观察错误  
  
**（可选)**  
  
1.  验证与课堂讲师，它是确定以启用 *[AD 回收站](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* 在 Active Directory 管理中心内。  如果是这样，移动到下一步。  
  
2.  填充用户帐户的 UPN  
  
3.  删除帐户  
  
4.  填充使用相同的 UPN 为已删除的帐户不同的帐户  
  
5.  尝试使用回收 Bin GUI 来恢复此帐户  
  
6.  假设您只需提供与您在上一步中看到的错误。  （和没有刚执行的步骤的历史记录）你的目标是完成还原的帐户。  请参阅该工作簿例如步骤。  
  


