---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Active Directory 中的附录 C-受保护的帐户和组
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 606b3a42d70ee5c2a3479f9c9df2f95a495d6afd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408721"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 C：Active Directory 中的受保护帐户和组

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 C：Active Directory 中的受保护帐户和组

在 Active Directory 中，会将一组高度特权的帐户和组视为受保护的帐户和组。 使用 Active Directory 中的大多数对象，委派的管理员（已被委派管理 Active Directory 对象的权限的用户）可以更改对象的权限，包括更改权限以允许自身更改成员身份例如，组。  

但是，对于受保护的帐户和组，通过自动过程设置和强制实施对象的权限，从而确保对象的权限保持一致，即使对象已移动到目录也是如此。 即使有人手动更改了某个受保护对象的权限，此过程也可以确保将权限快速返回到默认值。  

### <a name="protected-groups"></a>受保护组

下表包含域控制器操作系统列出的 Active Directory 中的受保护组。  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>操作系统 Active Directory 中的受保护帐户和组

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012、 <br> Windows Server 2008 R2， <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|管理员|管理员|管理员|管理员|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|域控制器|域控制器|域控制器|域控制器|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||企业密钥管理员|
||||密钥管理员|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|打印操作员|打印操作员|打印操作员|打印操作员|
|||只读域控制器|只读域控制器|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

AdminSDHolder 对象的目的是为域中的受保护帐户和组提供 "模板" 权限。 AdminSDHolder 在每个 Active Directory 域的系统容器中自动创建为对象。 其路径为：**CN = AdminSDHolder，CN = System，DC = < domain_component >，DC = < domain_component >？。**  

与管理员组拥有的 Active Directory 域中的大多数对象不同，AdminSDHolder 由域管理员组拥有。 默认情况下，EAs 可以更改任何域的 AdminSDHolder 对象，这与域的 Domain Admins 和 Administrators 组相同。 此外，尽管 AdminSDHolder 的默认所有者是域的 Domain Admins 组，但 Administrators 或 Enterprise Admins 的成员可以获得对象的所有权。  

#### <a name="sdprop"></a>SDProp

SDProp 是在域控制器上每60分钟运行一次（默认情况下），该进程持有域的 PDC 模拟器（PDCE）。 SDProp 将域的 AdminSDHolder 对象上的权限与域中的受保护帐户和组的权限进行比较。 如果对任何受保护的帐户和组的权限与 AdminSDHolder 对象上的权限不匹配，则会重置受保护帐户和组的权限，使其与域的 AdminSDHolder 对象的权限相匹配。  

此外，在受保护的组和帐户上禁用权限继承，这意味着，即使将帐户和组移到目录中的不同位置，它们也不会继承其新父对象的权限。 对 AdminSDHolder 对象禁用了继承，因此对父对象的权限更改不会更改 AdminSDHolder 的权限。  

##### <a name="changing-sdprop-interval"></a>更改 SDProp 间隔

通常，不需要更改 SDProp 运行的时间间隔，因为测试目的除外。 如果需要更改域的 PDCE 上的 SDProp 间隔，请使用 regedit 在 Hklm\system\currentcontrolset\services\ntds\parameters 中添加或修改 AdminSDProtectFrequency DWORD 值  

值的范围是从60到7200（1到2小时）之间的秒数。 若要撤消更改，请删除 AdminSDProtectFrequency 项，这将导致 SDProp 恢复到60分钟间隔。 通常不应在生产域中减少此时间间隔，因为这会增加域控制器上的 LSASS 处理开销。 这一增加的影响取决于域中受保护对象的数量。  

##### <a name="running-sdprop-manually"></a>手动运行 SDProp

测试 AdminSDHolder 更改的更好方法是手动运行 SDProp，这会导致任务立即运行，但不会影响计划的执行。 对于运行 Windows Server 2008 和更早版本的域控制器，在运行 Windows Server 2012 或 Windows Server 2008 R2 的域控制器上，手动运行 SDProp 的执行方式略有不同。  

[Microsoft 支持部门文章 251343](https://support.microsoft.com/kb/251343)中提供了在较早版本的操作系统上手动运行 SDProp 的过程，以下是针对较旧和较新操作系统的分步说明。 在任一情况下，都必须连接到 Active Directory 中的 rootDSE 对象，并使用 rootDSE 对象的 null DN 执行修改操作，并将操作的名称指定为要修改的属性。 有关 rootDSE 对象上的可修改操作的详细信息，请参阅 MSDN 网站上的[RootDSE 修改操作](https://msdn.microsoft.com/library/cc223297.aspx)。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或更早版本中手动运行 SDProp

可以通过使用 Ldp.exe 或通过运行 LDAP 修改脚本强制 SDProp 运行。 若要使用 Ldp.exe 运行 SDProp，请在更改域中的 AdminSDHolder 对象之后执行以下步骤：  

1. 启动**ldp.exe**。  
2. 单击 "Ldp" 对话框上的 "**连接**"，然后单击 "**连接**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. 在 "**连接**" 对话框中，键入持有 PDC 模拟器（PDCE）角色的域的域控制器的名称，然后单击 **"确定"** 。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. 验证是否已成功连接，如 **Dn 所示：（RootDSE）**  在以下屏幕截图中，单击 "**连接**"，然后单击 "**绑定**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. 在 "**绑定**" 对话框中，键入有权修改 rootDSE 对象的用户帐户的凭据。 （如果以该用户身份登录，则可以选择 "**绑定为**当前已登录的用户"。）单击“确定”。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. 完成绑定操作后，单击 "**浏览**"，然后单击 "**修改**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. 在 "**修改**" 对话框中，将**DN**字段留空。 在 "**编辑条目属性**" 字段中，键入**FixUpInheritance**，然后在 "**值**" 字段中键入**Yes**。 单击**Enter**填充**条目列表**，如以下屏幕截图所示。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. 在 "填充的修改" 对话框中，单击 "运行"，然后验证对 AdminSDHolder 对象所做的更改是否已出现在该对象上。  

> [!NOTE]  
> 有关修改 AdminSDHolder 以允许指定的无特权帐户修改受保护组的成员身份的信息，请参阅 [Appendix I：为 Active Directory @ no__t 中的受保护帐户和组创建管理帐户。  

如果希望通过 LDIFDE 或脚本手动运行 SDProp，可以按如下所示创建修改项：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 中手动运行 SDProp

还可以通过使用 Ldp.exe 或通过运行 LDAP 修改脚本强制 SDProp 运行。 若要使用 Ldp.exe 运行 SDProp，请在更改域中的 AdminSDHolder 对象之后执行以下步骤：  

1. 启动**ldp.exe**。  

2. 在 " **Ldp** " 对话框中，单击 "**连接**"，然后单击 "**连接**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. 在 "**连接**" 对话框中，键入持有 PDC 模拟器（PDCE）角色的域的域控制器的名称，然后单击 **"确定"** 。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. 验证是否已成功连接，如 **Dn 所示：（RootDSE）**  在以下屏幕截图中，单击 "**连接**"，然后单击 "**绑定**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. 在 "**绑定**" 对话框中，键入有权修改 rootDSE 对象的用户帐户的凭据。 （如果以该用户身份登录，则可以选择 "**绑定为当前已登录的用户**"。）单击“确定”。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. 完成绑定操作后，单击 "**浏览**"，然后单击 "**修改**"。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. 在 "**修改**" 对话框中，将**DN**字段留空。 在 "**编辑项属性**" 字段中，键入**RunProtectAdminGroupsTask**，然后在 "**值**" 字段中键入**1**。 单击 " **Enter** " 以填充条目列表，如下所示。  

   ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. 在 "填充的**修改**" 对话框中，单击 "**运行**"，然后验证对 AdminSDHolder 对象所做的更改是否已出现在该对象上。  

如果希望通过 LDIFDE 或脚本手动运行 SDProp，可以按如下所示创建修改项：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
