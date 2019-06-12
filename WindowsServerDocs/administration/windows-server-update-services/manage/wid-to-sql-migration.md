---
title: 迁移 WSUS 数据库 （Windows 内部数据库） 从 WID 到 SQL
description: Windows Server Update Service (WSUS) 主题-如何将 WSUS 数据库 (SUSDB) 从 Windows 内部数据库实例迁移到 SQL Server 的本地或远程实例。
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9015bbc54a4c4bda0f691b79dbb7d3ba8ddbc4a1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439890"
---
>适用于：Windows Server 2012 中，Windows Server 2012 R2、 Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>从 WID 的 WSUS 数据库迁移到 SQL

使用以下步骤迁移 WSUS 数据库 (SUSDB) 从 Windows 内部数据库实例到 SQL Server 的本地或远程实例。

## <a name="prerequisites"></a>系统必备

- SQL 实例。 这可以是默认值**MSSQLServer**或自定义实例。
- SQL Server Management Studio
- 具有 WID 角色安装的 WSUS
- IIS （这是通常都包含在安装 WSUS 通过服务器管理器时）。 未安装，它将需要是。

## <a name="migrating-the-wsus-database"></a>迁移 WSUS 数据库

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>在 WSUS 服务器上停止 IIS 和 WSUS 服务

从 PowerShell （已提升权限），运行：

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>分离 SUSDB 从 Windows 内部数据库

#### <a name="using-sql-management-studio"></a>使用 SQL Management Studio

1. 右键单击**SUSDB** - &gt; **任务** - &gt;单击**分离**: ![image1](images/image1.png)
2. 检查**删除现有连接**然后单击**确定**（可选，如果存在活动连接）。
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>使用命令提示符

> [!IMPORTANT]
> 以下步骤演示如何分离 WSUS 数据库 (SUSDB) 从 Windows 内部数据库实例使用**sqlcmd**实用程序。 有关详细信息**sqlcmd**实用程序，请参阅[sqlcmd 实用工具](https://go.microsoft.com/fwlink/?LinkId=81183)。
> 1. 打开提升的命令提示符
> 2. 使用 Windows 内部数据库实例中运行以下的 SQL 命令，以分离 WSUS 数据库 (SUSDB) **sqlcmd**实用程序：

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

1. 复制**SUSDB.mdf**并**SUSDB\_log.ldf**从 WID 数据文件夹 ( **%systemdrive%** \** Windows\WID\Data * *) 到 SQL 实例数据文件夹。

> [!TIP]
> 例如，如果你的 SQL 实例文件夹是**C:\Program Files\Microsoft SQL Server\MSSQL12。MSSQLSERVER\MSSQL**，以及 WID 数据文件夹是**C:\Windows\WID\Data，** 中的 SUSDB 文件复制**C:\Windows\WID\Data**到**C:\Program Files\Microsoft SQL Server\MSSQL12。MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>将 SUSDB 附加到 SQL 实例

1. 在中**SQL Server Management Studio**下**实例**节点，右键单击**数据库**，然后单击**附加**。
    ![image3](images/image3.png)
2. 在中**附加数据库**框中，在**要附加的数据库**，单击**添加**按钮，然后找到**SUSDB.mdf**文件 (从复制WID 文件夹中），然后单击**确定**。
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> 这也是能够完成使用 Transact-sql。  请参阅[有关附加数据库的 SQL 文档](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database)有关其说明。
>
> 示例 （使用上一示例中的路径）：
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

在附加 SUSDB 之后, 确认**NT AUTHORITY\NETWORK SERVICE**具有登录名对 SQL Server 实例的权限执行以下操作：

1. 转到 SQL Server Management Studio
2. 打开实例
3. 单击**安全**
4. 单击**登录名**

**NT AUTHORITY\NETWORK SERVICE**应列出帐户。 如果不是这样，您需要通过添加新登录名添加。

> [!IMPORTANT]
> 如果从 WSUS 不同计算机上 SQL 实例，应将 WSUS 服务器的计算机帐户列入格式 **[FQDN]\\[WSUSComputerName] $** 。  如果没有，请执行以下步骤可用于将其添加，替换**NT AUTHORITY\NETWORK SERVICE**与 WSUS 服务器的计算机帐户 ( **[FQDN]\\[WSUSComputerName] $** ) 这将是***除了***权限授予**NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>添加 NT AUTHORITY\NETWORK SERVICE 并为其授予权限

1. 右键单击**登录名**单击**新登录名...**
    ![image6](images/image6.png)
2. 上**常规**页上，填写**登录名**(**NT AUTHORITY\NETWORK SERVICE**)，并设置**默认数据库**SUSDB 到。
    ![image7](images/image7.png)
3. 上**服务器角色**页上，确保**公共**并**sysadmin**选择。
    ![image8](images/image8.png)
4. 上**用户映射**页：
    - 下**映射到此登录名的用户**： 选择**SUSDB**
    - 下**数据库角色成员身份：SUSDB**，确保以下检查：
        - **public**
        - **webService** ![image9](images/image9.png)
5. 单击“确定” 

现在应看到**NT AUTHORITY\NETWORK SERVICE**下登录名。
![image10](images/image10.png)

#### <a name="database-permissions"></a>数据库权限

1. 右键单击 SUSDB
2. 选择**属性**
3. 单击**权限**

**NT AUTHORITY\NETWORK SERVICE**应列出帐户。

1. 如果不是，添加的帐户。
2. 在登录名名称文本框中，输入采用以下格式的 WSUS 计算机:
    > [**FQDN]\\[WSUSComputerName]$**
3. 确认**默认数据库**设置为**SUSDB**。

    > [!TIP]
    > 在以下示例中，FQDN 是**Contosto.com**和 WSUS 计算机名称是否**WsusMachine**:
    >
    > ![image11](images/image11.png)

4. 上**用户映射**页上，选择**SUSDB**数据库下 **"用户映射到此登录名"**
5. 检查**webservice**下 **"数据库角色成员身份：SUSDB"** : ![image12](images/image12.png)
6. 单击**确定**以保存设置。
    > [!NOTE]
    > 您可能需要重新启动 SQL 服务所做的更改才会生效。

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>编辑注册表，到点的 WSUS，到 SQL Server 实例

> [!IMPORTANT]
> 请仔细按照本部分中的步骤。 如果注册表修改不正确，可能会发生严重问题。 在修改之前[备份注册表以进行还原](https://support.microsoft.com/en-us/help/322756)在出现问题时。

1. 单击“开始”  ，单击“运行”  ，键入“regedit”  ，然后单击“确定”  。
2. 找到以下项：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. 在中**值**文本框中，键入 **[ServerName]\\[InstanceName]** ，然后单击**确定**。 如果实例名称是默认实例，请键入 **[ServerName]** 。
4. 找到以下项：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. 重命名的关键**UpdateServices 数据库** ![image41](images/image14.png)

    > [!NOTE]
    > 如果不执行更新此密钥，然后**WsusUtil**将尝试服务 WID 而不是在迁移到 SQL 实例。

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>在 WSUS 服务器上启动 IIS 和 WSUS 服务

从 PowerShell （已提升权限），运行：

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> 如果使用 WSUS 控制台中，关闭并重新启动它。

## <a name="uninstalling-the-wid-role-not-recommended"></a>卸载 WID 角色 （不推荐）

> [!WARNING]
> 删除 WID 角色也会删除数据库文件夹 ( **%SystemDrive%\Program Files\Update Services\Database**)，其中包含 WSUSUtil.exe 的所需的安装后任务的脚本。 如果您选择卸载 WID 角色，请确保备份 **%SystemDrive%\Program Files\Update Services\Database**事先文件夹。

使用 PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

WID 角色会被删除后，验证以下注册表项存在：**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-Database**