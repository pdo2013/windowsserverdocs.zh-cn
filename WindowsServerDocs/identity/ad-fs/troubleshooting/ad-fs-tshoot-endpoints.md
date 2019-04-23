---
title: AD FS 故障排除-AD FS 终结点
description: 本文档介绍如何对 AD FS 终结点进行故障排除
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857558"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS 故障排除-AD FS 的元数据终结点
终结点提供对联合身份验证服务器功能的 AD FS，例如发布的联合身份验证元数据的访问。  若要验证 AD FS 服务器正在响应 web 请求，我们可以检查各个终结点。


## <a name="federation-metadata-test"></a>联合身份验证元数据测试
被动联合身份验证是指在其中您的浏览器重新定向到 AD FS 登录页的方案。  通过测试元数据终结点中，我们可以确定 AD FS 服务器正在响应这些被动方案中的 web 请求。  使用以下过程测试终结点。

1.  使用 web 浏览器，导航到你的 AD FS 联合身份验证元数据终结点。  例如：  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Xml 文件应本地下载到计算机。
3. 将其打开，并验证它包含类似于以下信息的信息：![被动](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>WS-MEX 测试 （活动的测试）
Ws-metadataexchange 是 web 服务协议，并且是 WS 联合身份验证路线图的一部分。  它使用 SOAP 消息请求的元数据。  通过测试终结点中，我们可以确定是否 AD FS 服务器响应 web 请求对 Ws-metadataexchange。  使用以下过程测试终结点。
1.  使用 web 浏览器，导航到你的 AD FS 联合身份验证元数据终结点。  例如：  https://sts.contoso.com/adfs/services/trust/mex
2. Xml 文件应自动显示在浏览器中。  它应类似于下图所示：

![活跃](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)