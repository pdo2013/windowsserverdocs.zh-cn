---
Title: 部署漫游用户配置文件
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 07/09/2018
ms.author: jgerend
ms.openlocfilehash: c662b8c44e3603ec972e06f3fb0ddbd55e1af904
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192718"
---
# <a name="deploying-roaming-user-profiles"></a>部署漫游用户配置文件

>适用于：Windows 10、 Windows 8.1，Windows 8、 Windows 7、 Windows Server 2019、 Windows Server 2016、 Windows Server （半年频道）、 Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2

本主题介绍如何使用 Windows Server 部署[漫游用户配置文件](folder-redirection-rup-overview.md)到 Windows 客户端计算机。 漫游用户配置文件将重定向用户配置文件的文件共享，以便用户接收相同的操作系统和多台计算机上的应用程序设置。

本主题最近更改的列表，请参阅[更改历史记录](#change-history)本主题的部分。

>[!IMPORTANT]
>由于中所做的安全性更改[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)，我们更新了[步骤 4:（可选） 为漫游用户配置文件创建 GPO](#step-4-optionally-create-a-gpo-for-roaming-user-profiles)本主题，因此该 Windows 可以正确应用漫游用户配置文件策略 （和恢复受影响的 Pc 上的本地策略为） 中。

> [!IMPORTANT]
>  用户开始自定义操作系统的就地升级在下面的配置后将会丢失：
> - 用户配置漫游配置文件
> - 允许用户对开始进行的更改
>
> 因此，开始菜单将重置为默认值为新的 OS 版本后操作系统就地升级。 有关解决方法，请参阅[附录 c:工作大约后重置开始菜单布局升级](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)。

## <a name="prerequisites"></a>先决条件

### <a name="hardware-requirements"></a>硬件要求

漫游用户配置文件需要基于 x64 或基于 x86 的计算机，则不支持该功能通过 Windows rt。

### <a name="software-requirements"></a>软件要求

漫游用户配置文件有以下软件要求：

- 如果要在具有现有本地用户配置文件的环境中部署带有文件夹重定向的漫游用户配置文件，则在部署漫游用户配置文件之前先部署文件夹重定向，以最大程度减小漫游配置文件。 在成功重定向现有的用户文件夹后，可部署漫游用户配置文件。
- 若要管理漫游用户配置文件，你必须以域管理员安全组、企业管理员安全组或组策略创建者所有者安全组成员身份登录。
- 客户端计算机必须运行 Windows 10、 Windows 8.1，Windows 8、 Windows 7、 Windows Vista、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008。
- 客户端计算机必须加入你管理的 Active Directory 域服务 (AD DS)。
- 必须提供一台安装了组策略管理和 Active Directory 管理中心的计算机。
- 必须提供文件服务器，才能托管漫游用户配置文件。

    - 如果文件共享使用 DFS 命名空间，则 DFS 文件夹（链接）必须具有单个目标，以防止用户在不同服务器上进行互相冲突的编辑。
    - 如果文件共享使用 DFS 复制与另一台服务器复制内容，则用户必须只能够访问源服务器，以防止用户在不同服务器上进行互相冲突的编辑。
    - 如果文件共享已群集化，则在文件共享上禁用连续可用性，以免出现性能问题。
- 若要使用主计算机支持漫游用户配置文件中，有其他客户端计算机和 Active Directory 架构要求。 有关详细信息，请参阅[部署文件夹重定向和漫游用户配置文件的主计算机](deploy-primary-computers.md)。
- 用户的开始菜单不会漫游 Windows 10、 Windows Server 2019 或 Windows Server 2016，如果他们正在使用多台 PC、 远程桌面会话主机或虚拟化桌面基础结构 (VDI) 服务器的布局。 作为一种解决方法，您可以指定开始布局，如本主题中所述。 也可使用的正确漫游开始菜单设置与远程桌面会话主机服务器或 VDI 服务器一起使用时的用户配置文件磁盘。 有关详细信息，请参阅[更轻松地与 Windows Server 2012 中的用户配置文件磁盘的用户数据管理](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/)。

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>在多个版本的 Windows 中使用漫游用户配置文件时的注意事项

如果你决定跨多个版本的 Windows 使用漫游用户配置文件，我们建议采取下列措施：

- 配置 Windows 来为每个操作系统版本维护单独的配置文件版本。 这有助于防止不必要和不可预知的问题（如配置文件损坏）。
- 使用文件夹重定向来将用户文件（如文档和图片）存储在用户配置文件之外。 这使用户可以跨不同操作系统版本使用相同文件。 它还能保持配置文件具有较小的大小并保持快速登录。
- 为漫游用户配置文件分配足够的存储空间。 如果支持两个操作系统版本，则配置文件的数目将增加一倍（因此占用了总空间），因为将为每个操作系统版本维护单独的配置文件。
- 不要在运行 Windows Vista/Windows Server 2008 和 Windows 7/Windows Server 2008 R2 的计算机之间使用漫游用户配置文件。 由于其配置文件版本不兼容，这些操作系统版本之间的漫游不支持。
- 请告知你的用户，在一个操作系统版本上进行的更改将不会漫游到另一个操作系统版本。
- 将你的环境移动到使用不同的配置文件版本的 Windows 的版本时 (例如，从 Windows 10 到 Windows 10，版本 1607年，请参阅[附录 b:配置文件版本参考信息](#appendix-b-profile-version-reference-information)列表)，用户会收到新的空漫游用户配置文件。 可以通过使用文件夹重定向将常见文件夹重定向获取新的配置文件的影响降至最低。 不存在的迁移漫游用户配置文件从一个配置文件版本到另一个受支持的方法。

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>第 1 步：支持使用单独的配置文件版本

如果您要运行 Windows 8.1，Windows 8、 Windows Server 2012 R2 或 Windows Server 2012 的计算机上部署漫游用户配置文件，我们建议对在部署之前在 Windows 环境进行一些更改。 这些更改有助于确保将来的操作系统升级顺利进行，并且有助于同时运行多个版本带有漫游用户配置文件的 Windows。

若要进行这些更改，请使用以下过程。

1. 在你要使用漫游、强制、超级强制或域的默认配置文件的所有计算机上，下载并安装相应的软件更新：

    - Windows 8.1 或 Windows Server 2012 R2： 安装软件更新文章中所述[2887595](http://support.microsoft.com/kb/2887595) Microsoft 知识库中 （如果已发布）。
    - Windows 8 或 Windows Server 2012：安装 Microsoft 知识库的文章 [2887239](http://support.microsoft.com/kb/2887239) 中所述的软件更新。
2. 在运行 Windows 8.1，Windows 8、 Windows Server 2012 R2 或 Windows Server 2012 将使用漫游用户配置文件的所有计算机上使用注册表编辑器或组策略来创建以下注册表项 DWORD 值，并将其设置为`1`。 有关使用组策略创建注册表项的信息，请参阅 [配置注册表项](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

    ```PowerShell
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    >[!WARNING]
    >不正确地编辑注册表可能会对系统造成严重损坏。 在更改注册表之前，应备份计算机上任何有价值的数据。
3. 重新启动计算机。

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>步骤 2：创建漫游用户配置文件安全组

如果你的环境尚未设置漫游用户配置文件，则第一步是创建一个安全组，其中包含你希望应用漫游用户配置文件策略设置的所有用户和/或计算机。

- 通用漫游用户配置文件部署的管理员通常创建用户的安全组。
- 远程桌面服务或虚拟化桌面部署的管理员通常使用用户和共享计算机的安全组。

下面介绍了如何为漫游用户配置文件创建一个安全组：

1. 打开服务器管理器的计算机上使用 Active Directory 管理中心安装。
2. 上**工具**菜单中，选择**Active Directory 管理中心**。 此时将出现 Active Directory 管理中心。
3. 右键单击相应的域或 OU 中，选择**新建**，然后选择**组**。
4. 在“创建组”  窗口的“组”  部分，指定以下设置：

    - 在“组名”  中，键入安全组的名称，例如：**漫游用户配置文件相关用户和计算机**。
    - 在中**组作用域**，选择**安全**，然后选择**全局**。
5. 在中**成员**部分中，选择**添加**。 此时将出现“选择用户、联系人、计算机、服务帐户或组”对话框。
6. 如果你想要将计算机帐户包括在安全组中，选择**对象类型**，选择**计算机**复选框，然后选择**确定**。
7. 键入用户/组或想要部署漫游用户配置文件中，选择的计算机的名称**确定**，然后选择**确定**试。

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>步骤 3:创建用于漫游用户配置文件的文件共享

如果还没有单独的文件共享为漫游用户配置文件 （独立于任何用于重定向的文件夹，以防止意外缓存漫游配置文件文件夹的共享），使用以下过程运行 Windows 的服务器上创建文件共享服务器。

>[!NOTE]
>某些功能可能会不同，也是不可用，具体取决于正在使用的 Windows Server 的版本。

下面介绍了如何在 Windows Server 上创建文件共享：

1. 在服务器管理器导航窗格中，选择**文件和存储服务**，然后选择**共享**以显示共享页。
2. 在共享磁贴中，选择**任务**，然后选择**新共享**。 此时将出现新建共享向导。
3. 上**选择配置文件**页上，选择**SMB 共享 – 快速**。 如果已安装的文件服务器资源管理器并且要使用文件夹管理属性，而是选择**SMB 共享 – 高级**。
4. 在“共享位置”  页上，选择你要在其上创建共享的服务器和卷。
5. 在“共享名”  页上，在“共享名”  框中键入共享的名称（例如， **用户配置文件$** ）。

    >[!TIP]
    >在创建共享时，通过在共享名后放置 ```$``` 来隐藏共享。 这会在普通浏览器中隐藏共享。
6. 在“其他设置”  页上，清除“启用连续可用性”  复选框（如果存在），并且可以选择性地选中“启用基于访问的枚举”  和“加密数据访问”  复选框。
7. 上**权限**页上，选择**自定义权限...** . 将出现高级安全设置对话框。
8. 选择**禁用继承**，然后选择**继承权限对此对象的显式权限转换为**。
9. 设置权限，如中所述[所需的权限在文件共享托管漫游用户配置文件](#required-permissions-for-the-file-share-hosting-roaming-user-profiles)并显示以下屏幕截图中，删除未列出的组和帐户、 权限和添加特殊对步骤 1 中创建的漫游用户配置文件用户和计算机组的权限。
    
    ![高级安全设置窗口，其中显示在表 1 中所述的权限](media\advanced-security-user-profiles.jpg)
    
    **图 1** 设置漫游用户配置文件共享的权限
10. 如果选择了“SMB 共享 – 高级”  配置文件，则在“管理属性”  页上，选择“用户文件”  文件夹使用值。
11. 如果选择了“SMB 共享 – 高级”  配置文件，则在“配额”  页上选择性地选择要应用于共享用户的配额。
12. 上**确认**页上，选择**创建。**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>针对文件共享托管漫游用户配置文件所需权限

|       |       |       |
|   -   |   -   |   -   |
| 用户帐户 | 访问 | 适用对象 |
|   系统    |  完全控制     |  此文件夹、子文件夹和文件     |
|  Administrators     |  完全控制     |  仅此文件夹     |
|  创建者/所有者     |  完全控制     |  仅子文件夹和文件     |
| 需要将数据放在共享中的用户（漫游用户配置文件用户和计算机）的安全组      |  列出文件夹 / 读取数据 *（高级权限）* <br />创建文件夹 / 附加数据 *（高级权限）* |  仅此文件夹     |
| 其他组和帐户   |  无（删除）     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>步骤 4：（可选）为漫游用户配置文件创建 GPO

如果尚未为漫游用户配置文件设置创建 GPO，请使用以下过程创建一个空白 GPO 以用于漫游用户配置文件。 此 GPO 允许你配置漫游用户配置文件设置（如主计算机支持，将对此进行单独讨论），并且还可用于在计算机上启用漫游用户配置文件，这与在虚拟化桌面环境中部署时或使用远程桌面服务部署时通常采用的做法一样。

下面介绍了如何为漫游用户配置文件创建 GPO:

1. 在装有组策略管理的计算机上打开服务器管理器。
2. 从**工具**菜单中选择**组策略管理**。 将出现“组策略管理”
3. 右键单击域或 OU 要在其中安装漫游用户配置文件，然后选择**在此域中创建 GPO 并在此处链接**。
4. 在中**新的 GPO**对话框中，键入 gpo 的名称 (例如，**漫游用户配置文件设置**)，然后选择**确定**。
5. 右键单击新创建的 GPO，然后清除“已启用链接”  复选框。 这将阻止应用该 GPO，直到已完成对它的配置为止。
6. 选中该 GPO。 中**安全筛选**一部分**作用域**选项卡上，选择**Authenticated Users**，然后选择**删除**以防止 GPO从正在应用到每个人。
7. 在中**安全筛选**部分中，选择**添加**。
8. 在中**选择用户、 计算机或组**对话框中，键入在步骤 1 中创建安全的名称组 (例如，**漫游用户配置文件用户和计算机**)，然后选择**确定**.
9. 选择**委派**选项卡上，选择**添加**，类型**Authenticated Users**，选择**确定**，然后选择**确定**再次接受默认值的读取权限。
    
    此步骤是必需由于中所做的安全性更改[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)。

>[!IMPORTANT]
>由于中所做的安全性更改[MS16 072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016)，现在必须提供对 GPO 的 Authenticated Users 组委派读取权限-否则 GPO 不会应用于用户，或如果已应用，则删除 GPO，将用户配置文件重定向回本地 PC。 有关详细信息，请参阅[部署组策略安全更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>步骤 5：（可选）在用户帐户上设置漫游用户配置文件

如果你要将漫游用户配置文件部署到用户帐户，请使用以下过程在 Active Directory 域服务中指定用于用户帐户的漫游用户配置文件。 如果您将漫游用户配置文件部署到计算机，如对于远程桌面服务或虚拟化桌面部署，而是使用中所述的过程，通常需要[步骤 6:（可选） 设置漫游用户配置文件在计算机上](#step-6-optionally-set-up-roaming-user-profiles-on-computers)。

>[!NOTE]
>如果你使用 Active Directory 在用户帐户上设置漫游用户配置文件，并使用组策略在计算机上进行设置，则基于计算机的策略设置优先级较高。

下面介绍了如何在用户帐户上设置漫游用户配置文件：

1. 在 Active Directory 管理中心中，导航到相应域中的“用户”  容器（或 OU）。
2. 选择要向其分配漫游用户配置文件，右键单击这些用户，然后选择的所有用户**属性**。
3. 在中**配置文件**部分中，选择**配置文件路径：** 复选框，然后输入路径的文件共享要存储用户的漫游用户配置文件后, 跟`%username%`（这是自动替换为在用户登录的用户名称第一个时间）。 例如：
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    若要指定强制漫游用户配置文件，请指定指向之前创建，例如的 NTuser.man 文件的路径`fs1.corp.contoso.comUser Profiles$default`。 有关详细信息，请参阅[创建强制用户配置文件](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
4. 选择“确定”  。

>[!NOTE]
>默认情况下，在使用漫游用户配置文件时，允许部署所有基于 Windows ® 运行时的（Windows 应用商店）应用。 但是，在使用特殊配置文件时，默认情况下将不部署应用。 特殊配置文件是在用户注销后放弃所做更改的用户配置文件：
><br><br>若要删除对特殊配置文件的应用部署的限制，请启用**允许特殊配置文件中的部署操作**策略设置（位于 Computer Configuration\Policies\Administrative Templates\Windows Components\App Package Deployment）。 但在此情况下，已部署的应用将在计算机上存储一些数据，这些数据可能会累积，例如一台计算机有数百位用户的情况。 若要清理应用，查找或开发工具，它使用[CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API 以进行清除的用户不再有配置文件的计算机上的应用程序包。
><br><br>有关 Windows 应用商店应用的其他背景信息，请参阅 [管理客户端对 Windows 应用商店的访问](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>)。

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>步骤 6：（可选）在计算机上设置漫游用户配置文件

如果你要将漫游用户配置文件部署到计算机（像通常对远程桌面服务或虚拟化桌面部署的做法一样），请使用以下过程。 如果要部署到用户帐户的漫游用户配置文件，而是使用中所述的过程[步骤 5:（可选） 在用户帐户上设置漫游用户配置文件](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)。

可以使用组策略将漫游用户配置文件应用于运行 Windows 8.1，Windows 8、 Windows 7、 Windows Vista、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的计算机。

>[!NOTE]
>如果你使用组策略在计算机上设置漫游用户配置文件，并使用 Active Directory 在用户帐户上进行设置，则基于计算机的策略设置具有较高优先级。

下面介绍了如何在计算机上设置漫游用户配置文件：

1. 在装有组策略管理的计算机上打开服务器管理器。
2. 从**工具**菜单中，选择**组策略管理**。 将显示组策略管理。
3. 在组策略管理，右键单击在步骤 3 中创建的 GPO (例如，**漫游用户配置文件设置**)，然后选择**编辑**。
4. 在组策略管理编辑器窗口中，依次导航到“计算机配置”  、“策略”  、“管理模板”  、“系统”  、“用户配置文件”  。
5. 右键单击**设置漫游的所有用户登录此计算机的配置文件路径**，然后选择**编辑**。
    > [!TIP]
    > 用户的主文件夹（如果已配置）是某些程序（如 Windows PowerShell）使用的默认文件夹。 你可以使用 AD DS 中用户帐户属性的“主文件夹”  部分，为每个用户配置备用的本地或网络位置。 若要配置运行 Windows 8.1，Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2 或在虚拟桌面环境中的 Windows Server 2012 的计算机的所有用户的主文件夹位置，请启用**设置用户主文件夹**策略设置，然后指定要映射 （或指定一个本地文件夹） 的文件共享和驱动器号。 请不要使用环境变量或省略号。 用户的别名将附加到用户登录期间指定的路径末尾。
6. 在中**属性**对话框中，选择**已启用**
7. 在中**用户登录此计算机应使用此漫游配置文件路径**框中，输入你想要存储用户的漫游用户配置文件后, 跟的文件共享的路径`%username%`（这会自动替换为用户名称第一个时间在用户登录）。 例如：

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    若要指定强制漫游用户配置文件，这是一个预配置的配置文件，用户无法进行永久更改 （当用户注销时，更改将重置），指定指向之前创建，例如的 NTuser.man 文件的路径， `\\fs1.corp.contoso.com\User Profiles$\default`。 有关详细信息，请参阅 [创建强制用户配置文件](https://docs.microsoft.com/windows/client-management/mandatory-user-profile)。
8. 选择“确定”  。

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>步骤 7：选择 Windows 10 电脑的指定开始布局

可以使用组策略来应用特定的开始菜单布局，以便用户在所有 Pc 上看到相同的开始布局。 如果用户登录到多台 PC 和您希望它们在 Pc 中具有一致的开始布局，请确保 GPO 应用于所有他们的 Pc。

若要指定开始布局，请执行以下操作：

1. 更新的 Windows 10 电脑，为 Windows 10 版本 1607 （也称为周年更新） 或更高版本，并安装的 2017 年 3 月 14 日累积更新 ([KB4013429](https://support.microsoft.com/kb/4013429)) 或更高版本。
2. 创建一个完整或部分开始菜单布局 XML 文件。 若要执行此操作，请参阅[自定义和导出开始布局](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)。
    * 如果指定*完整*开始布局，用户不能自定义开始菜单的任何部分。 如果指定*分部*开始布局，用户可以自定义磁贴你指定的锁定组以外的所有内容。 但是，使用部分开始布局，到开始菜单的用户自定义项不会漫游到其他电脑。
3. 使用组策略要应用于漫游用户配置文件创建的 GPO 的自定义的开始布局。 若要执行此操作，请参阅[使用组策略来应用域中的自定义的开始布局](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment)。
4. 使用组策略在 Windows 10 电脑上设置以下注册表值。 若要执行此操作，请参阅[配置注册表项](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>)。

| **操作** | **Update** |
|------------|------------|
|配置单元|**HKEY_LOCAL_MACHINE**|
|密钥路径|**Software\Microsoft\Windows\CurrentVersion\Explorer**|
|值名称|**SpecialRoamingOverrideAllowed**|
|值类型|**REG_DWORD**|
|“数值数据”|**1** (或**0**禁用)|
|Base|**十进制**|

5. （可选）启用要进行更快地登录用户的首次登录优化。 若要执行此操作，请参阅[应用策略来缩短登录时间](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time)。
6. （可选）通过从你用于部署客户端电脑的 Windows 10 基本映像中删除不必要的应用，进一步减少登录时间。 Windows Server 2016 和 Windows Server 2019 没有任何预配的应用程序，因此可以跳过此步骤，在服务器映像。
    - 若要删除应用程序，请使用[删除 AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)卸载以下应用程序在 Windows PowerShell 中的 cmdlet。 如果已部署您的 Pc 可以编写脚本使用这些应用程序删除[删除 AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps)。
    
      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>卸载这些应用程序会降低登录时间，但您可以将其安装如果您的部署需求其中任何一个进行保留。

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>步骤 8：启用漫游用户配置文件 GPO

如果你使用组策略在计算机上设置漫游用户配置文件，或者如果使用组策略自定义其他漫游用户配置文件设置，则下一步是启用 GPO，以允许将其应用于受影响的用户。

>[!TIP]
>如果您计划实现主计算机支持，请立即登录，才能启用该 GPO。 这会防止在启用主计算机支持之前将用户数据复制到非主计算机。 有关特定策略设置，请参阅[部署文件夹重定向和漫游用户配置文件的主计算机](deploy-primary-computers.md)。

下面介绍了如何启用漫游用户配置文件 GPO:

1. 打开“组策略管理”。
2. 右键单击你创建的 GPO，然后选择**已启用链接**。 菜单项旁边会出现一个复选框。

## <a name="step-9-test-roaming-user-profiles"></a>步骤 9：测试漫游用户配置文件

若要测试漫游用户配置文件，请登录到一台已针对漫游用户配置文件配置用户帐户的计算机，或登录到已针对漫游用户配置文件进行配置的计算机。 然后确认已将该配置文件重定向。

下面介绍了如何测试漫游用户配置文件：

1. 登录到主计算机（如果已启用主计算机支持），该计算机具有你已启用漫游用户配置文件的用户帐户。 如果你已在特定计算机上启用漫游用户配置文件，则登录到其中一台计算机。
2. 如果用户以前已登录到计算机，则打开提升的命令提示符，然后键入以下命令以确保最新的组策略设置已应用到客户端计算机：
    
    ```PowerShell
    GpUpdate /Force
    ```
3. 若要确认漫游用户配置文件，请打开**Control Panel**，选择**系统和安全**，选择**系统**，选择**高级系统设置**，选择**设置**部分中用户配置文件，然后查找**漫游**中**类型**列。

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>附录 A：有关部署漫游用户配置文件的清单

|状态|操作|
|:---:|---|
|☐<br>☐<br>☐<br>☐<br>☐|1.准备域<br>-将计算机加入到域<br>-启用使用单独的配置文件版本<br>创建用户帐户<br>-（可选） 部署文件夹重定向|
|☐<br><br><br>|2.创建漫游用户配置文件的安全组<br>组名称：<br>-成员：|
|☐<br><br>|3.创建用于漫游用户配置文件的文件共享<br>文件共享名称：|
|☐<br><br>|4.创建用于漫游用户配置文件的 GPO<br>-GPO 名称：|
|☐|5.配置漫游用户配置文件策略设置|
|☐<br>☐<br>☐|6.启用漫游用户配置文件<br>-用户帐户上的 AD DS 中启用吗？<br>-在计算机帐户上的组策略中启用吗？<br>|
|☐|7.（可选）适用于 Windows 10 电脑指定必需的开始布局|
|☐<br>☐<br><br>☐<br><br>☐|8.（可选）启用主计算机支持<br>-指定用户的主计算机<br>用户和主计算机映射的位置：<br>-（可选） 启用主计算机支持文件夹重定向<br>-基于计算机的或基于用户的？<br>-为漫游用户配置文件 （可选） 启用主计算机支持|
|☐|9.启用漫游用户配置文件 GPO|
|☐|10.测试漫游用户配置文件|

## <a name="appendix-b-profile-version-reference-information"></a>附录 B：配置文件版本参考信息

每个配置文件具有大致对应于版本的 Windows 使用的配置文件的配置文件版本。 例如，Windows 10，版本 1703年和版本 1607 都使用。V6 配置文件版本。 Microsoft 创建新的配置文件版本仅在需要以保持兼容性，是每个版本的 Windows，为什么不包含新的配置文件版本时。

下表列出了各种版本的 Windows 上漫游用户配置文件的位置。

|操作系统版本|漫游用户配置文件位置|
|---|---|
|Windows XP 和 Windows Server 2003|```\\<servername>\<fileshare>\<username>```|
|Windows Vista 和 Windows Server 2008|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 7 和 Windows Server 2008 R2|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 8 和 Windows Server 2012|```\\<servername>\<fileshare>\<username>.V3``` （软件更新和注册表项应用后）<br>```\\<servername>\<fileshare>\<username>.V2``` （在之前的软件更新和注册表项被应用）|
|Windows 8.1 和 Windows Server 2012 R2|```\\<servername>\<fileshare>\<username>.V4``` （软件更新和注册表项应用后）<br>```\\<servername>\<fileshare>\<username>.V2``` （在之前的软件更新和注册表项被应用）|
|Windows 10|```\\<servername>\<fileshare>\<username>.V5```|
|Windows 10，版本 1703年和版本 1607|```\\<servername>\<fileshare>\<username>.V6```|

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>附录 C：工作大约后重置开始菜单布局升级

下面是一些方法可以解决获取的就地升级后重置的开始菜单布局：

 - 如果只有一个用户使用过设备和 IT 管理员使用 SCCM 等托管的 OS 部署策略它们可以执行以下操作：
    
    1. 导出在升级之前使用导出 Startlayout 开始菜单布局 
    2. OOBE 后但在用户登录之前导入与 StartLayout 导入开始菜单布局  
 
    > [!NOTE] 
    > 导入 StartLayout 修改默认用户配置文件。 创建导入发生后的所有用户配置文件将都获取已导入的开始布局。
 
 - IT 管理员可以选择以管理与组策略启动的布局。 使用组策略提供了集中式的管理解决方案来将启动一个标准化的布局应用于用户。 有两种模式下为将用于开始管理组策略的模式。 完整的锁定和部分的锁定。 完整锁定方案可防止用户对开始的布局进行任何更改。 部分锁定方案允许用户启动的特定区域进行更改。 有关详细信息，请参阅[自定义和导出开始布局](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout)。
        
    > [!NOTE]
    > 部分锁定方案中的用户所做更改将仍会丢失在升级过程。

 -  让布局重置发生，并允许最终用户可以重新配置开始启动。 通知电子邮件或其他通知可以发送到最终用户期望其开始布局重置后操作系统升级到最小化的影响。 

# <a name="change-history"></a>更改历史记录

下表总结了一些对本主题进行的最重要的更改。

|Date|描述 |Reason|
|--- |---         |---   |
|2019 年 5 月 1 日，|2019 添加的更新|
|2018 年 4 月 10 日，|当用户开始自定义都将丢失操作系统的就地升级后的添加的讨论|已知问题的标注。|
|2018 年 3 月 13日日 |Windows Server 2016 的更新 | 脱离早期版本库并在当前版本的 Windows Server 更新。|
|2017 年 4 月 13日日|添加了适用于 Windows 10，版本 1703，配置文件信息并阐明如何漫游配置文件版本工作，升级操作系统时，请参阅[在多个版本的 Windows 上使用漫游用户配置文件时的注意事项](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)。|客户反馈。|
|2017 年 3 月 14日日|用于指定必需的开始布局中的 Windows 10 Pc 添加了可选步骤[附录 a:有关部署漫游用户配置文件的清单](#appendix-a-checklist-for-deploying-roaming-user-profiles)。|最新 Windows 更新中的功能更改。|
|2017 年 1 月 23日日|添加了一个步骤到[步骤 4:（可选） 为漫游用户配置文件创建 GPO](#step-4-optionally-create-a-gpo-for-roaming-user-profiles)委派给经过身份验证的用户，现在是读取权限所需由于组策略安全更新。|安全更改组策略处理过程。|
|2016 年 12 月 29日日|添加中的链接[步骤 8:启用漫游用户配置文件 GPO](#step-8-enable-the-roaming-user-profiles-gpo)以便更轻松地获取有关如何设置主计算机的组策略的信息。 此外修复了几个引用步骤 5 和 6 具有数字的错误。|客户反馈。|
|2016 年 12 月 5 日，|添加了的信息说明漫游问题开始菜单设置。|客户反馈。|
|2016 年 7 月 6 日，|添加 Windows 10 中的配置文件版本后缀[附录 b:配置文件版本参考信息](#appendix-b-profile-version-reference-information)。 也从支持的操作系统的列表中删除 Windows XP 和 Windows Server 2003。|新版本的 Windows，并删除不再受支持的 Windows 版本信息更新。|
|2015 年 7 月 7日|添加了在使用群集文件服务器时禁用连续可用性的要求和步骤。|禁用连续可用性后，群集文件共享可以为小型写入操作（对于漫游用户配置文件很常见）提供更好的性能。|
|2014 年 3 月 19 日|配置文件版本后缀大写 (。V2。V3。V4) 中[附录 b:配置文件版本参考信息](#appendix-b-profile-version-reference-information)。|尽管 Windows 不区分大小写，如果在使用 NFS 文件共享，但务必具有正确的配置文件后缀形式 （大写）。|
|2013 年 10 月 9 日|Windows Server 2012 R2 和 Windows 8.1，阐明了一些事项并添加修订[在多个版本的 Windows 上使用漫游用户配置文件时的注意事项](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows)和[附录 b:配置文件版本参考信息](#appendix-b-profile-version-reference-information)部分。|针对新版本; 的更新客户反馈。|

## <a name="more-information"></a>详细信息

- [部署文件夹重定向、 脱机文件和漫游用户配置文件](deploy-folder-redirection.md)
- [文件夹重定向和漫游用户配置文件部署主计算机](deploy-primary-computers.md)
- [实现用户状态管理](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [复制的用户配置文件数据的 Microsoft 的支持声明](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [使用 DISM 旁加载应用](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [打包、 部署和查询的基于 Windows 运行时的应用程序疑难解答](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)