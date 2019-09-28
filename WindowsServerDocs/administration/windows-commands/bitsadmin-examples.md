---
title: bitsadmin 示例
description: 下面的示例演示如何使用 bitsadmin 工具执行最常见的任务。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c675f08752b3464f7ab1eddd4e9fddf3b16db5f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381768"
---
# <a name="bitsadmin-examples"></a>bitsadmin 示例

下面的示例演示如何使用 `bitsadmin` 工具执行最常见的任务。

## <a name="transfer-a-file"></a>传输文件

**/Transfer**开关是执行下列任务的快捷方式。 此开关将创建作业，将文件添加到作业，激活传输队列中的作业并完成作业。 在传输完成或发生错误之前，BITSAdmin 会继续在 MS-DOS 窗口中显示进度信息。

**bitsadmin/transfer myDownloadJob/download/priority normal `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>创建下载作业

使用 **/create**开关创建名为 myDownloadJob 的下载作业。

**bitsadmin/create myDownloadJob**

BITSAdmin 返回一个用于唯一标识该作业的 GUID。 在后续调用中使用 GUID 或作业名称。 以下文本是示例输出。

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

接下来，使用 **/addfile**开关将一个或多个文件添加到下载作业。

## <a name="add-files-to-the-download-job"></a>将文件添加到下载作业

使用 **/addfile**开关将文件添加到作业。 对要添加的每个文件重复此调用。 如果有多个作业使用 myDownloadJob 作为其名称，则必须将 myDownloadJob 替换为作业的 GUID，以唯一标识作业。

**bitsadmin/addfile myDownloadJob https://downloadsrv/10mb.zip c： \\ 10mb**

若要激活传输队列中的作业，请使用 **/resume**开关。

## <a name="activate-the-download-job"></a>激活下载作业

当你创建新作业时，BITS 会挂起该作业。 若要激活传输队列中的作业，请使用 **/resume**开关。 如果有多个作业使用 myDownloadJob 作为其名称，则必须将 myDownloadJob 替换为作业的 GUID，以唯一标识作业。

**bitsadmin/resume myDownloadJob**

若要确定作业的进度，请使用 **/list**、 **/info**或 **/monitor**开关。

## <a name="determine-the-progress-of-the-download-job"></a>确定下载作业的进度

使用 **/info**开关来确定作业的进度。 如果有多个作业使用 myDownloadJob 作为其名称，则必须将 myDownloadJob 替换为作业的 GUID，以唯一标识作业。

**bitsadmin/info myDownloadJob/verbose**

**/Info**开关返回作业的状态以及传输的文件和字节数。 当传输状态时，BITS 已成功传输作业中的所有文件。 **/Verbose**参数提供作业的完整详细信息。 以下文本是示例输出。

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

若要接收传输队列中所有作业的信息，请使用 **/list**或 **/monitor**开关。

## <a name="completing-the-download-job"></a>完成下载作业

传输作业的状态，则 BITS 已成功传输作业中的所有文件。 但是，这些文件之前将不可用您使用 **/ 完成** 切换。 如果有多个作业使用 myDownloadJob 作为其名称，则必须将 myDownloadJob 替换为作业的 GUID，以唯一标识作业。

**bitsadmin/complete myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>监视传输队列中的作业

使用 **/list**、 **/monitor**或 **/info**开关来监视传输队列中的作业。 **/List**开关提供队列中所有作业的信息。

**bitsadmin/list**

**/List**开关返回作业的状态，以及传输队列中所有作业的文件和字节数。 以下文本是示例输出。

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

使用 **/monitor**开关监视队列中的所有作业。 **/Monitor**开关每隔5秒刷新一次数据。 若要停止刷新，请按 CTRL + C。

**bitsadmin/monitor**

**/Monitor**开关返回作业的状态，以及传输队列中所有作业的文件和字节数。 以下文本是示例输出。

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>从传输队列中删除作业

使用 **/reset**开关从传输队列中删除所有作业。

**bitsadmin/reset**

以下文本是示例输出。

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
