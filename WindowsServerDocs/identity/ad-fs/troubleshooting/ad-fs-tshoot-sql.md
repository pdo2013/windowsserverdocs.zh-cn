---
title: AD FS 故障排除-SQL 连接
description: 本文档介绍如何对 AD FS 的各个方面进行故障排除
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b09094b6e305bc85b38e94d11fbc8845d555437
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443932"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>AD FS 故障排除-SQL 连接
AD FS 提供对 AD FS 场数据使用远程 SQL Server 的功能。  如果您的服务器场中的 AD FS 服务器无法与后端 SQL server 通信，你将发现问题。  以下文档将提供一些基本步骤来测试通过后端服务器通信。

## <a name="acquire-the-sql-database-connection-string"></a>获取 SQL 数据库连接字符串
要测试检查 SQL 连接时的第一点是，如果 AD FS 具有正确的 SQL 连接信息。  这可以使用 PowerShell。

### <a name="to-acquire-the-sql-connection-string"></a>若要获取 SQL 连接字符串
1.  打开 Windows PowerShell
2. 请输入以下命令：`$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService`并按 Enter
3. 请输入以下命令：`$adfs.ConfigurationDatabaseConnectionString`并按 enter。
4. 你应看到连接字符串信息。
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>创建一个通用数据链接 (UDL) 文件来测试连接
通用数据链接文件或 UDL 文件基本上是一个文本文件，其中包含的数据库连接字符串。  通过使用我们上面获取的信息，我们可以测试在 SQL server 响应的连接。

### <a name="to-create-a-udl-file-to-test-connectivity"></a>若要创建 udl 文件来测试连接

1. 打开记事本并将文件另存 test.udl。  请确保你拥有**的所有文件**从下拉列表中选择**另存为类型**。
2. 双击 test.udl
3. 填写以下信息：。 **选择或输入服务器名称：** 使用上面 b 的连接字符串中的数据源。 **输入登录到服务器上的信息：** 使用 AD FS 服务帐户或具有远程登录的权限的帐户。  如果帐户是 windows 帐户，请使用集成身份验证，否则为输入的用户名和密码。
    c. **选择在服务器上的数据库：** 使用初始目录从上面的字符串。  例如：AdfsConfigurationV3.
   ![测试连接](media/ad-fs-tshoot-sql/sql4.png)
1. 单击**测试连接**。</br>
![Success](media/ad-fs-tshoot-sql/sql3.png)

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>使用 SQL Server Management Studio 来测试连接
此外可以[下载](https://go.microsoft.com/fwlink/?linkid=864329)并安装 SSMS 测试数据库连接。

### <a name="to-test-connectivity-with-ssms"></a>若要测试使用 SSMS 连接
1. 下载并安装 SQL Server Management Studio。
![安装](media/ad-fs-tshoot-sql/sql5.png)
1. 打开 SSMS 中，输入服务器名称。  上面提供的数据源。
2. 使用 AD FS 服务帐户或具有远程登录的权限的帐户。  如果帐户是 windows 帐户，请使用集成身份验证，否则为输入的用户名和密码。
![“连接”](media/ad-fs-tshoot-sql/sql6.png)
1. 应会看到左侧和右侧填充。  展开数据库并验证能看到 AD FS 数据库。
![AD FS 数据库](media/ad-fs-tshoot-sql/sql7.png)

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)