---
title: WSUS 消息和疑难解答提示
description: Windows Server Update Service （WSUS）主题-使用 WSUS 邮件进行故障排除
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e432a962662995cf570b28d0b9496594f3e10e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369856"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>WSUS 消息和疑难解答提示

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

本主题包含有关以下 WSUS 消息的信息：

-   "计算机未报告状态"

-   "消息 ID 6703-WSUS 同步失败"

-   "错误0x80070643：安装期间出现严重错误 "

-   "某些服务没有运行。 检查以下服务 [...] "

## <a name="computer-has-not-reported-status"></a>计算机未报告状态
当 WSUS 客户端计算机未向 WSUS 服务器发送信息以指示其当前更新状态时，将在 WSUS 控制台中生成此消息。 此问题通常是由于 WSUS 客户端计算机（而不是 WSUS 服务器）引起的。

最常见的原因如下：

-   计算机断开了与网络的连接：
    -   拔下网络电缆。
    -   介入的网络电缆出现故障。
    -   计算机的网络适配器有故障。
    -   计算机连接到的网络端口已禁用。
    -   无线适配器无法与公司无线接入点建立关联。
-   计算机处于关闭状态。 （它已关闭，或处于睡眠或休眠模式。）

## <a name="message-id-6703---wsus-synchronization-failed"></a>消息 ID 6703-WSUS 同步失败
> 消息：请求失败，HTTP 状态为503：服务不可用。
> 
> 源：Updateservices-api. AdminProxy. createUpdateServer。

当你尝试打开 WSUS 服务器上的更新服务时，会收到以下错误：

> 错误：连接错误
> 
> 尝试连接到 WSUS 服务器时出错。 发生此错误的原因有很多。 如果问题仍然存在，请与网络管理员联系。 单击 "重置服务器" 节点，再次连接到服务器。

除此之外，尝试访问 WSUS 管理网站的 URL （例如 `http://CM12CAS:8530`）会失败，并出现以下错误：

> HTTP 错误503。 服务不可用

在这种情况下，最可能的原因是 IIS 中的 WsusPool 应用程序池处于停止状态。

此外，应用程序池的专用内存限制（KB）可能设置为默认值 1843200 KB。 如果遇到此问题，请将专用内存限制增加到4GB （4000000 KB），然后重新启动应用程序池。 若要增加专用内存限制，请选择 WsusPool 应用程序池，并单击 "编辑应用程序池" 下的 "高级设置"。 然后将专用内存限制设置为4GB （4000000 KB）。 重新启动应用程序池后，请监视 SMS_WSUS_SYNC_MANAGER 组件状态、wcm .log 和 wsyncmgr.log 中的失败情况。 请注意，可能需要将专用内存限制增加到8GB （8000000 KB）或更高，具体取决于环境。

有关更多详细信息，请参阅：[ConfigMgr 2012 中的 WSUS 同步失败，出现 HTTP 503 错误](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>错误0x80070643：安装期间发生严重错误
WSUS 安装程序使用 Microsoft SQL Server 执行安装。 之所以出现此问题，是因为运行 WSUS 安装程序的用户在 SQL Server 中没有系统管理员权限。

若要解决此问题，请向用户帐户或 SQL Server 中的组帐户授予系统管理员权限，然后再次运行 WSUS 安装程序。

## <a name="some-services-are-not-running-check-the-following-services"></a>某些服务没有运行。 检查以下服务：

- **Selfupdate**有关 Selfupdate 服务疑难解答的信息，请参阅[必须更新自动更新](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx)。

- **WSSUService：** 此服务有助于同步。 如果同步出现问题，请访问 WSUSService，方法是单击 "**开始**"，指向 "**管理工具**"，单击 "**服务**"，然后在服务列表中查找 " **Windows Server 更新服务**"。 请执行以下操作：
    
    -   验证此服务是否正在运行。 如果 "已停止" 或 "**重新启动**" 来刷新服务，请单击 "**启动**"。
    
    -   使用事件查看器来检查**应用程序**、 **Securit**y 和**系统**事件日志，以查看是否存在可能指示问题的事件。
    
    -   你还可以检查 SoftwareDistribution 以查看是否存在可能指示问题的事件。

- **Web servicesSQL 服务：** Web 服务承载于 IIS 中。 如果未运行，请确保 IIS 正在运行（或已启动）。 还可以通过在命令提示符处键入**iisreset**来重置 Web 服务。

- **SQL 服务：** 除 selfupdate 服务之外的每个服务都需要 SQL 服务正在运行。 如果有任何日志文件指示 SQL 连接问题，请先检查 SQL 服务。 若要访问 SQL 服务，请单击 "**开始**"，指向 "**管理工具**"，单击 "**服务**"，然后查找下列各项之一：
    
  - **MSSQLSERver** （如果使用的是 WMSDE 或 MSDE，或者使用 SQL Server 并且使用实例名称的默认实例名称）
    
  - **MSSQL $ WSUS** （如果你使用的是 SQL Server 数据库，并且已命名数据库实例 "WSUS"）
    
    右键单击该服务，如果该服务未运行，则单击 "**启动**"; 如果该服务正在运行，则单击 "**重新启动**" 以刷新它。
