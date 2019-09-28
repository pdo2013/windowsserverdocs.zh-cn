---
title: AD FS 疑难解答-SQL 连接
description: 本文档介绍如何排查 AD FS 的各个方面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09d61292b91c83466f9770184d431b3e6d627dca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385441"
---
# <a name="ad-fs-troubleshooting---sql-connectivity"></a>AD FS 疑难解答-SQL 连接
AD FS 提供将远程 SQL Server 用于 AD FS 场数据的功能。  如果场中的 AD FS 服务器无法与后端 SQL server 通信，则会出现问题。  以下文档将提供一些基本步骤来测试与后端服务器的通信。

## <a name="acquire-the-sql-database-connection-string"></a>获取 SQL 数据库连接字符串
检查 SQL 连接时要测试的第一件事是，如果 AD FS 有正确的 SQL 连接信息。  可以使用 PowerShell 完成此操作。

### <a name="to-acquire-the-sql-connection-string"></a>获取 SQL 连接字符串
1.  打开 Windows PowerShell
2. 输入以下内容： `$adfs = gwmi -Namespace root/ADFS -Class SecurityTokenService` 并按 Enter
3. 输入以下内容： `$adfs.ConfigurationDatabaseConnectionString` 并按 enter。
4. 应会看到连接字符串信息。
![](media/ad-fs-tshoot-sql/sql2.png)

## <a name="create-a-universal-data-link-udl-file-to-test-connectivity"></a>创建用于测试连接的通用数据链接（UDL）文件
通用数据链接文件或 UDL 文件本质上是包含数据库连接字符串的文本文件。  通过使用我们在上面获取的信息，可以测试 SQL server 是否响应连接。

### <a name="to-create-a-udl-file-to-test-connectivity"></a>创建用于测试连接的 udl 文件

1. 打开记事本并将该文件另存为 "test.txt"。  请确保已从下拉菜单中选择了 "**保存为类型**" 的**所有文件**。
2. 双击 "test.txt"
3. 填写以下信息：。 **选择或输入服务器名称：** 使用位于 b 之上的连接字符串中的数据源。 **输入用于登录到服务器的信息：** 使用 AD FS 服务帐户或有权远程登录的帐户。  如果该帐户是 windows 帐户，请使用集成身份验证，否则输入用户名和密码。
    c. **选择服务器上的数据库：** 使用上述字符串中的初始目录。  例如：AdfsConfigurationV3.
   @no__t 0Test 连接 @ no__t-1
1. 单击 "**测试连接**"。</br>
![Success @ no__t-1

## <a name="use-sql-server-management-studio-to-test-connectivity"></a>使用 SQL Server Management Studio 测试连接
你还可以[下载](https://go.microsoft.com/fwlink/?linkid=864329)并安装 SSMS 来测试数据库连接。

### <a name="to-test-connectivity-with-ssms"></a>测试与 SSMS 的连接
1. 下载并安装 SQL Server Management Studio。
![安装](media/ad-fs-tshoot-sql/sql5.png)
1. 打开 SSMS，输入服务器名称。  上面的数据源。
2. 使用 AD FS 服务帐户或有权远程登录的帐户。  如果该帐户是 windows 帐户，请使用集成身份验证，否则输入用户名和密码。
![“连接”](media/ad-fs-tshoot-sql/sql6.png)
1. 你应看到已填充的左侧。  展开 "数据库"，并验证是否显示了 "AD FS 数据库"。
@no__t 0AD FS 数据库 @ no__t-1

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)