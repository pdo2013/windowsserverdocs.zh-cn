---
title: 对命名空间启用基于访问的枚举
description: 本文介绍如何对命名空间启用基于访问的枚举。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e9a2b2162fcf11385a5a866372e57338df87c541
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850588"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>对命名空间启用基于访问的枚举

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

基于访问的枚举可隐藏用户无权访问的文件和文件夹。 默认情况下，对于 DFS 命名空间，不启用此功能。 你可以通过使用 DFS 管理对 DFS 文件夹启用基于访问的枚举。 若要控制对文件夹目标中的文件和文件夹进行基于访问的枚举，必须通过使用共享和存储管理，对每个共享文件夹启用基于访问的枚举。

若要启用基于访问的命名空间上的枚举，命名空间的所有服务器必须都运行 Windows Server 2008 或更高版本。 此外，基于域的命名空间必须使用 Windows Server 2008 模式。 有关 Windows Server 2008 模式的要求的信息，请参阅[选择 Namespace 类型](choose-a-namespace-type.md)。

在某些环境中，启用基于访问的枚举可能会导致服务器上的 CPU 使用率较高，以及对用户的响应时间较长。

> [!NOTE]
> 如果升级域功能级别为 Windows Server 2008 时有现有的基于域的 DFS 管理的命名空间，便可以启用基于访问权限的枚举这些命名空间上。 但是，您将不能编辑权限，以隐藏文件夹从任何组或用户，除非将命名空间迁移到 Windows Server 2008 模式。 有关详细信息，请参阅[将基于域的命名空间迁移到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)。


若要对 DFS 命名空间使用基于访问的枚举，必须执行以下步骤：

-   对命名空间启用基于访问的枚举
-   控制哪些用户和组可以查看单个 DFS 文件夹


> [!WARNING]
> 如果用户已知 DFS 路径，则基于访问的枚举不会阻止他们获得对文件夹目标的引用。 只有共享权限或文件夹目标（共享文件夹）本身的 NTFS 文件系统权限，才能阻止用户访问该文件夹目标。 DFS 文件夹权限仅用于显示或隐藏 DFS 文件夹，而不用来控制访问权限，这使得读取访问权限是 DFS 文件夹级别的唯一相关权限。 有关详细信息，请参阅[使用继承的权限执行基于访问的枚举](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
你可以使用 Windows 界面或命令行对命名空间启用基于访问的枚举。

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>使用 Windows 界面启用基于访问的枚举

1.  在控制台树中的**命名空间**节点下，右键单击相应的命名空间，然后单击**属性**。

2.  单击**高级**选项卡，然后选中**对此命名空间启用基于存取的枚举**复选框。

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>使用命令行启用基于访问的枚举

1.  在安装有**分布式文件系统**角色服务或**分布式文件系统工具**功能的服务器上打开命令提示符窗口。

2.  键入以下命令，其中 *< 命名空间\_根 >* 是命名空间的根：

    ```  
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> 若要使用 Windows PowerShell 管理对命令空间的基于访问的枚举，请使用 [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx)、[Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx) 和 [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx) cmdlet。 Windows Server 2012 中引入了 DFSN Windows PowerShell 模块。

你可以使用 Windows 界面或命令行控制哪些用户和组可以查看单个 DFS 文件夹。

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>使用 Windows 界面控制文件夹的可见性

1.  在控制台树中的**命名空间**节点下，找到要控制其可见性的文件夹（包含目标），右键单击该文件夹，然后单击**属性**。

2.  单击“高级”选项卡。

3.  单击**设置 DFS 文件夹的显式查看权限**，然后再单击**配置查看权限**。

4.  单击**添加**或**删除**，以添加或删除组或用户。

5.  若要允许用户查看 DFS 文件夹，请选择相应的组或用户，然后选中**允许**复选框。

    若要对组或用户隐藏该文件夹，请选择该组或用户，然后选中**拒绝**复选框。

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>使用命令行控制文件夹的可见性

1.  在安装有**分布式文件系统**角色服务或**分布式文件系统工具**功能的服务器上打开命令提示符窗口。

2.  键入以下命令，其中*&lt;DFSPath&gt;* 是 DFS 文件夹 （链接） 的路径 *< 域\\帐户 >* 是组或用户帐户的名称和 *（...）* 将被替换为其他访问控制项 (Ace):

    ```
    dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
    ```

    例如，将现有的权限替换权限，允许 Domain Admins 和 CONTOSO\\培训师组 Read (R) 访问\\contoso.office\public\training 文件夹中，键入以下命令：

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace 
   ```

3. 若要从命令提示符执行其他任务，请使用以下命令：


| Command | 描述 |
|---|---|
|[Dfsutil 属性 sd 拒绝](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|拒绝组或用户，使其无法查看文件夹。|
|[重置 Dfsutil 属性 sd](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |从文件夹中删除所有权限。|
|[Dfsutil 属性 sd revoke](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| 从文件夹中删除组或用户 ACE。 |

## <a name="see-also"></a>请参阅

-   [创建 DFS Namespace](create-a-dfs-namespace.md)
-   [为 DFS 命名空间委派管理权限](delegate-management-permissions-for-dfs-namespaces.md)
-   [安装 DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [基于访问的枚举中使用继承的权限](using-inherited-permissions-with-access-based-enumeration.md)