---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: "附录 C-受保护的帐户和 Active Directory 中的组"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 c：受保护的帐户和 Active Directory 中的组

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>附录 c：受保护的帐户和 Active Directory 中的组  
在 Active Directory，默认套高权限的帐户和组算是受保护的帐户和组。 大多数中 Active Directory 对象时，委派的管理员 （已委派的权限来管理 Active Directory 对象用户） 可以更改对象，包括更改权限，以允许更改会员身份组中，例如他们的权限。  

但是，受保护的帐户和组，对象的设置和权限强制通过自动流程，以确保的权限的对象保持一致，即使的对象有移动到的目录。 即使有人手动更改受保护的对象权限时，此过程将确保权限快速返回为默认值。  

### <a name="protected-groups"></a>组受保护  
下表包含了域控制器操作系统按照列出的 Active Directory 中受保护的组。  

**受保护的帐户和 Active Directory 由操作系统中的组**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008**|  
|管理员|帐户运营商|帐户运营商|帐户运营商|  
||管理员|管理员|管理员|  
||管理员|管理员|管理员|  
||备份运营商|备份运营商|备份运营商|  
||证书发布者|||  
|域管理员|域管理员|域管理员|域管理员|  
||域控制器|域控制器|域控制器|  
|企业管理员|企业管理员|企业管理员|企业管理员|  
||Krbtgt|Krbtgt|Krbtgt|  
||打印运营商|打印运营商|打印运营商|  
||||仅阅读域控制器|  
||复制程序|复制程序|复制程序|  
|方案管理员|方案管理员|方案管理员|方案管理员|  
||服务器运营商|服务器运营商|服务器运营商|  

#### <a name="adminsdholder"></a>AdminSDHolder  
AdminSDHolder 对象旨在提供"模板"权限的受保护的帐户和域中的组。 作为对象系统容器的每个 Active Directory 域中，会自动创建 AdminSDHolder。 它的路径是： **CN = AdminSDHolder，CN = 系统特区 = < domain_component >，直流 = < domain_component >？。**  

与不同的 Active Directory 的域中所拥有的管理员组，大多数对象 AdminSDHolder 归域管理员组。 默认情况下，EAs 可以更改任何域 AdminSDHolder 对象，因为可以的域域管理员和管理员组。 此外，尽管默认所有者 AdminSDHolder 的是域域管理员组，但成员管理员或企业管理员可以的所有权对象。  

#### <a name="sdprop"></a>SDProp  
SDProp 已保存的域 PDC 仿真器 (PDCE) 的域控制器运行 （默认） 每隔 60 分钟的进程。 SDProp 比较上的受保护的帐户和域中的组权限的域 AdminSDHolder 对象的权限。 如果不匹配的受保护的帐户和组任何权限 AdminSDHolder 对象的权限，受保护的帐户和组权限被重置匹配的域的 AdminSDHolder 对象。  

此外，权限继承处于禁用状态受保护的组和帐户，这意味着，即使帐户和组移到的目录中的其他位置时，它们执行操作不文件沿用文件权限从其新家长对象。 继承 AdminSDHolder 对象中处于禁用状态，以便为家长对象的权限变化不会影响 AdminSDHolder 的权限。  

##### <a name="changing-sdprop-interval"></a>更改 SDProp 的时间间隔。  
通常情况下，不需要的时间间隔哪些 SDProp 运行，除外测试目的的更改。 如果你需要更改 SDProp 间隔，请在域中，PDCE 使用 regedit 添加或修改中 HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters AdminSDProtectFrequency DWORD 值。  

值各种处于 7200 （两个小时的一分钟） 为 60 秒。 可反转所做的更改，请删除 AdminSDProtectFrequency 密钥，将导致 SDProp 还原回 60 分钟的时间间隔。 你通常应不减少此间隔生产域中因为它会增加 LSASS 域控制器上的头顶上飞过时处理。 这种增加的影响介绍取决于域中的受保护对象数量。  

##### <a name="running-sdprop-manually"></a>手动运行 SDProp  
测试 AdminSDHolder 更改为更好的方法是 SDProp 手动运行，这使任务立即运行，但不会影响计划的执行。 手动运行 SDProp 是运行 Windows Server 2008 域控制器上的略有不同执行和运行 Windows Server 2012 或 Windows Server 2008 R2 域控制器上比早期。  

对于运行较旧操作系统手动 SDProp 过程中提供了[Microsoft 支持文章 251343](https://support.microsoft.com/kb/251343)，，以下是及更高版本较旧操作系统的分步说明进行操作。 不论采取何种情况，你必须连接到 rootDSE 对象 Active Directory 中，并执行与空 DN 修改操作 rootDSE 对象，指定为到修改特性操作的名称。 有关可修改 rootDSE 对象操作的详细信息，请参阅[rootDSE 修改操作](https://msdn.microsoft.com/library/cc223297.aspx)MSDN 网站上。  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>在 Windows Server 2008 或更早版本中手动运行 SDProp  
你可以强制 SDProp 运行通过使用 Ldp.exe 或运行 LDAP 修改脚本。 若要运行 SDProp 使用 Ldp.exe，请执行以下步骤后所做的更改到某个域中 AdminSDHolder 对象：  

1.  启动**Ldp.exe**。  

2.  单击**连接**上 Ldp 对话框中，然后单击**连接**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  在**连接**对话框中，键入拥有 PDC 仿真器 (PDCE) 角色域域控制器的名称，然后单击**确定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  验证你已连接的指示成功**Dn: (RootDSE)**在下面的屏幕截图，请单击**连接**单击**绑定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  在**绑定**对话框中，键入已修改 rootDSE 对象权限的用户帐户的凭据。 (如果你的用户身份登录，你可以选择**作为绑定**当前登录的用户。)单击**确定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  你已完成绑定操作后，单击**浏览**，然后单击**修改**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  在**修改**对话框中，保留**DN**字段留空。 在**编辑条目特性**字段中，键入**FixUpInheritance**，并在**值**字段中，键入**是**。 单击**Enter**填充**列表入口**下面的屏幕截图中所示。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  在较密集修改对话框中，单击运行，并验证拥有该对象上出现了 AdminSDHolder 对象到所做的更改。  

> [!NOTE]  
> 有关修改 AdminSDHolder 允许指定未经授权的客户修改受保护的组成员的信息，请参阅[附录 i： 创建管理的帐户中 Active Directory 的受保护的帐户和组](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

如果你想要通过 LDIFDE 或脚本手动运行 SDProp，你可以创建一个修改条目，如下所示：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>在 Windows Server 2012 或 Windows Server 2008 R2 手动运行 SDProp  
你也可以强制 SDProp 运行通过使用 Ldp.exe 或运行 LDAP 修改脚本。 若要运行 SDProp 使用 Ldp.exe，请执行以下步骤后所做的更改到某个域中 AdminSDHolder 对象：  

1.  启动**Ldp.exe**。  

2.  在**Ldp**对话框中，单击**连接**，然后单击**连接**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  在**连接**对话框中，键入拥有 PDC 仿真器 (PDCE) 角色域域控制器的名称，然后单击**确定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  验证你已连接的指示成功**Dn: (RootDSE)**在下面的屏幕截图，请单击**连接**单击**绑定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  在**绑定**对话框中，键入已修改 rootDSE 对象权限的用户帐户的凭据。 (如果你的用户身份登录，你可以选择**绑定当前登录的用户**。)单击**确定**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  你已完成绑定操作后，单击**浏览**，然后单击**修改**。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  在**修改**对话框中，保留**DN**字段留空。 在**编辑条目特性**字段中，键入**RunProtectAdminGroupsTask**，并在**值**字段中，键入**1**。 单击**Enter**填充列表入口如下所示。  

    ![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  在较密集**修改**对话框中，单击**运行**，并验证是否拥有该对象上出现了 AdminSDHolder 对象到所做的更改。  

如果你想要通过 LDIFDE 或脚本手动运行 SDProp，你可以创建一个修改条目，如下所示：  

![受保护的帐户和组](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
