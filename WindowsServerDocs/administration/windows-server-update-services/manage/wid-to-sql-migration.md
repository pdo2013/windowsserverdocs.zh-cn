---
title: 将 WSUS 数据库从（Windows 内部数据库） WID 迁移到 SQL
description: Windows Server Update Service （WSUS）主题-如何将 WSUS 数据库（SUSDB）从 Windows 内部数据库实例迁移到 SQL Server 的本地或远程实例。
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 0977aa1fd9a6848bd7b85bb592b6a82556277e72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361577"
---
>适用于：Windows Server 2012，Windows Server 2012 R2，Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>将 WSUS 数据库从 WID 迁移到 SQL

使用以下步骤将 WSUS 数据库（SUSDB）从 Windows 内部数据库实例迁移到 SQL Server 的本地或远程实例。

## <a name="prerequisites"></a>先决条件

- SQL 实例。 这可以是默认的**MSSQLServer**或自定义实例。
- SQL Server Management Studio
- 已安装 WID 角色的 WSUS
- IIS （当通过服务器管理器安装 WSUS 时，通常包括此情况）。 它尚未安装，将需要。

## <a name="migrating-the-wsus-database"></a>迁移 WSUS 数据库

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>在 WSUS 服务器上停止 IIS 和 WSUS 服务

从 PowerShell （已提升）运行：

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>从 Windows 内部数据库分离 SUSDB

#### <a name="using-sql-management-studio"></a>使用 SQL Management Studio

1. 右键单击**SUSDB** - @ no__t**任务**- @ no__t 单击 "**分离**： ![image1 @ no__t-8
2. 选中 "**删除现有连接**"，然后单击 **"确定"** （如果存在活动连接，则可选）。
    ![image2 @ no__t-1

#### <a name="using-command-prompt"></a>使用命令提示符

> [!IMPORTANT]
> 以下步骤说明如何使用**sqlcmd**实用工具从 Windows 内部数据库实例分离 WSUS 数据库（SUSDB）。 有关**sqlcmd**实用工具的详细信息，请参阅[sqlcmd 实用工具](https://go.microsoft.com/fwlink/?LinkId=81183)。
> 1. 打开提升的命令提示符
> 2. 运行以下 SQL 命令，通过**sqlcmd**实用工具从 Windows 内部数据库实例分离 WSUS 数据库（SUSDB）：

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>将 SUSDB 文件复制到 SQL Server

1. 将 WID 数据文件夹中的**SUSDB**和**SUSDB\_log.ldf** （ **% SystemDrive%** \* * WINDOWS\WID\DATA * *）复制到 SQL 实例数据文件夹。

> [!TIP]
> 例如，如果 SQL 实例文件夹是**C:\Program FILES\MICROSOFT sql Server\MSSQL12。MSSQLSERVER\MSSQL**，WID Data 文件夹为**C:\Windows\WID\Data，** 将 SUSDB 文件从**C:\Windows\WID\Data**复制到**C:\Program Files\Microsoft SQL Server\MSSQL12。MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>将 SUSDB 附加到 SQL 实例

1. 在**SQL Server Management Studio**的 "**实例**" 节点下，右键单击 "**数据库**"，然后单击 "**附加**"。
    ![image3](images/image3.png)
2. 在 "**附加数据库**" 框中，在 "**要附加的数据库**" 下，单击 "**添加**" 按钮，找到**SUSDB**文件（从 WID 文件夹复制），然后单击 **"确定"** 。
    ![image4 @ no__t-1 ![image5 @ no__t

> [!TIP]
> 也可以使用 Transact-sql 来完成此操作。  有关附加数据库的说明，请参阅[SQL 文档](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database)。
>
> 示例（使用上一示例中的路径）：
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>验证 SQL Server 和数据库登录名和权限

#### <a name="sql-server-login-permissions"></a>SQL Server 登录权限

附加 SUSDB 后，通过执行以下操作，验证**NT AUTHORITY\NETWORK 服务**是否具有对 SQL Server 实例的登录权限：

1. 进入 SQL Server Management Studio
2. 打开实例
3. 单击 "**安全**"
4. 单击 "**登录**"

应该列出**NT AUTHORITY\NETWORK SERVICE**帐户。 如果不是，则需要通过添加新的登录名来添加它。

> [!IMPORTANT]
> 如果 SQL 实例与 WSUS 在不同的计算机上，则应使用 **[FQDN] \\ [WSUSComputerName] $** 格式列出 WSUS 服务器的计算机帐户。  如果不是，可以使用以下步骤添加它，将**NT AUTHORITY\NETWORK SERVICE**替换为 WSUS 服务器的计算机帐户（ **[FQDN] \\ [WSUSComputerName] $** ），这***是授予对*** **NT 机构的权限 \网络服务**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>添加 NT AUTHORITY\NETWORK SERVICE 并向其授予权限

1. 右键单击 "**登录名**"，然后单击 "**新建登录名 ...** "
    ![image6 @ no__t-1
2. 在 "**常规**" 页上，填写**登录名**（**NT AUTHORITY\NETWORK SERVICE**），并将**默认数据库**设置为 "SUSDB"。
    ![image7 @ no__t-1
3. 在 "**服务器角色**" 页上，确保选择 "**公用**" 和 " **sysadmin** "。
    ![image8 @ no__t-1
4. 在 "**用户映射**" 页上：
    - 在 "**映射到此登录名的用户**" 下：选择**SUSDB**
    - 在 **Database 的角色成员身份：SUSDB @ no__t-0，请确保选中以下各项：
        - **公布**
        - **webService** ![image9 @ no__t-2
5. 单击“确定”

你现在应在 "登录名" 下看到**NT AUTHORITY\NETWORK 服务**。
![image10 @ no__t-1

#### <a name="database-permissions"></a>数据库权限

1. 右键单击 SUSDB
2. 选择**属性**
3. 单击**权限**

应该列出**NT AUTHORITY\NETWORK SERVICE**帐户。

1. 如果不是，请添加帐户。
2. 在 "登录名" 文本框中，按以下格式输入 WSUS 计算机：
    > [**FQDN] \\ [WSUSComputerName] $**
3. 验证 "**默认数据库**" 是否设置为 " **SUSDB**"。

    > [!TIP]
    > 在以下示例中，FQDN 为**Contosto.com** ，WSUS 计算机名称为**WsusMachine**：
    >
    > ![image11](images/image11.png)

4. 在 "**用户映射**" 页上，在 **"映射到此登录名的用户"** 下选择**SUSDB**数据库
5. 检查  **"的数据库角色成员身份下的**webservice** ：SUSDB "** ： ![image12 @ no__t-2
6. 单击 **"确定"** 保存设置。
    > [!NOTE]
    > 可能需要重新启动 SQL 服务，更改才能生效。

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>编辑注册表以将 WSUS 指向 SQL Server 实例

> [!IMPORTANT]
> 请认真遵循本部分所述的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改注册表之前，请[备份注册表](https://support.microsoft.com/en-us/help/322756)，以便在出现问题时可以还原。

1. 单击“开始”，单击“运行”，键入“regedit”，然后单击“确定”。
2. 找到以下项：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. 在 "**值**" 文本框中，键入 **[ServerName] \\ [InstanceName]** ，然后单击 **"确定"** 。 如果实例名称是默认实例，则键入 **[ServerName]** 。
4. 找到以下项：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![image13 @ no__t-2
5. 将密钥重命名为**updateservices-api-Database** ![image41 @ no__t-2

    > [!NOTE]
    > 如果不更新此密钥， **wsusutil.exe**将尝试为 WID 而不是已迁移到的 SQL 实例服务。

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>在 WSUS 服务器上启动 IIS 和 WSUS 服务

从 PowerShell （已提升）运行：

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> 如果你使用的是 WSUS 控制台，请关闭并重新启动它。

## <a name="uninstalling-the-wid-role-not-recommended"></a>正在卸载 WID 角色（不推荐）

> [!WARNING]
> 删除 WID 角色还会删除数据库文件夹（ **%SystemDrive%\Program Files\Update Services\Database**），其中包含用于安装后任务的 wsusutil.exe 所需的脚本。 如果选择卸载 WID 角色，请确保事先备份 **%SystemDrive%\Program Files\Update Services\Database**文件夹。

使用 PowerShell：

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

删除 WID 角色后，验证是否存在以下注册表项：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-Database**