---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 附录 C-受保护帐户和 Active Directory 中的组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834698"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 C：受保护的帐户和 Active Directory 中的组

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 C：受保护的帐户和 Active Directory 中的组

Active Directory 中受保护的帐户和组被视为一组默认的高特权的帐户和组。 使用 Active Directory 中的大多数对象，委派的管理员 （已被委派管理 Active Directory 对象的权限的用户） 可以更改对象，包括更改权限以允许进行更改的成员身份的权限组，以便示例。  

但是，与受保护的帐户和组，选择对象权限设置和执行通过自动过程，以确保对该对象将保持一致，即使两个对象的权限移动目录。 即使有人手动更改受保护的对象的权限，此过程可确保权限快速返回到其默认值。  

### <a name="protected-groups"></a>受保护的组

下表包含在域控制器操作系统列出的 Active Directory 中的受保护的组。  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>由操作系统的 Active Directory 中受保护的帐户和组

| Windows Server 2003 RTM | Windows Server 2003 SP1+ | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|管理员|管理员|管理员|管理员|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|域控制器|域控制器|域控制器|域控制器|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||企业密钥管理|
||||密钥的管理员|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|打印操作员|打印操作员|打印操作员|打印操作员|
|||只读域控制器|只读域控制器|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

AdminSDHolder 对象的用途是提供对受保护的帐户和域中的组"模板"权限。 AdminSDHolder 自动创建为每个 Active Directory 域的系统容器中的对象。 其路径为：**CN=AdminSDHolder,CN=System,DC=<domain_component>,DC=<domain_component>?.**  

与大多数 Active Directory 域中对象所拥有的管理员组，不同 AdminSDHolder 归 Domain Admins 组。 默认情况下，EAs 可以进行更改到任何域的 AdminSDHolder 对象，如可能是域的 Domain Admins 和管理员组。 此外，尽管 AdminSDHolder 的默认所有者是域的 Domain Admins 组，但管理员或 Enterprise Admins 的成员可以执行对象的所有权。  

#### <a name="sdprop"></a>SDProp

SDProp 是保留域的 PDC 模拟器 (PDCE) 的域控制器运行 （默认） 每隔 60 分钟的进程。 SDProp 比较域的 AdminSDHolder 对象上的受保护的帐户和域中的组的权限的权限。 如果在任何受保护的帐户和组权限不匹配的 AdminSDHolder 对象上的权限，对受保护的帐户和组的权限会重置以匹配这些域的 AdminSDHolder 对象。  

此外，禁用了权限继承对受保护的组和帐户，这意味着，即使帐户和组都移到不同位置的目录中，他们不继承权限从其新的父对象。 AdminSDHolder 对象上禁用了继承，以便对父对象的权限更改不会更改 AdminSDHolder 的权限。  

##### <a name="changing-sdprop-interval"></a>更改 SDProp 间隔

通常情况下，不需要更改的时间间隔 SDProp 运行，除了测试目的。 如果你需要更改域，PDCE 的 SDProp 间隔，使用 regedit 添加或修改中 HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters 的 AdminSDProtectFrequency DWORD 值。  

值的范围是 7200 （两个小时到一分钟） 到 60 秒。 若要反转所做的更改，请删除 AdminSDProtectFrequency 密钥，这将导致 SDProp 若要还原到 60 分钟时间间隔。 你通常应不减少此时间间隔内生产域中因为这样可以提高 LSASS 的域控制器上的处理开销。 这种增长的影响是依赖于域中的受保护对象的数目。  

##### <a name="running-sdprop-manually"></a>手动运行 SDProp

测试 AdminSDHolder 更改的更好方法是运行 SDProp 手动，这将导致任务立即运行，但不会影响计划的执行。 手动运行 SDProp 是运行 Windows Server 2008 的域控制器上稍有不同的执行和而不是运行 Windows Server 2012 或 Windows Server 2008 R2 的域控制器，更早版本。  

中提供了过程为手动在较早的操作系统上运行 SDProp [Microsoft 支持文章 251343](https://support.microsoft.com/kb/251343)，和以下是较旧和较新操作系统的分步说明。 在任一情况下，您必须连接到 Active Directory 中的 rootDSE 对象并执行修改操作具有 null DN rootDSE 对象，该操作的名称指定为修改的属性。 有关对 rootDSE 对象的可修改操作的详细信息，请参阅[rootDSE 修改操作](https://msdn.microsoft.com/library/cc223297.aspx)MSDN 网站上。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或早期版本中手动运行 SDProp

您可以强制 SDProp 运行使用 Ldp.exe 或运行的是 LDAP 修改脚本。 若要运行 SDProp 使用 Ldp.exe，请执行以下步骤之后所做的更改到域中的 AdminSDHolder 对象,：  

1. 启动**Ldp.exe**。  
2. 单击**连接**Ldp 对话框，然后单击**Connect**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. 在中**Connect**对话框中，键入承载 PDC 模拟器 (PDCE) 角色的域的域控制器的名称，单击**确定**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. 验证是否已成功，由连接**Dn:(RootDSE)** 在以下屏幕截图中，单击**连接**然后单击**绑定**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. 在中**绑定**对话框框中，键入有权修改 rootDSE 对象的用户帐户的凭据。 (如果该用户的身份登录，则可以选择**绑定为**当前登录的用户。)单击“确定” 。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. 完成绑定操作后，单击**浏览**，然后单击**修改**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. 在中**修改**对话框中，保留**DN**字段留空。 在中**编辑条目属性**字段中，键入**FixUpInheritance**，然后在**值**字段中，键入**是**。 单击**Enter**来填充**条目列表**如以下屏幕截图中所示。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. 中填充的修改对话框中，单击运行，并验证对 AdminSDHolder 对象所做的更改已出现在该对象。  

> [!NOTE]  
> 有关修改 AdminSDHolder 若要允许指定无特权的帐户修改受保护组的成员身份的信息，请参阅[附录 i:创建管理帐户的受保护帐户和组在 Active Directory 中](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

如果想要通过 LDIFDE 或脚本手动运行 SDProp，则可以创建修改条目如下所示：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 中手动运行 SDProp

也可以强制 SDProp 运行使用 Ldp.exe 或运行的是 LDAP 修改脚本。 若要运行 SDProp 使用 Ldp.exe，请执行以下步骤之后所做的更改到域中的 AdminSDHolder 对象,：  

1. 启动**Ldp.exe**。  

2. 在中**Ldp**对话框中，单击**连接**，然后单击**Connect**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. 在中**Connect**对话框中，键入承载 PDC 模拟器 (PDCE) 角色的域的域控制器的名称，单击**确定**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. 验证是否已成功，由连接**Dn:(RootDSE)** 在以下屏幕截图中，单击**连接**然后单击**绑定**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. 在中**绑定**对话框框中，键入有权修改 rootDSE 对象的用户帐户的凭据。 (如果该用户的身份登录，则可以选择**为当前已登录用户绑定**。)单击“确定” 。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. 完成绑定操作后，单击**浏览**，然后单击**修改**。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. 在中**修改**对话框中，保留**DN**字段留空。 在中**编辑条目属性**字段中，键入**RunProtectAdminGroupsTask**，然后在**值**字段中，键入**1**。 单击**Enter**来填充条目列表，如下所示。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. 在填充**修改**对话框中，单击**运行**，并验证您对 AdminSDHolder 对象所做的更改已出现在该对象。  

如果想要通过 LDIFDE 或脚本手动运行 SDProp，则可以创建修改条目如下所示：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
