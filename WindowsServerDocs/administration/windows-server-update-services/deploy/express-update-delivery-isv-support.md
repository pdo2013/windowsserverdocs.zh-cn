---
title: Express 更新传递 ISV 支持
description: Windows Server Update Service (WSUS) 主题-如何独立的软件供应商 (ISV) 可以配置使用 WSUS Express 更新交付
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: b891f61ff2c930591c33805d0e3bc595ebf196f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850488"
---
#<a name="express-update-delivery-isv-support"></a>Express 更新传递 ISV 支持

>适用于：Windows 10、windows Server 2016

Windows 10 更新的下载可能很大，因为每个包包含所有先前发布的修补程序，以确保一致性和简洁性。  

之后的版本 7，Windows 便提供了名为的功能，降低 Windows 更新下载的大小[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)，并且尽管消费型电子设备支持它默认情况下，Windows 10 企业版设备需要 Windows Server Update服务 (WSUS)，以利用 Express。

## <a name="how-microsoft-supports-express"></a>Microsoft 如何支持 Express

- **在 WSUS 独立上 express**

    快速更新交付已[可在所有支持的 WSUS 版本上](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx)。

- **在设备直接连接到 Windows 更新上 express** 

    使用者设备支持快速下载： 他们使用 Windows Update (WU) 客户端以扫描、 下载和安装更新。 在下载阶段 WU 客户端请求 Express 包并下载相应的字节范围。

-  **使用[适用于企业的 Windows 更新](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)管理的企业设备**也可受益于 Express 更新传递支持，而无需对配置进行任何更改。

##<a name="how-isvs-can-take-advantage-of-express"></a>Isv 可以如何充分利用 Express

Isv 可以使用 WSUS 和 WU 客户端来支持快速更新传递。 Microsoft 建议以下三个步骤，每个在以下各节中更详细地讨论：

1.  [**将 WSUS 配置**](#BKMK_1)

    WSUS 服务器是必需的扫描和更新同步 (可以找到更多信息[此处](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**指定并填充 ISV 文件缓存**](#BKMK_2)

    建议使用 ISV 文件缓存来承载更新的内容，其中包括更新 cabinet 文件 （.cab 文件），快速打包 （.psf 文件）。

3.  [**设置为定向 WU 客户端操作的 ISV 客户端代理**](#BKMK_3)

>[!NOTE]
>需要累积更新的 Windows 10 版本 1607年版本中 （或之后） 2017 年 1 月 ([KB3213986 (OS 生成 14393.693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693)安装。
    
   - ISV 客户端代理确定哪些更新批准和时下载和安装更新
   - WU 客户端确定要下载的字节范围，并启动下载请求

### <a name="BKMK_1"></a>步骤 1:将 WSUS 配置

WSUS 用作到 Windows 更新的接口，并管理描述需要将下载的 Express 包的所有元数据。 如果你需要部署，请参阅[**概述的 Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx)。 WSUS 部署后，主要考虑因素是在 WSUS 服务器上本地存储更新内容。 在配置 WSUS，我们建议不本地存储更新。 这假设已将这些包的部署定向你的环境中的软件。 有关如何配置 WSUS 本地存储的详细信息，请参阅[**确定位置到应用商店更新**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx)。

### <a name="BKMK_2"></a>步骤 2:指定并填充 ISV 文件缓存 

####<a name="specify-the-isv-file-cache"></a>指定 ISV 文件缓存

新客户端的组策略和移动设备管理 (MDM) 设置中详细介绍[**配置服务提供程序参考**](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference)定义 ISV 文件缓存的位置。

| **名称**                                              | **说明**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 配置备用下载位置的更新。 | 从 Microsoft 更新主机更新为指定的备用 intranet 服务器。 然后可以使用此更新服务以自动更新的计算机在网络上 |

设置 ISV 文件缓存的备用下载位置时，有两个选项：

1. **指定 ISV HTTP 服务器主机名**，这是 ISV 文件缓存
    
    这种方法配置 WU 客户端，请下载到策略中指定的 HTTP 服务器的请求

2. **指定 localhost**
 
    这种方法配置 WU 客户端，请下载到本地主机的请求。 这允许 ISV 客户端代理，以处理这些请求和根据需要完成下载请求的路由。

>[!IMPORTANT]
>ISV 文件缓存需要以下项：                                                          
                                                                                                                                   >- 服务器必须是 HTTP 1.1 规范按照 RFC: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
                                                                                                                                   >具体而言，需要 web 服务器以支持                                                                                                                                                                                                                                      [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)并[**获取**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)请求<br>                                                                                                                                                                                                                                                                                                  分部范围请求<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -保持活动状态<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -不使用"Transfer-encoding: chunked？                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>填充 ISV 文件缓存

ISV 文件缓存必须填充要在托管客户端上安装的更新与关联的文件。 

**若要填充 ISV 文件缓存：**

1. 使用[WSUS Api](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx)访问更新的文件路径和文件名为 MU 服务。

    WSUS 服务器上每个更新的元数据包含在更新的文件路径和 Microsoft Update 上的文件名称，如下所示 (Microsoft Update 中的主机名以粗体显示后, 跟文件路径和文件名): **http://download.windowsupdate.com** /c/msdownload/更新 /software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. 从 Microsoft 更新下载文件并将其存储在 ISV 文件缓存使用这两种方法之一： 

 - 使用的存储文件**作为 MU 服务上的同一文件夹路径**

 - 使用的存储文件**ISV 定义文件夹路径**

    具有 HTTP 服务器 （或 localhost） 重定向**HTTP GET**引用 MU 文件夹路径和文件名的名称，为 ISV 文件位置的请求。

### <a name="BKMK_3"></a>步骤 3:设置为定向 WU 客户端操作的 ISV 客户端代理

ISV 客户端代理会安排下载和使用以下建议的工作流的已批准更新的安装：

1.  ISV 客户端代理调用 WU 客户端会对 WSUS 服务器进行扫描

2.  扫描到 WU 客户端返回适用的更新组

3.  ISV 客户端确定要批准、 下载和安装的更新

4.  ISV 客户端代理调用 WU 客户端下载批准的更新

5.  ISV 客户端代理下载更新后调用 WU 客户端安装已批准的更新

请参阅[搜索、 下载，以及安装更新](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387102(v=vs.85).aspx)有关使用 WU 客户端要扫描的其他信息，下载并安装更新。

### <a name="download-workflow-options"></a>下载工作流选项

以下是两个插图的下载 ISV 文件缓存中的工作流选项：

![工作流 1](../../media/express-update-delivery-isv-support/image1.png)

![工作流 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Express 下载的工作方式

- 对于支持 Express 的操作系统更新，服务上存储了两个版本的文件有效负载：

 - **完整文件版本**-实质上替换为更新二进制文件的本地版本

 - **速成版**-包含增量数据所需修补在设备上现有的二进制文件。 

   作为扫描阶段的一部分下载到客户端的更新的元数据中引用的完整文件版本和 Express 版本。 

   **快速下载工作原理如下：**

   WU 客户端将尝试下载 Express 首先，并在某些情况下 fall 返回到完整文件根据需要 （例如，如果将通过代理服务器不支持字节范围请求）。

  1. 当 WU 客户端启动 Express 下载**WU 客户端首先会下载一个存根**，这是 Express 包的一部分。

  2. **WU 客户端将此存根 （stub） 传递给 Windows 安装程序**，它使用存根 （stub） 来执行本地清单，比较在设备上使用所需转到所提供的文件的最新版本的文件的增量。

  3. **Windows 安装程序然后请求 WU 客户端下载范围**已确定这是必需的。

  4. **WU 客户端下载这些范围，并将其传递给 Windows 安装程序**，其中应用范围，然后确定是否需要其他范围。 此重复，直到 Windows 安装程序会告知 WU 客户端已下载所有必要的范围。

  此时，下载已完成，更新已准备好进行安装。

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>传递优化如何降低带宽消耗

传递优化 (DO) 是一个自我管理的分布式的缓存解决方案，希望减少带宽消耗操作系统更新、 操作系统升级和应用程序。 允许客户端下载替代源 （例如在网络上的其他对等节点） 中的这些元素与指定的下载位置 （ISV 文件缓存在此方案中） 结合使用。

默认情况下，在 Windows 10 企业版和教育版不要允许对等共享上组织自己的网络，但你可以配置它以不同的方式使用组策略和移动设备管理 (MDM) 设置。

请参阅[配置传递优化适用于 Windows 10 更新](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization)有关执行操作的详细信息。
