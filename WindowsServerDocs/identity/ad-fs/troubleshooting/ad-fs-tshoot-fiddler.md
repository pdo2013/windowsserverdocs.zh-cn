---
title: AD FS 故障排除-Fiddler
description: 本文档介绍什么是 Fiddler 以及如何安装和配置 Fiddler 来对 AD FS 声明问题进行故障排除
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865628"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS 故障排除-Fiddler
Fiddler 是一个工具，可用于捕获 HTTP/HTTPS web 流量。  可以使用此工具以帮助解决声明颁发过程。  通过查看流量中，我们可以获得更好地了解交互分解。  本文档将介绍如何安装和设置 Fiddler 以捕获 AD FS 流量。  使用 WS 联合身份验证的示例 fiddler 跟踪，请参阅[AD FS 进行故障排除-Fiddler 的 WS 联合身份验证](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>下载并安装 Fiddler
您可以下载 Fiddler[此处](https://www.telerik.com/download/fiddler)。  下载后它将继续操作并安装它。

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>配置 Fiddler 以捕获 AD FS 流量
为了捕获 AD FS 通信，我们需要配置 Fiddler 来解密 SSL 流量。 

### <a name="configure-the-fiddler-ssl-certificate"></a>配置 Fiddler SSL 证书
 使用以下过程设置 Fiddler 来解密 SSL 通信。

1.  打开 Fiddler
2.  在顶部下,**工具**，选择**Fiddler 选项**。
3.  单击 HTTPS 选项卡。
4.  勾选**解密 HTTPS 流量**，然后选择**从浏览器仅**从下拉列表。
5.  勾选**忽略服务器证书错误**。
6.  单击 **“确定”**。

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>后续步骤

- [AD FS 进行故障排除](ad-fs-tshoot-overview.md)