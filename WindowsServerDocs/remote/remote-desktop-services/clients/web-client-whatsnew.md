---
title: Web 客户端中的新增功能
description: 了解远程桌面 Web 客户端的最新更改
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/11/2019
ms.localizationpriority: medium
ms.openlocfilehash: d233708c6d206569f42f2a883bd342e90813f6a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404092"
---
# <a name="whats-new-in-the-web-client"></a>Web 客户端中的新增功能

我们会定期更新[远程桌面 Web 客户端](remote-desktop-web-client.md)，添加新功能并修复问题。 可在下面找到最新更新。

> [!NOTE]
> 我们为 Web 客户端更改了版本控制系统。 从版本 1.0.18.0 开始，所有 Web 客户端发行版本都会包含编号（采用“W.X.Y.Z”的格式）。 远程桌面 Web 客户端的发行版号会始终以 0 结尾（例如 W.X.Y.0）。 每个 Windows 虚拟桌面 Web 客户端发行版都会更改最后一个数字，直到下一个远程桌面 Web 客户端发行版（例如 1.0.18.1）。

## <a name="updates-for-version-10180"></a>针对版本 1.0.18.0 的更新
*发布日期：* 2019/5/14

- 在“设置”选项卡中添加了资源启动方法配置，使用户可以在浏览器中打开资源，或下载 .rdp 文件，以处理另一个客户端。 此设置可能由管理员进行配置。有关适用于此功能的管理员配置的详细信息可以在 [Web 客户端设置文档](remote-desktop-web-client-admin.md)中找到。
- 修复了颜色呈现问题，使远程会话中的颜色更鲜艳。
- 修订了与远程资源源错误相关的错误消息。 
- 添加了对更多 office 快捷方式的支持，如选择性粘贴 (Ctrl+Alt+V)。
- 添加了供用户在远程会话中调用 Windows 键的键盘快捷方式 (Alt+F3)
- 更新了针对尝试使用已过期密码进行身份验证的用户的错误消息。
- 刷新了“所有资源”页面上的源 UI。
- 解决了在会话重新连接过程中出现的重叠对话框。
- 修复了资源任务栏中的远程资源图标大小调整。

## <a name="updates-for-version-1011"></a>针对版本 1.0.11 的更新
*发布日期：* 2019/2/22

- 实现了在 Windows Server 2019 中无需 RD 网关即可连接到 RD 代理。
- 按字母顺序排序源（即，RemoteApp 第一个，桌面第二）。
- 修复了多个辅助功能 bug，从而提高了屏幕阅读器兼容性。
- 更新了我们的生成工具。
- 各种 bug 修补程序。

## <a name="updates-for-version-107"></a>针对版本 1.0.7 的更新
*发布日期：* 2019/1/24

- 现在支持在内部网络上脱机使用。
- 改进了非 Microsoft Edge 浏览器上的呈现。
- 实现了对源检索重试尝试的限制以阻止 DoS。
- 修复了辅助功能 bug，使有视觉障碍的用户可以使用 Web 客户端。
- 改进了针对源错误向用户显示的错误消息。
- 添加了 Ctrl + Alt + End (Windows) 和 fn + control + option + delete (Mac) 快捷方式以在远程计算机中调用 Ctrl + Alt + Del。
- 改进了对崩溃事件的遥测。
- 改进了我们的生成管道和生成工具。
- 各种 bug 修补程序。

## <a name="updates-for-version-101"></a>针对版本 1.0.1 的更新
*发布日期：* 2018/10/29

- 在“关于”页面上添加了用于“捕获支持信息”  的选项以诊断问题。
- 现在支持 InPrivate 模式。
- 改进了对非英语键盘的支持。
- 修复了具有非英语字符的工具提示显示不正确的问题。
- 修复了受影响 Chrome 用户的图形呈现问题。
- 使用完整 DST 支持更新了时区重定向。
- 改进了针对内存不足错误的错误消息。
- 各种 bug 修补程序。

## <a name="updates-for-version-100"></a>针对版本 1.0.0 的更新
*发布日期：* 2018/07/16

- 远程桌面 Web 客户端现已公开发布。
- 管理员可以为 Web 客户端全局关闭遥测。
- 各种 bug 修补程序。

## <a name="updates-for-version-090"></a>针对版本 0.9.0 的更新
*发布日期：* 07/05/2018

- Web 客户端中的新登录体验。
- 启动桌面或应用连接（单一登录）时不再提示输入凭据。
- 将 Web 客户端迁移到了新 URL：<https://server_FQDN/RDWeb/webclient/index.html>
- 添加了时区重定向。
- 各种 bug 修补程序。

## <a name="updates-for-version-081"></a>针对版本 0.8.1 的更新
*发布日期：* 2018/05/17

- 用于解决 CVE-2018-0886 中所述的 CredSSP 加密 Oracle 修正的更新。
- 修复了启用打印时某些语言的连接失败。
- 改进了网关不属于部署时的错误消息。
- 添加了“帮助”  和“反馈”  选项。

## <a name="updates-for-version-080"></a>针对版本 0.8.0 的更新
*发布日期：2018 年 3 月 28 日*

- Web 客户端的初始公共预览版。
- 使用 CTRL+C  和 CTRL+V  ，通过剪贴板复制/粘贴文本。
- 打印到 PDF 文件。
- 采用 18 种语言进行了本地化。
 
