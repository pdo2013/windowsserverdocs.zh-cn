---
title: 排查用户配置文件与事件
description: 如何解决问题加载和卸载用户配置文件使用事件和跟踪日志。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6099dac7d77e37b761785b4f58b6106472e5ba1e
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081917"
---
# <a name="troubleshoot-user-profiles-with-events"></a>排查用户配置文件与事件

>适用于： Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2 和 Windows Server 2016。

本主题讨论如何解决问题加载和卸载用户配置文件使用事件和跟踪日志。 以下各节介绍如何使用三个记录用户配置文件信息的事件日志。

## <a name="step-1-checking-events-in-the-application-log"></a>步骤 1： 检查应用程序日志中的事件

解决问题加载和卸载的用户 （包括漫游用户配置文件） 的配置文件是使用事件查看器来检查任何警告和错误的事件日志中的记录 User Profile Service 应用程序中第一步。

下面是如何在应用程序日志中查看用户配置文件服务事件：

1. 启动事件查看器。 为此，请打开**控制面板**，选择**系统和安全**，然后，在**管理工具**部分中，选择**查看事件日志**。 事件查看器窗口中打开。
2. 在控制台树中，先导航到**Windows 日志**，然后**应用程序**。
3. 在操作窗格中，选择**筛选当前日志**。 筛选当前日志对话框将打开。
4. 在**事件来源**框中，选择**用户配置文件服务**复选框，，然后选择**确定**。
5. 查看留意特定错误事件的事件的列表。
6. 查找值得注意的事件，请选择事件日志联机帮助链接显示的其他信息和故障排除步骤。
7. 若要执行进一步的故障排除，请注意的日期和时间的值得注意的事件，然后检查操作日志 （如在步骤 2 中所述） 查看周围的错误或警告事件的时间，User Profile Service 正在做什么的详细信息。

>[!NOTE]
>您可以放心地忽略 User Profile Service 事件 1530年"Windows 检测到注册表文件仍在使用其他应用程序或服务。"

## <a name="step-2-view-the-operational-log-for-the-user-profile-service"></a>步骤 2： 查看操作日志中的 User Profile Service

如果您不能解决问题使用单独的应用程序日志，请使用以下过程操作日志中查看用户配置文件服务事件。 此日志显示某些服务的内部原理，并查明的配置文件加载或卸载出现问题的过程可帮助。

默认情况下，在所有 Windows 安装启用 Windows 应用程序日志和用户配置文件服务操作日志。

下面是如何查看操作日志中的用户配置文件服务：

1. 在事件查看器控制台树中，导航到**应用程序和服务日志**，则**Microsoft**，然后**Windows**，然后**User Profile Service**，然后**操作**。
2. 检查周围应用程序日志中提到的错误或警告事件的时间发生的事件。

## <a name="step-3-enable-and-view-analytic-and-debug-logs"></a>步骤 3： 启用并查看分析和调试日志

如果您需要比操作日志提供的更多详细信息，您可以启用分析和调试日志在受影响的计算机上。 此日志记录级别更加详细，应禁用除时尝试解决问题。

下面是如何启用和查看分析和调试日志：

1. 在事件查看器的**操作**窗格中，选择**视图**，然后选择**显示分析和调试日志**。
2. 导航到**应用程序和服务日志**，则**Microsoft**，然后**Windows**，然后**用户配置文件服务**，然后**诊断**。
3. 选中**启用日志**，然后选择**是**。 这样的诊断日志，将启动日志记录。
4. 如果您需要更多详细信息，请参阅[步骤 4： 创建和解码跟踪](#step-4:-creating-and-decoding-a-trace)有关如何创建跟踪日志的详细信息。
5. 完成疑难解答问题后，导航到的**诊断**日志、 选择**禁用日志**、 选择**视图**，然后清除**显示分析和调试日志**复选框以隐藏分析和调试日志记录。

## <a name="step-4-creating-and-decoding-a-trace"></a>步骤 4： 创建和解码跟踪

如果您不能解决使用事件的问题，您可以重现该问题，时创建跟踪日志 （ETL 文件），然后对公共符号从 Microsoft 符号服务器进行解码。 跟踪日志提供有关 User Profile Service 执行操作，并可以帮助您查明出现故障的具体信息。

最佳策略时使用 ETL 跟踪是首先捕获可能的最小日志。 一旦日志进行解码，搜索故障的日志。

下面是如何创建和解码的 User Profile Service 的跟踪：

1. 登录到其中用户遇到问题，使用本地 Administrators 组的成员的帐户的计算机。
2. 从提升的命令提示符处输入以下命令，其中*\ < Path\ >* 是到本地文件夹您之前创建，例如 C:\\logs 路径：
        
    ```PowerShell
    logman create trace -n RUP -o <Path>\RUP.etl -ets
    logman update RUP -p {eb7428f5-ab1f-4322-a4cc-1f1a9b2c5e98} 0x7FFFFFFF 0x7 -ets
    ```
3. 从开始屏幕中，选择用户的名称，，然后选择**开关帐户**，注意不要注销管理员。 如果使用远程桌面，关闭管理员会话建立用户会话。
4. 重现该问题。 过程再现问题通常是为用户遇到问题，用户注销，或同时登录。
5. 后重现问题，以本地管理员身份再次登录。
6. 从提升的命令提示符处运行以下命令来保存到 ETL 文件的日志：
  
    ```PowerShell
    logman stop -n RUP -ets
    ```
7. 键入以下命令以将 ETL 文件导出到当前目录 （可能是您的主文件夹或 %WINDIR%\\System32 文件夹） 中的可读文件：
    
    ```PowerShell
    Tracerpt <path>\RUP.etl
    ```
8. 打开**Summary.txt**文件和**Dumpfile.xml**文件 （也可以打开它们在 Microsoft Excel 更轻松地查看日志的完整详细信息）。 查找包括**失败**或已**失败**; 的事件您可以放心地忽略包含**未知**的事件名称的行。

## <a name="more-information"></a>详细信息

* [部署漫游用户配置文件](deploy-roaming-user-profiles.md)