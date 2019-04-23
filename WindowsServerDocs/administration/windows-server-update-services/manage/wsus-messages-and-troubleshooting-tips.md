---
title: WSUS 消息和疑难解答提示
description: Windows Server Update Service (WSUS) 主题-使用 WSUS 消息进行故障排除
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cc24893b1a227501959002ea2d81c62813855d4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883628"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>WSUS 消息和疑难解答提示

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题包含有关以下 WSUS 消息的信息：

-   "计算机尚未报告状态"

-   "消息 ID 6703-WSUS 同步失败"

-   "错误 0x80070643:在安装过程中的错误"

-   "未运行某些服务。 检查以下服务 [...]"

## <a name="computer-has-not-reported-status"></a>计算机尚未报告状态
WSUS 客户端计算机不发送到 WSUS 服务器，用于指示其当前的更新状态的信息时，会在 WSUS 控制台中生成此消息。 WSUS 客户端计算机不是 WSUS 服务器，通常会出现此问题。

最常见原因如下：

-   计算机已失去与网络的连接：
    -   网络电缆被拔出。
    -   有故障的介入性网络电缆。
    -   计算机具有错误的网络适配器。
    -   在计算机连接的网络端口已被禁用。
    -   无线适配器不能将与相关联并连接到公司无线访问点。
-   在计算机处于关闭状态。 （它已关闭或处于睡眠或休眠模式。）

## <a name="message-id-6703---wsus-synchronization-failed"></a>消息 ID 6703-WSUS 同步失败
> 消息：请求失败，HTTP 状态 503:服务不可用。

> 源：Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

当您尝试打开 WSUS 服务器上更新服务时您会收到以下错误：

> 错误：连接错误

> 尝试连接到 WSUS 服务器时出错。 多种原因，可能发生此错误。 请如果问题仍然存在，与网络管理员联系。 单击重置服务器节点，以重新连接到服务器。

除此之外，尝试访问 WSUS 管理网站的 URL (即， http://CM12CAS:8530)失败，出现错误：

> HTTP 错误 503。 服务不可用

在此情况下，最可能的原因是在 IIS 中的 WsusPool 应用程序池处于停止状态。

此外，应用程序池的专用内存限制 (KB) 可能设置为默认值 1843200 KB。 如果遇到此问题，将专用内存限制增加到 4 GB (4000000 KB)，并重新启动应用程序池。 若要增加专用内存限制，请选择 WsusPool 应用程序池，并单击高级设置下编辑应用程序池。 然后将专用内存限制设置为 4 GB (4000000 KB)。 在重新启动应用程序池后，监视的 SMS_WSUS_SYNC_MANAGER 组件状态、 wcm.log 和失败的 wsyncmgr.log。 请注意，可能需要根据环境增加专用内存限制为 8 GB (8000000 KB) 或更高版本。

有关其他详细信息，请参阅：[ConfigMgr 2012 中的 WSUS 同步失败，出现 HTTP 503 错误](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>错误 0x80070643:安装时发生严重错误
WSUS 安装程序使用 Microsoft SQL Server 来执行安装。 因为在运行 WSUS 安装程序的用户不具有 SQL Server 中的系统管理员权限，将发生此问题。

若要解决此问题，授予系统管理员权限的用户帐户或 SQL Server 中的组帐户，然后再次运行 WSUS 安装程序。

## <a name="some-services-are-not-running-check-the-following-services"></a>某些服务未运行。 检查以下服务：

- **自更新：** 请参阅[自动更新必须更新](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx)有关故障排除 Selfupdate 服务的信息。

- **WSSUService.exe:** 此服务有助于同步。 如果必须同步的问题，通过单击访问 WSUSService.exe**启动**，依次指向**管理工具**，再单击**Services**，然后查找**Windows Server Update Service**在服务列表中。 请执行以下操作：
    
    -   验证此服务正在运行。 单击**启动**如果已停止或**重新启动**可以刷新服务。
    
    -   使用事件查看器来检查**应用程序**， **Securit**y，并且**系统**事件日志以查看是否存在任何可能表示存在问题的事件。
    
    -   此外可以检查 <wsusinstallationfolder>\logfiles 以查看是否有可能表示存在问题的事件。

- **Web servicesSQL 服务：** 在 IIS 中承载 web 服务。 如果它们未运行，请确保 IIS 已运行 （或已开始）。 此外可以尝试通过键入来重置 Web 服务**iisreset**在命令提示符。

- **SQL 服务：** 除了 selfupdate 服务的每个服务要求 SQL 服务正在运行。 如果任何日志文件指示 SQL 连接问题，请先检查 SQL 服务。 若要访问 SQL 服务，请单击**启动**，依次指向**管理工具**，单击**Services**，然后查找以下项之一：
    
    -   **MSSQLSERver** （如果使用的 WMSDE 或 MSDE 中，或者如果您使用 SQL Server，并且使用实例名称的默认实例名称）
    
    -   **MSSQL$ WSUS** （如果您使用的是 SQL Server 数据库和"WSUS"具有命名的数据库实例）
    
    右键单击该服务，然后依次**启动**如果服务未运行，或**重新启动**时将刷新该服务正在运行。
