---
title: 为 Web 和文件内容创建内容服务器数据包（可选）
description: 本指南说明了在运行 Windows Server 2016 和 Windows 10 的计算机上的托管的缓存模式下部署 BranchCache
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817168"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>为 Web 和文件内容创建内容服务器数据包（可选）

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

您可以使用此过程对 Web 服务和文件服务器上的内容，然后创建要在托管的缓存服务器上导入的数据包。 

此过程是可选的因为您不需要 prehash 和预加载内容托管的缓存服务器上。 如果不执行预加载内容，数据是自动添加到托管缓存的客户端下载其通过 WAN 连接。

此过程提供了执行哈希文件服务器和 Web 服务器上的内容的说明。 如果没有这些类型的内容服务器，您无需执行该内容服务器类型的说明进行操作。

>[!IMPORTANT]
>在执行此过程之前，必须安装和配置 BranchCache 内容服务器上。 此外，如果你打算更改服务器机密内容服务器上的，这样做之前预\-哈希内容 – 修改服务器机密将使以前失效\-生成哈希值。

若要执行该过程，你必须是 Administrators 组的成员。

## <a name="to-create-content-server-data-packages"></a>若要创建数据的包的内容服务器

1. 在每个内容服务器上，找到的文件夹和你想要对数据的包中添加的文件。 标识或创建用于更高版本在此过程中保存数据包的文件夹。

2. 在服务器上，使用管理员特权打开 Windows PowerShell。

3. 执行一项或两项操作，具体取决于你拥有的内容服务器的类型：

    > [!NOTE]
    > 值为 – 路径参数是你的内容所在的文件夹。 必须将以下命令中的示例值替换有效的文件夹位置为包含你想要对包中添加的数据在内容服务器上。
  
    - 如果你想要对内容的文件服务器上，键入以下命令，然后按 ENTER。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果你想要对内容 Web 服务器上，键入以下命令，然后按 ENTER。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 通过在每个内容服务器上运行以下命令创建的数据包。 示例值替换为\(d:\\temp\) – 目标参数的位置标识或在此过程开始时创建的。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 从内容服务器上，访问你想要预加载内容，在托管的缓存服务器上共享并将数据包复制到托管的缓存服务器上的共享。

若要继续使用本指南，请参阅[在托管缓存服务器上导入数据包&#40;可选&#41;](9-Bc-Import-Data.md)。

