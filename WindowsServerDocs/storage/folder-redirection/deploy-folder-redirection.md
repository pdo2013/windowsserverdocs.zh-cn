---
title: 部署使用脱机文件的文件夹重定向
description: 如何使用 Windows Server 部署使用脱机文件的文件夹重定向到 Windows 客户端计算机。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4b6c58f0f33b45052e038a9af941297a294d17b2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812506"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>部署使用脱机文件的文件夹重定向

>适用于：Windows 10、 Windows 7、 Windows 8、 Windows 8.1，Windows Vista、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2008 R2 和 Windows Server （半年频道）

本主题介绍如何使用 Windows Server 部署使用脱机文件的文件夹重定向到 Windows 客户端计算机。

本主题最近更改的列表，请参阅[更改历史记录](#change-history)。

> [!IMPORTANT]
> 由于中所做的安全性更改[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)，我们更新了[步骤 3:为文件夹重定向创建 GPO](#step-3-create-a-gpo-for-folder-redirection)的本主题，因此该 Windows 可以正确应用文件夹重定向策略 （并不还原受影响的 Pc 上的重定向的文件夹）。

## <a name="prerequisites"></a>系统必备

### <a name="hardware-requirements"></a>硬件要求

文件夹重定向需要基于 x64 或基于 x86 的计算机，则不受 Windows® rt。

### <a name="software-requirements"></a>软件要求

文件夹重定向具有以下软件要求：

- 若要管理文件夹重定向，必须以 Domain Administrators 安全组、 Enterprise Administrators 安全组或 Group Policy Creator Owners 安全组的成员的身份登录。
- 客户端计算机必须运行 Windows 10、 Windows 8.1，Windows 8、 Windows 7、 Windows Server 2019、 Windows Server 2016、 Windows Server （半年频道）、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008。
- 客户端计算机必须加入你管理的 Active Directory 域服务 (AD DS)。
- 必须提供一台安装了组策略管理和 Active Directory 管理中心的计算机。
- 文件服务器必须是可用于托管重定向的文件夹。
    - 如果文件共享使用 DFS 命名空间，则 DFS 文件夹（链接）必须具有单个目标，以防止用户在不同服务器上进行互相冲突的编辑。
    - 如果文件共享使用 DFS 复制与另一台服务器复制内容，则用户必须只能够访问源服务器，以防止用户在不同服务器上进行互相冲突的编辑。
    - 当使用群集的文件共享，请禁用要避免出现性能问题与文件夹重定向和脱机文件的文件共享上的连续可用性。 此外，脱机文件可能无法切换到脱机模式 3-6 分钟后用户丢失持续可用文件共享，可能会对尚未使用脱机文件始终脱机模式下的用户的访问权限。

> [!NOTE]
> 文件夹重定向中的某些较新功能有其他客户端计算机和 Active Directory 架构要求。 有关详细信息，请参阅[部署主计算机](deploy-primary-computers.md)，[禁用上的脱机文件的文件夹](disable-offline-files-on-folders.md)，[启用始终脱机模式下](enable-always-offline.md)，和[启用优化移动文件夹](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>第 1 步：创建一个文件夹重定向安全组

如果你的环境尚未设置与文件夹重定向，第一步是创建包含你想要将文件夹重定向策略设置应用的所有用户的安全组。

下面介绍了如何为文件夹重定向创建安全组：

1. 打开服务器管理器的计算机上使用 Active Directory 管理中心安装。
2. 上**工具**菜单中，选择**Active Directory 管理中心**。 此时将出现 Active Directory 管理中心。
3. 右键单击相应的域或 OU 中，选择**新建**，然后选择**组**。
4. 在“创建组”  窗口的“组”  部分，指定以下设置：
    - 在“组名”  中，键入安全组的名称，例如：**文件夹重定向用户**。
    - 在中**组作用域**，选择**安全**，然后选择**全局**。
5. 在中**成员**部分中，选择**添加**。 此时将出现“选择用户、联系人、计算机、服务帐户或组”对话框。
6. 键入的名称的用户或组你想要部署文件夹重定向，请选择**确定**，然后选择**确定**试。

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>步骤 2：创建重定向的文件夹的文件共享

如果你还没有用于重定向的文件夹的文件共享，使用以下过程在运行 Windows Server 2012 的服务器上创建文件共享。

> [!NOTE]
> 如果在运行另一个版本的 Windows Server 的服务器上创建文件共享，则某些功能可能会有所不同或不可用。

下面介绍了如何在 Windows Server 2019、 Windows Server 2016 和 Windows Server 2012 上创建文件共享：

1. 在服务器管理器导航窗格中，选择**文件和存储服务**，然后选择**共享**以显示共享页。
2. 在中**共享**磁贴中，选择**任务**，然后选择**新共享**。 此时将出现新建共享向导。
3. 上**选择配置文件**页上，选择**SMB 共享 – 快速**。 如果已安装的文件服务器资源管理器并且要使用文件夹管理属性，而是选择**SMB 共享 – 高级**。
4. 在“共享位置”  页上，选择你要在其上创建共享的服务器和卷。
5. 上**共享名**页上，键入共享的名称 (例如，**用户 $** ) 中**共享名**框。
    >[!TIP]
    >在创建共享时，通过在共享名后放置 ```$``` 来隐藏共享。 这将隐藏普通浏览器中的共享。
6. 上**其他设置**页上，如果存在，请清除启用连续可用性复选框，并 （可选） 选择**启用基于访问权限的枚举**和**加密数据访问**复选框。
7. 上**权限**页上，选择**自定义权限...** . 将出现高级安全设置对话框。
8. 选择**禁用继承**，然后选择**继承权限对此对象的显式权限转换为**。
9. 表 1 所述以及图 1 所示，删除权限未列出的组和帐户，并将特殊权限添加到在步骤 1 中创建的文件夹重定向用户组设置的权限。
    
    ![设置重定向的文件夹共享的权限](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **图 1**重定向的文件夹共享的设置的权限
10. 如果选择了“SMB 共享 – 高级”  配置文件，则在“管理属性”  页上，选择“用户文件”  文件夹使用值。
11. 如果选择了“SMB 共享 – 高级”  配置文件，则在“配额”  页上选择性地选择要应用于共享用户的配额。
12. 上**确认**页上，选择**创建。**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>该文件所需的权限共享承载重定向的文件夹

| 用户帐户  | 访问  | 适用对象  |
| --------- | --------- | --------- |
| 用户帐户 | 访问 | 适用对象 |
| 系统     | 完全控制        |    此文件夹、子文件夹和文件     |
| Administrators     | 完全控制       | 仅此文件夹        |
| 创建者/所有者     |   完全控制      |   仅子文件夹和文件      |
| 安全组的用户无需将数据放在共享 （文件夹重定向用户）     |   列出文件夹 / 读取数据 *（高级权限）* <br /><br />创建文件夹 / 附加数据 *（高级权限）* <br /><br />读取属性 *（高级权限）* <br /><br />读取扩展属性 *（高级权限）* <br /><br />读取权限 *（高级权限）*      |  仅此文件夹       |
| 其他组和帐户     |  无（删除）       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>步骤 3:为文件夹重定向创建 GPO

如果你还没有为文件夹重定向设置创建一个 GPO，使用以下过程创建一个。

下面介绍了如何为文件夹重定向创建 GPO:

1. 在装有组策略管理的计算机上打开服务器管理器。
2. 从**工具**菜单中，选择**组策略管理**。
3. 右键单击域或 OU 要在其中安装文件夹重定向，然后选择**在此域中创建 GPO 并在此处链接**。
4. 在中**新的 GPO**对话框中，键入 gpo 的名称 (例如，**文件夹重定向设置**)，然后选择**确定**。
5. 右键单击新创建的 GPO，然后清除“已启用链接”  复选框。 这将阻止应用该 GPO，直到已完成对它的配置为止。
6. 选中该 GPO。 中**安全筛选**一部分**作用域**选项卡上，选择**Authenticated Users**，然后选择**删除**以防止 GPO从正在应用到每个人。
7. 在中**安全筛选**部分中，选择**添加**。
8. 在中**选择用户、 计算机或组**对话框中，键入在步骤 1 中创建安全的名称组 (例如，**文件夹重定向用户**)，然后选择**确定**.
9. 选择**委派**选项卡上，选择**添加**，类型**Authenticated Users**，选择**确定**，然后选择**确定**再次接受默认值的读取权限。
    
    此步骤是必需由于中所做的安全性更改[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)。

> [!IMPORTANT]
> 由于中所做的安全性更改[MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)，现在必须授予对文件夹重定向 GPO 的 Authenticated Users 组委派读取权限-否则 GPO 不会应用于用户，或如果已应用，该 GPO 是删除，将文件夹重定向回本地 PC。 有关详细信息，请参阅[部署组策略安全更新 MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/)。

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>步骤 4：配置文件夹重定向的脱机文件

创建用于文件夹重定向设置的 GPO 后, 编辑组策略设置来启用和配置文件夹重定向，如以下过程中所述。

> [!NOTE]
> 脱机文件是用于 Windows 客户端计算机上的重定向文件夹的默认情况下启用和禁用运行 Windows Server 的计算机上，除非用户更改。 若要使用组策略来控制，是否启用了脱机文件，请使用**允许或禁止使用的脱机文件功能**策略设置。
> 有关的一些其他脱机文件组策略设置的信息，请参阅[启用高级脱机文件功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)，并[脱机文件配置组策略](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>)。

下面介绍了如何在组策略中配置文件夹重定向：

1. 在组策略管理，右键单击你创建的 GPO (例如，**文件夹重定向设置**)，然后选择**编辑**。
2. 在组策略管理编辑器窗口中，导航到**用户配置**，然后**策略**，然后**Windows 设置**，然后**文件夹重定向**。
3. 右键单击你想要重定向的文件夹 (例如，**文档**)，然后选择**属性**。
4. 在中**属性**对话框中，从**设置**框中，选择**基本-将每个人的文件夹重定向到同一位置**。

    > [!NOTE]
    > 若要将文件夹重定向应用于运行 Windows XP 或 Windows Server 2003 客户端计算机，请选择**设置**选项卡并选择**也将重定向策略应用到 Windows 2000、 Windows 2000 Server、 Windows XP 和Windows Server 2003 操作系统**复选框。

5. 在中**目标文件夹位置**部分中，选择**为根路径下的每个用户创建的文件夹**，然后在**根路径**框中，键入文件共享存储的路径重定向文件夹，例如：  **\\ \\fs1.corp.contoso.com\\用户 $**
6. 选择**设置**选项卡上，然后在**删除策略时**部分中，可以选择性地选中**策略被删除时，将文件夹重定向回本地用户配置文件位置**（此设置可帮助使文件夹重定向行为更有预见性地 adminisitrators 和用户）。
7. 选择**确定**，然后选择**是**警告对话框中。

## <a name="step-5-enable-the-folder-redirection-gpo"></a>步骤 5：启用文件夹重定向 GPO

完成配置文件夹重定向组策略设置后下, 一步是启用 GPO，使它可以应用于受影响的用户。

> [!TIP]
> 如果你计划实现主计算机支持或其他策略设置，现在请进行该操作，然后再启用该 GPO。 这会防止在启用主计算机支持之前将用户数据复制到非主计算机。

下面介绍了如何启用文件夹重定向 GPO:

1. 打开“组策略管理”。
2. 右键单击该 GPO 创建，并选择**已启用链接**。 菜单项旁边将显示一个复选框。

## <a name="step-6-test-folder-redirection"></a>步骤 6：测试文件夹重定向

若要测试文件夹重定向，请登录到计算机配置文件夹重定向的用户帐户。 然后，确认重定向的文件夹和配置文件。

下面介绍了如何测试文件夹重定向：

1. 登录到主计算机 （如果启用主计算机支持） 使用已启用文件夹重定向的用户帐户。
2. 如果用户以前已登录到计算机，则打开提升的命令提示符，然后键入以下命令以确保最新的组策略设置已应用到客户端计算机：
    
    ```PowerShell
    gpupdate /force
    ```
3. 打开文件资源管理器。
4. 重定向的文件夹 （例如，在文档库中的 My Documents 文件夹），右键单击，然后选择**属性**。
5. 选择**位置**选项卡，并确认该路径显示，而不是本地路径指定的文件共享。

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>附录 A：有关部署文件夹重定向的清单

| 状态           | 操作 |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1.准备域<br>-将计算机加入到域<br>创建用户帐户 |
| ☐<br><br><br>   | 2.为文件夹重定向创建安全组<br>组名称：<br>-成员： |
| ☐<br><br>       | 3.创建重定向的文件夹的文件共享<br>文件共享名称： |
| ☐<br><br>       | 4.为文件夹重定向创建 GPO<br>-GPO 名称： |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5.配置文件夹重定向和脱机文件策略设置<br>-重定向的文件夹：<br>-Windows 2000，Windows XP 和 Windows Server 2003 支持已启用？<br>的已启用脱机文件？ （默认情况下，Windows 客户端计算机上启用）<br>-已启用始终脱机模式？<br>启用背景文件同步？<br>优化移动启用了重定向的文件夹？ |
| ☐<br><br>☐<br><br>☐<br>☐ | 6.（可选）启用主计算机支持<br>-基于计算机的或基于用户的？<br>-指定用户的主计算机<br>用户和主计算机映射的位置：<br>-（可选） 启用主计算机支持文件夹重定向<br>-为漫游用户配置文件 （可选） 启用主计算机支持 |
| ☐         | 7.启用文件夹重定向 GPO |
| ☐         | 8.测试文件夹重定向 |

## <a name="change-history"></a>更改历史记录

下表总结了一些对本主题进行的最重要的更改。

| Date | 描述 | Reason|
| --- | --- | --- |
| 2017 年 1 月 18日日 | 添加了一个步骤到[步骤 3:为文件夹重定向创建 GPO](#step-3-create-a-gpo-for-folder-redirection)委派给经过身份验证的用户，现在是读取权限所需由于组策略安全更新。 | 客户反馈 |

## <a name="more-information"></a>详细信息

* [文件夹重定向、 脱机文件和漫游用户配置文件](folder-redirection-rup-overview.md)
* [文件夹重定向和漫游用户配置文件部署主计算机](deploy-primary-computers.md)
* [启用高级脱机文件功能](enable-always-offline.md)
* [复制的用户配置文件数据的 Microsoft 的支持声明](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [使用 DISM 旁加载应用](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [打包、 部署和查询的基于 Windows 运行时的应用程序疑难解答](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)