---
title: Express 更新传递 ISV 支持
description: Windows Server Update Service （WSUS）主题-独立软件供应商（ISV）如何使用 WSUS 配置快速更新交付
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: a4880a1a66d9c722cfda9e194c4eff38c5058674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361721"
---
# <a name="express-update-delivery-isv-support"></a>Express 更新传递 ISV 支持

>适用于：Windows 10、Windows Server 2016

Windows 10 更新下载可能会很大，因为每个包都包含以前发布的所有修补程序，以确保一致性和简洁性。  

自第7版起，Windows 已能够使用名为[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)的功能减小 Windows 更新下载的大小，但使用者设备在默认情况下支持它，windows 10 企业版设备需要 WINDOWS SERVER UPDATE SERVICES （WSUS）才能执行Express 的优势。

## <a name="how-microsoft-supports-express"></a>Microsoft 如何支持 Express

- **独立于 WSUS 的 Express**

    [所有受支持的 WSUS 版本](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx)均已提供快速更新交付。

- **直接连接到 Windows 更新设备上的 Express** 

    消费者设备支持快速下载：它们使用 Windows 更新（WU）客户端来扫描、下载和安装更新。 在下载阶段，WU 客户端请求 Express 包并下载适当的字节范围。

-  **使用[适用于企业的 Windows 更新](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)管理的企业设备**也可受益于 Express 更新传递支持，而无需对配置进行任何更改。

## <a name="how-isvs-can-take-advantage-of-express"></a>Isv 如何利用 Express

Isv 可以使用 WSUS 和 WU 客户端来支持快速更新交付。 Microsoft 建议执行以下三个步骤，下面的部分将对此进行详细介绍：

1.  [**配置 WSUS**](#BKMK_1)

    WSUS 服务器是扫描 & 更新同步所必需的（可在[此处](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx)找到其他信息）

2.  [**指定和填充 ISV 文件缓存**](#BKMK_2)

    建议使用 ISV 文件缓存来托管更新内容，其中包括更新 cabinet 文件（.cab 文件）和 Express 包（psf 文件）。

3.  [**设置 ISV 客户端代理以定向 WU 客户端操作**](#BKMK_3)

>[!NOTE]
>需要安装 Windows 10 版本1607的累积更新（或之后）年1月2017（[KB3213986 （OS Build 14393.693）](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) 。
    
   - ISV 客户端代理确定要批准的更新以及何时下载和安装更新
   - WU 客户端确定要下载的字节范围并启动下载请求

### <a name="BKMK_1"></a>步骤1：配置 WSUS

WSUS 充当接口，用于 Windows 更新和管理描述需要下载的 Express 包的所有元数据。 如果需要部署，请参阅[**Windows Server Update Services 3.0 SP2 的概述**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx)。 部署 WSUS 后，主要注意事项是是否将更新内容存储在 WSUS 服务器本地。 配置 WSUS 时，建议不要在本地存储更新。 这假设你在你的环境中已有软件将这些包定向到部署。 有关如何配置 WSUS 本地存储的详细信息，请参阅[**确定存储更新的位置**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx)。

### <a name="BKMK_2"></a>步骤2：指定和填充 ISV 文件缓存 

#### <a name="specify-the-isv-file-cache"></a>指定 ISV 文件缓存

[**配置服务提供程序参考**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference)中详细说明的新客户端组策略和移动设备管理（MDM）设置定义了 ISV 文件缓存的位置。

| **名称**                                              | **说明**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 配置更新的备用下载位置。 | 指定替代的 intranet 服务器以托管 Microsoft 更新的更新。 然后，你可以使用此更新服务自动更新网络上的计算机 |

设置 ISV 文件缓存的备用下载位置时，有两个选项可供选择：

1. **指定 isv 文件缓存的 ISV HTTP 服务器主机名**
    
    此方法将 WU 客户端配置为对策略中指定的 HTTP 服务器进行下载请求

2. **指定 localhost**
 
    此方法将 WU 客户端配置为向 localhost 发出下载请求。 这允许 ISV 客户端代理处理这些请求并适当地路由以满足下载请求。

> [!IMPORTANT]
> ISV 文件缓存需要以下各项：                                                          
> - 根据 RFC，服务器必须符合 HTTP 1.1：<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> 具体而言，web 服务器需要支持                                                                                                                                                                                                                                      [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)和[**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)请求<br>                                                                                                                                                                                                                                                                                                  -部分范围请求<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -不要使用 "传输编码：分块"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>填充 ISV 文件缓存

必须用与要在被管理的客户端上安装的更新相关联的文件填充 ISV 文件缓存。 

**填充 ISV 文件缓存：**

1. 使用[WSUS api](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx)可访问 MU 服务的更新文件路径和文件名。

    WSUS 服务器上每个更新的元数据都在 Microsoft 更新上包含更新的文件路径和文件名，如下所示（Microsoft 更新以粗体显示，后跟文件路径和文件名）： **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74

2. 从 Microsoft 更新下载文件，并使用以下两种方法之一将文件存储在 ISV 文件缓存中： 

   - 使用**与 MU 服务相同的文件夹路径**存储文件

   - 使用**ISV 定义的文件夹路径**存储文件

     让 HTTP 服务器（或 localhost）将引用 MU 文件夹路径和文件名的**HTTP GET**请求重定向到 ISV 文件位置。

### <a name="BKMK_3"></a>步骤3：设置 ISV 客户端代理以定向 WU 客户端操作

ISV 客户端代理使用以下建议的工作流来协调下载和安装已批准的更新：

1.  ISV 客户端代理调用 WU 客户端来扫描 WSUS 服务器

2.  扫描将适用的更新集返回到 WU 客户端

3.  ISV 客户端确定要批准、下载和安装的更新

4.  ISV 客户端代理调用 WU 客户端以下载已批准的更新

5.  下载更新后，ISV 客户端代理将调用 WU 客户端以安装已批准的更新

有关使用 WU 客户端扫描、下载和安装更新的其他信息，请参阅[搜索、下载和安装更新](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx)。

### <a name="download-workflow-options"></a>下载工作流选项

下面是从 ISV 文件缓存下载工作流选项的两个插图：

![工作流1](../../media/express-update-delivery-isv-support/image1.png)

![工作流2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Express 下载的工作方式

- 对于支持 Express 的操作系统更新，服务上存储了两个版本的文件有效负载：

  - **完整文件版本**-实质上是替换更新二进制文件的本地版本

  - **Express 版本**-包含修补设备上现有二进制文件所需的增量。 

    完整文件版本和 Express 版本都在更新的元数据中引用，后者已作为扫描阶段的一部分下载到客户端。 

    **快速下载的工作方式如下：**

    WU 客户端将首先尝试下载 Express，并且在某些情况下，如果需要，将回退到完整文件（例如，如果要通过不支持字节范围请求的代理）。

  1. 当 WU 客户端启动快速下载时， **wu 客户端将首先下载**作为 express 包一部分的存根。

  2. **WU 客户端将此存根传递到 Windows installer，Windows installer**使用存根来进行本地清点，将设备上文件的增量与所提供文件的最新版本进行比较。

  3. **然后，Windows installer 请求 WU 客户端下载**已确定为必需的范围。

  4. **WU 客户端将下载这些范围并将其传递给 Windows installer**，这将应用范围，然后确定是否需要其他范围。 这将重复进行，直至 Windows installer 通知 WU 客户端已下载了所有必需的范围。

  此时，下载已完成，更新已准备好进行安装。

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>传递优化如何减少带宽消耗

交付优化（DO）是一种自助式分布式缓存解决方案，适用于希望减少操作系统更新、操作系统升级和应用程序的带宽消耗的企业。 允许客户端从备用源（如网络上的其他对等）中将这些元素与指定的下载位置（在此方案中为 ISV 文件缓存）一起下载。

默认情况下，在 Windows 10 企业版和教育版中，只允许在组织自己的网络上进行对等共享，但你可以使用组策略和移动设备管理（MDM）设置以不同的方式对其进行配置。

有关执行的详细信息，请参阅[配置 Windows 10 更新的传递优化](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization)。
