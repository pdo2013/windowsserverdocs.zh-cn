---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: "SPN 并 UPN 唯一"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>SPN 并 UPN 唯一

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

**作者**：与 Windows 组索旋转器高级支持升级工程师  
  
> [!NOTE]  
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。  
  
## <a name="overview"></a>概述  
创建运行阻止 Windows Server 2012 R2 域控制器重复服务主要名称 (SPN)，并主体用户名 (UPN)。 这包括如果还原或删除的对象的恢复或重命名处为物体会导致副本。  
  
### <a name="background"></a>背景  
重复服务主体姓名 (SPN) 通常发生和导致身份验证失败并可能会导致过多 LSASS CPU 使用率。 没有阻止重复 SPN 或 UPN 加收件箱方法。 *  
  
重复 UPN 值中断本地之间同步广告和 Office 365。  
  
*Setspn.exe 常用创建新的 Spn 和功能已内置到将检查有重复添加的 Windows Server 2008 发布的版本。  
  
**表用 \\\ * 阿拉伯语 1: UPN 和 SPN 唯一**  
  
|功能|评论|  
|-----------|-----------|  
|UPN 唯一|重复 Upn break 同步的本地 AD 帐户与 Windows Azure AD 基于如 Office 365 服务。|  
|SPN 唯一|Kerberos 需要相互身份验证 Spn。  重复 Spn 身份验证失败的结果。|  
  
唯一要求 Upn 和 Spn 的详细信息，请参阅[唯一约束](https://msdn.microsoft.com/library/dn392337.aspx)。  
  
## <a name="symptoms"></a>症状  
错误代码 8467 或 8468 或其 hex，符号或字符串等效内容将记录在各种屏幕对话，在事件 ID 2974 目录服务事件日志中。 仅在以下情况下，尝试创建重复 UPN 或 SPN 将被阻止：  
  
-   由 Windows Server 2012 R2 DC 处理写入  
  
**表用 \\\ * 阿拉伯语 2: UPN 和 SPN 唯一错误代码**  
  
|小数点|Hex|符号|字符串|  
|-----------|-------|------------|----------|  
|8467|21C 7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为 SPN 值为加/修改提供不是唯一森林范围。|  
|8648|21C 8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|操作失败，因为 UPN 值为加/修改提供不是唯一森林范围。|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>如果不是唯一 UPN 创建新的用户无法正常工作  
  
### <a name="dsamsc"></a>DSA.msc  
选择你的用户登录名称已在此企业中使用。 选择其他登录名称，然后再试一次。  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
修改现有帐户：  
  
在企业中已存在用户名指定的登录。 指定一个新的活动，或者通过更改前缀从列表中选择不同的职务。  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Active Directory 管理中心 (DSAC.exe)  
尝试中已存在 UPN 使用 Active Directory 管理中心创建新的用户，以下错误结果。  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 1 错误时创建的新用户失败由于重复 UPN 显示在广告管理中心**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>事件 2974年源： ActiveDirectory_DomainService  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**确定 SEQ 图 \\\ * 错误 8648 阿拉伯语 2 事件 ID 2974**  
  
事件 2974年列出已阻止的值以及一个或多个对象 （最多 10) 已包含该值的列表。  在下图中，您可以看到该 UPN 特性值*** dhunt@blue.contoso.com ***已经存在的 4 个其他对象。  由于这是一项新功能在 Windows Server 2012 R2 时，意外重复 UPN 和 Spn 混合环境中仍然会创建时向低级别 Dc 处理写入尝试。  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 3 事件 2974 显示所有对象包含重复 UPN**  
  
> [!TIP]  
> 定期多查看事件 ID 2974s:  
>   
> -   找出尝试创建重复 UPN 或 Spn  
> -   标识已包含重复的对象  
  
8648 ="操作失败，因为 UPN 值为加/修改提供不是唯一树林。"  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe 已内置到该自从 Windows Server 2008 发布重复 SPN 检测使用时**"-S"**选项。  你可以通过使用绕过重复 SPN 检测**"的一个"**但是选项。  创建的重复 SPN 被阻止时定位 Windows Server 2012 R2 DC SetSPN 使用的选项。  显示的错误消息相当于使用-S 选项时显示的一个:"复制 SPN 找到，中止操作"！  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**确定 SEQ 图 \\\ * 添加了重复 UPN 被阻止时，ADSIEdit 中显示的阿拉伯语 4 错误消息**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
从 Server 2012 定位 Windows Server 2012 R2 DC 电源运行：  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
运行 Windows Server 2012 定位 Windows Server 2012 R2 DC DSAC.exe:  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 5 DSAC 用户创建错误上非-Windows Server 2012 R2 时定位 Windows Server 2012 R2 DC**  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 6 DSAC 用户修改错误上非-Windows Server 2012 R2 时定位 Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>还原处为物体重复 UPN 导致失败：  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
当对象失败时，由于重复 UPN 还原记录没有事件 / SPN。  
  
对象 UPN 必须唯一以使其还原。  
  
1.  找对象回收站中存在的 UPN  
  
2.  找出所有具有相同的值的对象  
  
3.  删除重复 UPN(s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>找出上已删除的 objectUsing repadmin.exe 冲突 UPN  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>若要确定相同 UPN 所有对象： 使用 Repadmin.exe  
  
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
> 未之前记录**/ 删除**中 repadmin.exe 参数结果集中包含已删除的对象  
  
### <a name="using-global-search"></a>使用全球搜索  
  
-   打开 Active Directory 管理中心，然后导航到**全局搜索**  
  
-   选择**转换为 LDAP**单选按钮  
  
-   键入* *(范围内 =*ConflictingUPN*) * *  
  
    -   更换***ConflictingUPN***通过有冲突实际 UPN  
  
-   选择**应用**  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>使用 Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
如果需要恢复对象，你将需要从的其他对象中删除了重复 Upn。  只有一个对象，是非常简单使用 ADSIEdit 要删除副本。  如果有多个副本对象，Windows PowerShell 可能要使用的更好的工具。  
  
为 null 出的范围内属性，使用 Windows PowerShell:  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> 范围内特性是单值的属性，以便此过程将仅删除重复 UPN。  
  
### <a name="duplicate-spn"></a>重复 SPN  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**确定 SEQ 图 \\\ * 添加了重复 SPN 被阻止时，ADSIEdit 中显示的阿拉伯语 8 错误消息**  
  
登录事件日志已目录服务**ActiveDirectory_DomainService**事件 ID **2974年**。  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![SPN 并 UPN 唯一](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 9 错误重复 SPN 的作品被阻止时登录**  
  
### <a name="workflow"></a>流  
  
-   **如果直流 = = GC**  
  
    -   所需没有 offbox 通话，可以本地满足查询  
  
    -   ***UPN 情况***  
  
        -   查询本地树林 UPN 索引提供 UPN (*范围内; 全球索引*)  
  
            -   如果条目返回 = = 0-> 写入收益  
  
            -   如果条目返回 ！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   也会返回扩展的错误：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 情况***  
  
        -   查询本地树林 SPN 索引提供 SPN (*servicePrincipalName; 全球索引*)  
  
            -   如果条目返回 = = 0-> 写入收益  
  
            -   如果条目返回 ！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   也会返回扩展的错误：  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **如果 DC ！ = GC**  
  
    -   Offbox 通话**理想**但不是非常重要，即这是最佳的唯一检查  
  
        -   检查继续对本地 DIT 仅当 GC 找不到  
  
        -   记录，以指示例如事件  
  
    -   ***UPN 情况***  
  
        -   提交 LDAP 查询针对最近 GC？ 提供的 UPN 查询 GC 的树林 UPN 索引 (*范围内; 全球索引*)  
  
            -   如果条目返回 = = 0-> 写入收益  
  
            -   如果条目返回 ！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   也会返回扩展的错误：  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***SPN 情况***  
  
        -   提交 LDAP 查询针对最近 GC？ 为提供 SPN 查询 GC 的树林 SPN 索引 (*servicePrincipalName; 全球索引*)  
  
            -   如果条目返回 = = 0-> 写入收益  
  
            -   如果条目返回 ！ = 0-> 写入失败  
  
                -   记录的事件  
  
                -   也会返回扩展的错误：  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
重新动画已删除的对象时，来检查唯一存在 SPN 或 UPN 值。 如果找到副本，则无法正常工作的请求。  
  
-   某些特性变化 DNS 主机名，三千帐户名称等，例如当进行修改，Spn 是相应更新。 在此过程中，将会删除过时 Spn 和是构建新 Spn 并将其添加到数据库中。 根据其触发此路径必要属性修改是：  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
如果任何新的 SPN 值是重复，我们无法修改。 上述列表中的重要属性是 ATT_DNS_HOST_NAME （计算机名称） 和 ATT_SAM_ACCOUNT_NAME （三千帐户名称）。  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>请尝试： 探索 SPN 并 UPN 唯一  
这是几个的第一个"**试试这个**"模块的活动。  没有为此模块单独的实验指南。  **试试这个**活动是本质上是将允许你探索实验环境中的课材料的自由形式活动。  您可以按照提示，或转脚本关闭的选项，并附带你自己的活动。  
  
> [!NOTE]  
> -   这是几个的第一个"**试试这个**"活动。  
> -   没有为此模块单独的实验指南。  
> -   **试试这个**活动是本质上是将允许你探索实验环境中的课材料的自由形式活动。  
> -   您可以按照提示，或转脚本关闭的选项，并附带你自己的活动。  
> -   并非所有部分都在时**试试这个**提示时，建议您的仍在适当探索在此实验课中的内容。  
  
试用 SPN 并 UPN 唯一。  按照这些提示，或者你自行完成。  
  
1.  使用 UPN 创建新的用户  
  
2.  使用 Spn 创建帐户  
  
3.  请使用已之前定义 UPN 创建新的用户，或者更改 UPN 现有帐户。  对于 SPN 上其他帐户执行相同的操作  
  
    1.  填充与已在使用 UPN 现有的用户帐户  
  
        1.  使用 PowerShell、 ADSIEDIT 或 Active Directory 管理中心 (DSAC.exe)  
  
    2.  填充与已在使用 SPN 现有帐户  
  
        1.  使用 Windows PowerShell、 ADSIEDIT 或 SetSPN  
  
4.  观察错误  
  
**（可选)**  
  
1.  验证教室教师与它是否确定要启用*[广告回收站](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* Active Directory 管理中心中。  如果是这样，移动到下一步。  
  
2.  填充的 UPN 用户帐户  
  
3.  删除帐户  
  
4.  填充的同一 UPN 为已删除的帐户的其他帐户  
  
5.  尝试使用回收站 Bin GUI 恢复帐户  
  
6.  假设你只需提供与您在上一步中看到此错误。  （，无需你只需执行这些步骤的历史记录）你的目标是完成帐户恢复。  请参阅例如步骤工作簿。  
  


