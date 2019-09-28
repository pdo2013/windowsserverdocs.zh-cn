---
title: AD FS 故障排除-Fiddler
description: 本文档介绍什么是 Fiddler，以及如何安装和配置 Fiddler 以排查 AD FS 声明问题
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366146"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS 故障排除-Fiddler
Fiddler 是一个工具，可用于捕获 HTTP/HTTPS web 流量。  此工具可用于帮助对声明颁发过程进行故障排除。  通过查看流量，我们可以更好地了解交互的细分位置。  本文档介绍如何安装和设置 Fiddler 以捕获 AD FS 的流量。  有关使用 WS 联合身份验证的示例 fiddler 跟踪，请参阅[AD FS 疑难解答-fiddler](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>下载并安装 Fiddler
可在[此处](https://www.telerik.com/download/fiddler)下载 Fiddler。  下载后，请继续安装。

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>配置 Fiddler 以捕获 AD FS 流量
为了捕获 AD FS 流量，需要将 Fiddler 配置为解密 SSL 流量。 

### <a name="configure-the-fiddler-ssl-certificate"></a>配置 Fiddler SSL 证书
 使用以下过程设置 Fiddler 以解密 SSL 流量。

1.  打开 Fiddler
2.  在顶部的 "**工具**" 下，选择 " **Fiddler 选项**"。
3.  单击 "HTTPS" 选项卡。
4.  勾选 "**解密 HTTPS 流量**"，并仅从下拉的**浏览器**中选择。
5.  选中 "**忽略服务器证书错误**"。
6.  单击 **“确定”** 。

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>后续步骤

- [AD FS 疑难解答](ad-fs-tshoot-overview.md)