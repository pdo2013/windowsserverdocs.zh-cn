---
title: bitsadmin 示例
description: 以下示例演示如何使用 bitsadmin 工具执行最常见的任务。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: a98e1a876c972b0f146ff37aff0a77399b684e99
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308563"
---
# <a name="bitsadmin-examples"></a>bitsadmin 示例

下面的示例演示如何使用`bitsadmin`工具执行最常见的任务。

## <a name="transfer-a-file"></a>将文件传输

**/传输**开关是执行下面列出的任务的快捷方式。 此开关创建作业，可将文件添加到作业，激活的传输队列中的作业，并完成作业。 BITSAdmin 继续在 MS-DOS 窗口中显示进度信息，直到传输完成或发生错误。

**bitsadmin /transfer myDownloadJob /download /priority 正常 `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>创建下载作业

使用 **/ 创建**交换机，创建一个名为 myDownloadJob 的下载作业。

**bitsadmin /create myDownloadJob**

BITSAdmin 返回唯一标识该作业的 GUID。 在后续调用中使用的 GUID 或作业的名称。 以下文本是示例输出。

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

接下来，使用 **/addfile**开关来将一个或多个文件添加到下载作业。

## <a name="add-files-to-the-download-job"></a>将文件添加到下载作业

使用 **/addfile**开关来将文件添加到作业。 对于你想要添加每个文件重复此调用。 如果多个作业使用 myDownloadJob 作为它们的名称，必须将 myDownloadJob 替换为要唯一标识作业的作业的 GUID。

**bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

若要激活的传输队列中的作业，请使用 **/恢复**切换。

## <a name="activate-the-download-job"></a>激活下载作业

在创建新的作业时，BITS 将暂停作业。 若要激活的传输队列中的作业，请使用 **/恢复**切换。 如果多个作业使用 myDownloadJob 作为它们的名称，必须将 myDownloadJob 替换为要唯一标识作业的作业的 GUID。

**bitsadmin /resume myDownloadJob**

若要确定作业的进度，请使用 **/list**， **/info**，或 **/监视**切换。

## <a name="determine-the-progress-of-the-download-job"></a>确定下载作业进度

使用 **/info**开关，用于确定作业的进度。 如果多个作业使用 myDownloadJob 作为它们的名称，必须将 myDownloadJob 替换为要唯一标识作业的作业的 GUID。

**bitsadmin /info myDownloadJob /verbose**

**/Info** switch 返回的文件和传输的字节数和作业的状态。 当传输状态时，则 BITS 已成功传输作业中的所有文件。 **/Verbose**自变量提供的作业的完整详细信息。 以下文本是示例输出。

``` syntax
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being 
processed.
DESCRIPTION:
JOB FILES:
        0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

若要接收的传输队列中的所有作业的信息，请使用 **/list**或 **/监视**切换。

## <a name="completing-the-download-job"></a>完成下载作业

传输作业的状态，则 BITS 已成功传输作业中的所有文件。 但是，这些文件之前将不可用您使用 **/ 完成** 切换。 如果多个作业使用 myDownloadJob 作为它们的名称，必须将 myDownloadJob 替换为要唯一标识作业的作业的 GUID。

**bitsadmin /complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>在传输队列中监视作业

使用 **/list**， **/监视**，或 **/info**开关来监视作业的传输队列中。 **/List**交换机提供的队列中的所有作业的信息。

**bitsadmin /list**

**/List** switch 返回的文件和传输队列中的所有作业传输的字节数和作业的状态。 以下文本是示例输出。

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

使用 **/监视**开关来监视队列中的所有作业。 **/监视**交换机刷新的数据每隔 5 秒。 若要停止刷新，请输入 CTRL + C。

**bitsadmin /monitor**

**/监视**switch 返回的文件和传输队列中的所有作业传输的字节数和作业的状态。 以下文本是示例输出。

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>从传输队列中删除作业

使用 **/重置**开关来从传输队列中删除所有作业。

**bitsadmin /reset**

以下文本是示例输出。

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
