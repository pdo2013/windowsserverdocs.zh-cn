---
ms.assetid: c54b544f-cc32-4837-bb2d-a8656b22f3de
title: "Active Directory 复制和使用 Windows PowerShell (级别 100) 拓扑管理简介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 006179bb3220f7bccfc7510e1b8ef69678321074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-replication-and-topology-management-using-windows-powershell-level-100"></a>Active Directory 复制和使用 Windows PowerShell (级别 100) 拓扑管理简介

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

对于 Active Directory 的 Windows PowerShell 包括管理复制、网站、域和森林，域控制器以及分区的能力。 用户的以前管理工具，例如 Active Directory 的站点和服务管理单元和 repadmin.exe 会注意到类似的功能现已提供在上下文 Active Directory 的 Windows PowerShell。 此外，cmdlet 可以具有现有 Windows PowerShell 用于 Active Directory cmdlet，因此创建简化的体验，并使客户可以轻松地创建自动化脚本兼容。

> [!NOTE]
> 对于 Active Directory 复制和拓扑 cmdlet 的 Windows PowerShell 是以下环境中可用：
> 
> -    Windows Server 2012 域控制器
> -    远程服务器管理工具，用于广告 DS 和广告 LDS 与 Windows Server 2012 安装。
> -   Windows&reg; 8 与广告 DS 和安装的广告 LDS 远程服务器管理工具。

## <a name="installing-the-active-directory-module-for-windows-powershell"></a>安装 Windows PowerShell 模块的 Active Directory
广告 DS 服务器角色服务器运行 Windows Server 2012 上安装时，将默认情况下安装的 Active Directory 的 Windows PowerShell 模块。 任何其他步骤不都需要添加服务器角色以外。 您还可以在的服务器上的安装远程服务器管理工具，才能运行 Windows Server 2012 上安装的 Active Directory 模块以及你可以通过下载并安装在运行 Windows 8 计算机上安装的 Active Directory 模块[远程服务器管理工具 (RSAT)](https://www.microsoft.com/download/details.aspx?id=28972)。 请参阅[说明](https://www.microsoft.com/download/details.aspx?id=28972)安装步骤。

## <a name="scenarios-for-testing-windows-powershell-for-active-directory-replication-and-topology-management-cmdlets"></a>测试 Active Directory 复制和拓扑管理 cmdlet 的 Windows PowerShell 方案
以下情况下用于管理员熟悉新管理 cmdlet:

-   获取所有域控制器和他们相应站点的列表

-   管理复制拓扑

-   查看复制状态和信息

## <a name="lab-requirements"></a>实验要求

-   Windows Server 2012 的两个域控制器：**DC1**和**DC2**，是 contoso.com 域的一部分，位于该域内公司的网站。

## <a name="view-domain-controllers-and-their-sites"></a>查看他们的站点上的域控制器和
在此步骤中，您将使用 Active Directory 的 Windows PowerShell 模块查看现有的域控制器和复制拓扑域。

来完成以下过程中的步骤，你必须是管理员域组成员或具有等效的权限。

#### <a name="to-view-all-active-directory-sites"></a>若要查看所有 Active Directory 网站

1.  在**DC1**，单击**Windows PowerShell**在任务栏上。

2.  键入以下命令：

    `Get-ADReplicationSite -Filter *`

    这将返回各个站点的详细的信息。 `Filter`参数整个 Active Directory PowerShell cmdlet 限制返回对象的列表。 在此情况下，星号（*）指示所有站点对象。

    > [!TIP]
    > 在 Windows PowerShell 中，你可以使用 Tab 键为自动完成的命令。
    > 
    > 示例：键入`Get-ADRep`按 Tab 多次通过匹配命令跳过，直至到达`Get-ADReplicationSite`。 自动完成也适用于参数名称如`Filter`。

    若要设置格式的输出`Get-ADReplicationSite`命令作为表和将显示限制特定字段，你可以管道输出到`Format-Table`命令 (或"`ft`"简称):

    `Get-ADReplicationSite -Filter * | ft Name`

    这将返回的站点列表，包括只能名称字段较短的版本。

#### <a name="to-produce-a-table-of-all-domain-controllers"></a>若要生成表的所有域控制器

-   键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `Get-ADDomainController -Filter * | ft Hostname,Site`

    此命令返回域控制器举办的名称，以及他们的网站关联。

## <a name="manage-replication-topology"></a>管理复制拓扑
在上一个步骤中，运行该命令后, `Get-ADDomainController -Filter * | ft Hostname,Site`，**DC2**被列为的一部分**企业**站点。 在下面的过程中，您将创建新的分支机构站点，**分支 1**创建一个新站点链接、设置站点链接成本和复制频率，然后移动**DC2**到**分支 1**。

来完成以下过程中的步骤，你必须是管理员域组成员或具有等效的权限。

#### <a name="to-create-a-new-site"></a>若要创建一个新站点

-   键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `New-ADReplicationSite BRANCH1`

    此命令创建新分支机构站点，分支 1。

#### <a name="to-create-a-new-site-link"></a>若要创建一个新站点链接

-   键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `New-ADReplicationSiteLink 'CORPORATE-BRANCH1'  -SitesIncluded CORPORATE,BRANCH1 -OtherAttributes @{'options'=1}`

    此命令创建的站点链接**分支 1**和更改通知过程中打开。

    > [!TIP]
    > 使用自动完成参数名称到选项卡上，如`-SitesIncluded`和`-OtherAttributes`而不是手动将推出它们键入。

#### <a name="to-set-the-site-link-cost-and-replication-frequency"></a>若要设置站点链接成本和复制频率

-   键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `Set-ADReplicationSiteLink CORPORATE-BRANCH1 -Cost 100 -ReplicationFrequencyInMinutes 15`

    此命令设置站点链接开销**分支 1**在**100**和设置复制到该网站与频率**15 分钟**。

#### <a name="to-move-a-domain-controller-to-a-different-site"></a>若要移动到其他网站域控制器

-   键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `Get-ADDomainController DC2 | Move-ADDirectoryServer -Site BRANCH1`

    此命令将移域控制器，**DC2**到**分支 1**站点。

### <a name="verification"></a>验证

##### <a name="to-verify-site-creation-new-site-link-and-cost-and-replication-frequency"></a>验证网站创建新网站的链接，成本和复制的频率

-   单击**服务器管理器**，单击**工具**，然后单击**Active Directory 的站点和服务**和核实如下：

    验证**分支 1**站点包含所有的 Windows PowerShell 命令从正确的值。

    验证**企业分支 1**站点链接创建并连接**分支 1**和**企业**站点。

    验证**DC2**现在正在**分支 1**站点。 或者，你可以打开**Active Directory 的 Windows PowerShell 模块**键入以下命令以确认**DC2**现在正在**分支 1**站点：`Get-ADDomainController -Filter * | ft Hostname,Site`。

## <a name="view-replication-status-information"></a>查看复制状态的信息
在下面的过程中，您将使用 Windows PowerShell 之一 Active Directory 复制和管理 cmdlet `Get-ADReplicationUpToDatenessVectorTable DC1`、来繁衍使用的最新矢量表中每个域控制器由维护简单复制报告。 森林中的每个域控制器中看到的最高的原始写入 USN 跟踪的矢量该表的最新。

来完成以下过程中的步骤，你必须是管理员域组成员或具有等效的权限。

#### <a name="to-view-the-up-to-dateness-vector-table-for-a-single-domain-controller"></a>若要查看的最新矢量表单个域控制器

1.  键入以下命令以**Active Directory 的 Windows PowerShell 模块**提示：

    `Get-ADReplicationUpToDatenessVectorTable DC1`

    这将显示列表中看到的最高 Usn **DC1**森林中的每个域控制器。 **服务器**值是指在此情况下维护表，服务器**DC1**。 **合作伙伴**值指复制合作伙伴（直接或间接）进行了更改。 UsnFilter 值为所看到的最高 USN **DC1**从合作伙伴。 如果到林添加了新的域控制器，它将不会显示在**DC1**直到的表**DC1**接收来自新的域的更改。

#### <a name="to-view-the-up-to-dateness-vector-table-for-all-domain-controllers-in-a-domain"></a>若要查看所有域控制器的最新矢量表域中

1.  在 Windows PowerShell 提示 Active Directory 模块键入以下命令：

    `Get-ADReplicationUpToDatenessVectorTable * | sort Partner,Server | ft Partner,Server,UsnFilter`

    此命令替换**DC1**与`*`，因此从所有域控制器收集的最新矢量表格中的数据。 对数据进行排序通过**合作伙伴**和**服务器**然后表中显示。

    排序允许你轻松地将看到给定的复制合作伙伴的每个域控制器的最后一个 USN 进行比较。 这是一种快速检查该复制出现在您的环境方法。 如果复制正常工作，UsnFilter 值为给定的复制伙伴报告应非常类似跨所有域控制器。

## <a name="see-also"></a>请参阅
[高级 Active Directory 复制和使用 Windows PowerShell 和 #40; 拓扑管理级别 200 & #41;](Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md)


