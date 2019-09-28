---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: 使用 Windows PowerShell 的 Active Directory 复制和拓扑管理（级别 100）简介
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c8a5863865d465d55f1d5865fdcbdeeb942ce194
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409081"
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>使用 Windows PowerShell 的 Active Directory 复制和拓扑管理（级别 100）简介

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 的 Windows PowerShell 包含管理复制、站点、域和林、域控制器和分区的功能。 过去的管理工具（如 Active Directory 站点和服务管理单元与 repadmin.exe）的用户将发现如今也可在 Active Directory 的 Windows PowerShell 上下文中使用相似功能。 此外，cmdlet 可与现有的 Active Directory 的 Windows PowerShell cmdlet 兼容，从而营造简化的体验并允许客户轻松创建自动化脚本。

> [!NOTE]
> Active Directory 的 Windows PowerShell 复制和拓扑 cmdlet 可用于以下环境：
> 
> -    Windows Server 2012 域控制器
> -    Windows Server 2012 安装了 AD DS 和 AD LDS 的远程服务器管理工具。
> -   Windows @ no__t-0 8，其中安装了 AD DS 和 AD LDS 的远程服务器管理工具。

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>安装 Windows PowerShell 的 Active Directory 模块
如果在运行 Windows Server 2012 的服务器上安装了 AD DS 服务器角色，则默认情况下将安装适用于 Windows PowerShell 的 Active Directory 模块。 无需添加服务器角色之外的任何其他步骤。 你还可以通过安装远程服务器管理工具将 Active Directory 模块安装在运行 Windows Server 2012 的服务器上，并且可以通过下载并安装[来在运行 windows 8 的计算机上安装 Active Directory 模块远程服务器管理工具（RSAT）](https://www.microsoft.com/download/details.aspx?id=28972)。 有关安装步骤，请参阅 [说明](https://www.microsoft.com/download/details.aspx?id=28972)。

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>用于测试 Active Directory 的 Windows PowerShell 复制和拓扑管理 cmdlet 的方案
以下方案是为了帮助管理员熟悉新的管理 cmdlet 而设计：

-   获取所有控制器及相应站点的列表

-   管理复制拓扑

-   查看复制状态和信息

## <a name="lab-requirements"></a>实验室要求

-   两个 Windows Server 2012 域控制器：**DC1** 和 **DC2**，是 contoso.com 域的一部分，并位于该域的 CORPORATE 站点中。

## <a name="view-domain-controllers-and-their-sites"></a>查看域控制器及其站点
在此步骤中，你将使用 Windows PowerShell 的 Active Directory 模块查看此域的现有域控制器和复制拓扑。

要完成以下程序中的步骤，你必须是域管理员组的成员或拥有同等权限。

#### <a name="to-view-all-active-directory-sites"></a>查看所有 Active Directory 站点

1.  在“DC1”上，单击任务栏上的“Windows PowerShell”。

2.  键入下列命令：

    `Get-ADReplicationSite -Filter *`

    这会返回关于每个站点的详细信息。 `Filter` 参数在整个 Active Directory PowerShell cmdlet 中用于限制返回的对象列表。 在这种情况下，星号 (*) 指示所有站点对象。

    > [!TIP]
    > 你可使用 Tab 键自动完成 Windows PowerShell 中的命令。
    > 
    > 例如：键入 `Get-ADRep` 并按 Tab 键多次，以跳过匹配的命令，直到你达到 `Get-ADReplicationSite`。 自动完成也适用于参数名称，例如 `Filter`。

    若要将 `Get-ADReplicationSite` 命令的输出格式设置为表，并将显示范围限制为特定的字段，可以通过管道将输出传递给 @no__t 命令（或使用 "`ft`"）：

    `Get-ADReplicationSite -Filter * | ft Name`

    这会返回较短版本的站点列表，只包括“名称”字段。

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>生成所有域控制器的表格

-   在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    此命令会返回域控制器主机名称以及它们的站点关联。

## <a name="manage-replication-topology"></a>管理复制拓扑
在以前步骤中，运行该命令 `Get-ADDomainController -Filter * | ft Hostname,Site`后， **DC2** 列为 **CORPORATE** 站点的一部分。 在以下过程中，你将创建新的分支站点 **BRANCH1**，创建新的站点链接，并设置站点链接成本和复制频率，然后将 **DC2** 移到 **BRANCH1**。

要完成以下程序中的步骤，你必须是域管理员组的成员或拥有同等权限。

#### <a name="to-create-a-new-site"></a>创建新站点

-   在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `New-ADReplicationSite BRANCH1`

    此命令会创建新的分支机构站点 branch1。

#### <a name="to-create-a-new-site-link"></a>创建新的站点链接

-   在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    该命令创建指向 **BRANCH1** 的站点链接，并启用更改通知过程。

    > [!TIP]
    > 使用“Tab”自动完成参数名称，例如 `-SitesIncluded` 和 `-OtherAttributes`（而非手动键入它们）。

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>设置链接成本和复制频率

-   在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    该命令将指向 **BRANCH1** 的站点链接成本设置为 **100** ，并将该站点的复制频率设置为 **15 分钟**。

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>将域控制器移动到不同站点

-   在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    该命令将域控制器 **DC2** 移到 **BRANCH1** 站点。

### <a name="verification"></a>验证

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>若要验证站点创建、新站点链接和成本以及复制频率

-   单击 **“服务器管理器”** ，单击 **“工具”** ，然后单击 **“Active Directory 站点和服务”** ，并验证以下各项：

    验证 **BRANCH1** 站点包含所有来自 Windows PowerShell 命令的正确值。

    验证已创建 **CORPORATE-BRANCH1** 站点链接，并连接了 **BRANCH1** 和 **CORPORATE** 站点。

    验证 **DC2** 现已在 **BRANCH1** 站点中。 此外，你可以打开 **Windows PowerShell 的 Active Directory 模块**，并键入以下命令，以验证 **DC2** 现已在 **BRANCH1** 站点：`Get-ADDomainController -Filter * | ft Hostname,Site`.

## <a name="view-replication-status-information"></a>查看复制状态信息
在以下过程中，你会将一个 Windows PowerShell 用于 Active Directory 复制和管理 cmdlet `Get-ADReplicationUpToDatenessVectorTable DC1`，以便使用每个域控制器维护的最新矢量表生成简单的复制报告。 这个最新矢量表将跟踪从林中每个域控制器观察到的最高起源写入 USN。

要完成以下程序中的步骤，你必须是域管理员组的成员或拥有同等权限。

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>查看单独一个域控制器的最新是量表

1.  在提示“Windows PowerShell 的 Active Directory 模块” 时键入以下命令：

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    这显示了林中每个域控制器的 **DC1** 观察到的最高 USN 列表。 **Server** 值参考了维护该表的服务器，在这种情况下服务器为 **DC1**。 **Partner** 值参考了（直接或间接）做出更改的复制伙伴。 UsnFilter 值是来自 Partner 参数的 **DC1** 观察到的最高 USN。 如果将新的域控制器添加到林中，则它将不会出现在**dc1**的表中，直到**dc1**接收到来自新域的更改。

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>查看域中所有域控制器的最新矢量表

1.  在提示“Windows PowerShell 的 Active Directory 模块”时键入以下命令：

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    该命令使用 **替换了** DC1 `*`，因此收集的最新矢量表数据来自所有域控制器。 数据按 **Partner** 和 **Server** 排序，然后显示在表格中。

    排序允许你轻松对给予复制伙伴每个域控制器观察到的最后 USN 进行比较。 这是检查环境中是否正在进行复制的快速方法。 如果复制正常工作，所有域控制器中为给定复制伙伴报告的 UsnFilter 值必须非常相似。

## <a name="see-also"></a>请参阅
[使用 Windows PowerShell &#40;Level 200 的高级 Active Directory 复制和拓扑管理&#41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


