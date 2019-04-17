---
title: 创建的内容服务器数据包用于 Web 和文件内容 （可选）
description: 本指南提供了有关运行 Windows Server 2016 和 Windows 10 的计算机上托管的缓存型部署分支缓存的说明进行操作
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>创建的内容服务器数据包用于 Web 和文件内容 （可选）

>适用于：Windows Server（半年通道），Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以使用此过程 prehash Web 和文件服务器上的内容，，然后创建数据包导入托管的缓存服务器上。 

此过程是可选的因为你无需均为 prehash 和预先加载内容托管的缓存服务器上。 如果你未执行预加载的内容，数据将自动添加到托管缓存通过 WAN 连接的客户端下载它。

该过程将提供为 prehashing 内容文件服务器和 Web 服务器上的说明进行操作。 如果没有这些类型的内容服务器，则你不需要执行该内容服务器类型的说明进行操作。

>[!IMPORTANT]
>执行此过程之前，你必须上安装和配置分支缓存你内容的服务器。 此外，如果你计划更改的内容的服务器上的服务器机密上，执行此操作之前 pre\ 希内容 – 修改服务器幽静使 previously\ 生成哈希无效。

若要执行此步骤，必须是管理员组中的成员。

## <a name="to-create-content-server-data-packages"></a>若要创建内容服务器数据包

1. 每种内容服务器上找到的文件夹和你想要 prehash 数据包中添加的文件。 识别，或者创建你想要保存你的数据包以后在此过程中的文件夹。

2. 在服务器计算机上，打开具有管理员权限的 Windows PowerShell。

3. 执行以下一项或两项操作，具体取决于你所拥有的内容服务器类型：

    > [!NOTE]
    > 值 – 路径参数是你的内容所在的文件夹。 包含你想要 prehash 为一个包添加的数据内容服务器上，必须使用有效的文件夹位置替换示例格中下方的命令。
  
    - 如果你想要 prehash 内容文件服务器上，键入以下命令，，然后按 ENTER。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果你想要 prehash 内容的 Web 服务器上，键入以下命令，然后再次按 ENTER。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 通过运行以下命令，在每个内容服务器上创建数据包。 替换示例值 \(D:\\temp\) – 目标参数与标识，或者创建开始此过程处的位置。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 从内容服务器上，访问你想要预加载的内容，你托管的缓存服务器上的共享，并将数据包复制到托管的缓存服务器上的共享。

若要继续使用本指南，请参阅[导入数据包上托管缓存服务器 #40; 可选 & #41;](9-Bc-Import-Data.md).

