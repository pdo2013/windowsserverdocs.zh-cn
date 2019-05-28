---
title: 新增的远程桌面 web 客户端？
description: 了解远程桌面 web 客户端的最新更改
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 15218af2f084e9c998d89250aace1d763d03b42a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976330"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>新增的远程桌面 web 客户端？

我们会定期更新[远程桌面 web 客户端](remote-desktop-web-client.md)、 添加新功能和修复问题。 请查看以下最新的更新。

   >[!NOTE]
    >我们已为 web 客户端更改版本控制系统。 从版本 1.0.18.0 开始，所有 web 客户端发布版本将都包含数值 （在"W.X.Y.Z"的格式）。 远程桌面 web 客户端的版本号将始终以 0 (例如，W.X.Y.0) 结尾。 每个 Windows 虚拟桌面 web 客户端版本将更改下一步的远程桌面 web 客户端版本 (例如，1.0.18.1) 之前的最后一位数。

## <a name="updates-for-version-10180"></a>针对版本 1.0.18.0 的更新
*发布的日期：5/14/2019*

- 在设置选项卡，使用户能够在浏览器中打开资源，或下载.rdp 文件，以处理与另一个客户端中添加的资源启动方法配置。 此设置可能配置的管理员联系。有关管理员配置此功能可在详细信息[web 客户端安装程序文档](remote-desktop-web-client-admin.md)。
- 在远程会话中的颜色呈现问题，启用更鲜艳的固定的色。
- 与源的远程资源错误相关的已修改的错误消息。 
- 添加了的对更多 office 快捷方式，例如特殊的粘贴 (Ctrl + Alt + V)。
- 用户能够在远程会话 (Alt + F3) 中调用 Windows 键添加的键盘快捷方式
- 尝试使用已过期的密码进行身份验证的用户的更新的错误消息。
- 在所有资源页上刷新的源 UI。
- 会话过程中出现的解决重叠经过重新连接。
- 修复了远程资源图标大小调整资源任务栏中。 

## <a name="updates-for-version-1011"></a>针对版本 1.0.11 的更新
*发布的日期：2/22/2019*

- 已启用连接到 RD 代理，而无需在 Windows Server 2019 RD 网关。
- 按字母顺序排序馈送 (即，RemoteApps 第一个桌面第二个)。
- 修复了提高屏幕读取器兼容性的多个可访问性 bug。
- 更新了我们生成工具。
- 修复了多个 Bug。

## <a name="updates-for-version-107"></a>针对版本 1.0.7 版的更新
*发布的日期：1/24/2019*

- 现在支持在内部网络上的脱机使用。
- 改进了在 Microsoft Edge 浏览器上呈现。
- 实现的限制源的检索重试尝试阻止 DoS。
- 固定的可访问性 bug，使有视觉障碍的用户能够使用 web 客户端。
- 改进了错误消息显示给用户的源的错误。
- 添加了的 Ctrl + Alt + 结束 (Windows) 和 fn + 控件 + 选项 + delete (Mac) 的快捷方式来调用在远程计算机中的 Ctrl + Alt + Del。
- 改进了遥测数据的故障事件。 
- 改进了我们的生成管道和生成工具。
- 修复了多个 Bug。

## <a name="updates-for-version-101"></a>针对版本 1.0.1 的更新
*发布的日期：10/29/2018*

- 已添加到选项**捕获支持信息**上关于页面来诊断问题。
- 现在支持 inPrivate 模式。
- 改进了对非英语键盘的支持。
- 修复了问题，其中包含非英语字符的工具提示显示不正确。
- 修复图形受影响的 Chrome 用户呈现问题。
- 更新完整 DST 支持时区重定向。
- 改进了内存不足错误的错误消息。
- 修复了多个 Bug。

## <a name="updates-for-version-100"></a>针对版本 1.0.0 的更新
*发布的日期：07/16/2018*

- 远程桌面 web 客户端现已公开发布。
- 管理员可以为 web 客户端全局将关闭遥测。
- 修复了多个 Bug。

## <a name="updates-for-version-090"></a>更新为版本 0.9.0 的
*发布的日期：07/05/2018*

- 新的登录体验在 web 客户端。
- 不再提示输入凭据时启动的桌面或应用程序的连接 （单一登录）。
- 移到新 URL 的 web 客户端： **https://server_FQDN/RDWeb/webclient/index.html**
- 添加了的时区重定向。
- 修复了多个 Bug。

## <a name="updates-for-version-081"></a>针对版本 0.8.1 的更新
*发布的日期：05/17/2018*

- 用于解决 CredSSP 加密 oracle 修正 CVE 2018 0886年中所述的更新。
- 修复某些语言中的连接失败时启用了打印功能。
- 网关不是部署的一部分时，改进了的错误消息。
- **帮助**并**反馈**选项已添加。

## <a name="updates-for-version-080"></a>0.8.0 版本开始的更新
*发布的日期：03/28/2018*

- 初始的公共预览版本的 web 客户端。
- 复制/粘贴文本通过使用剪贴板**CTRL + C**并**CTRL + V**。
- 打印到 PDF 文件。
- 18 种语言的本地化。
 
