---
title: 为 Web 和文件内容创建内容服务器数据包（可选）
description: 本指南说明如何在运行 Windows Server 2016 和 Windows 10 的计算机上以托管缓存模式部署 BranchCache
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 104e3cfd0525c43857bb37d781f6b2475978238e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406385"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>为 Web 和文件内容创建内容服务器数据包（可选）

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

您可以使用此过程来 prehash Web 和文件服务器上的内容，然后创建要导入到托管缓存服务器上的数据包。 

此过程是可选的，因为你不需要在托管缓存服务器上 prehash 和预加载内容。 如果未预加载内容，则当客户端通过 WAN 连接下载数据时，数据会自动添加到托管缓存。

此过程说明了如何在文件服务器和 Web 服务器上执行哈希内容。 如果你没有这种类型的内容服务器，则无需执行该内容服务器类型的说明。

>[!IMPORTANT]
>在执行此过程之前，必须在内容服务器上安装并配置 BranchCache。 此外，如果你计划在内容服务器上更改服务器机密，请在前 @ no__t-0hashing content 之前执行此操作，修改服务器机密会使以前 @ no__t 1generated 哈希失效。

若要执行该过程，你必须是 Administrators 组的成员。

## <a name="to-create-content-server-data-packages"></a>创建内容服务器数据包

1. 在每个内容服务器上，找到要 prehash 的文件夹和文件，并将其添加到数据包。 在此过程中，标识或创建要在其中保存数据包的文件夹。

2. 在服务器计算机上，打开具有管理员权限的 Windows PowerShell。

3. 根据你拥有的内容服务器的类型，执行以下一项或两项操作：

    > [!NOTE]
    > – Path 参数的值是内容所在的文件夹。 您必须将以下命令中的示例值替换为内容服务器上包含要 prehash 并添加到包的数据的有效文件夹位置。
  
    - 如果要 prehash 的内容位于文件服务器上，请键入以下命令，然后按 ENTER。

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   如果要 prehash 的内容在 Web 服务器上，请键入以下命令，然后按 ENTER。

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. 通过在每个内容服务器上运行以下命令来创建数据包。 将– Destination 参数的示例值 \(D： \\temp @ no__t 替换为在此过程开始时标识或创建的位置。

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. 从内容服务器中，访问你要预加载内容的托管缓存服务器上的共享，然后将数据包复制到托管缓存服务器上的共享中。

若要继续学习本指南，请参阅在[托管缓存服务器&#40;上导入&#41;](9-Bc-Import-Data.md)数据包（可选）。

