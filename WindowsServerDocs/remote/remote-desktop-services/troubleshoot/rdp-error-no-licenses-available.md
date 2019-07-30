---
title: 客户端无法连接，出现“没有可用的许可证”错误
description: 排查在进行远程桌面连接时出现“没有可用的许可证”错误的问题
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 540c368812866655452115d8928a915a07cacc97
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529967"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>客户端无法连接，出现“没有可用的许可证”错误

这种情况应用于包含 RDSH 服务器和远程桌面许可服务器的部署。

首先，确定用户看到哪种行为：

- 由于没有可用的许可证或者没有可用的许可证服务器，会话已断开连接。
- 安全错误导致访问被拒绝。

以域管理员身份登录到 RD 会话主机，然后打开 RD 许可证诊断程序。 找到如下所示消息：

  - 远程桌面会话主机服务器的宽限期已过，但未使用任何许可证服务器配置 RD 会话主机服务器。 除非为 RD 会话主机服务器配置了许可证服务器，否则在连接到 RD 会话主机服务器时会被拒绝。
  - 许可证服务器 \<计算机名\> 不可用。 原因可能是出现网络连接问题、远程桌面许可服务已在许可证服务器上停止，或者 RD 许可不可用。

这些问题往往与以下用户消息相关联：

  - 由于此计算机没有可用的远程桌面客户端访问许可证，远程会话已断开连接。
  - 由于没有远程桌面许可证服务器可以提供许可证，远程会话已断开连接。

对于这种情况，请[配置 RD 许可服务](#configure-the-rd-licensing-service)。

如果 RD 许可证诊断程序列出了其他问题，例如“RDP 协议组件 X.224 在协议流中检测到了错误，并已断开连接客户端”，则可能是出现了影响许可证证书的问题。 此类问题往往与如下所示的用户消息相关联：

由于出现安全错误，客户端无法连接到终端服务器。 确保登录到网络后，重新尝试连接到服务器。

对于这种情况，请[刷新 X509 证书注册表项](#refresh-the-x509-certificate-registry-keys)。

## <a name="configure-the-rd-licensing-service"></a>配置 RD 许可服务

以下过程使用服务器管理器进行配置更改。 有关如何配置和使用服务器管理器的信息，请参阅[服务器管理器](../../../administration/server-manager/server-manager.md)

1. 打开服务器管理器并导航到“远程桌面服务”。  
2. 在“部署概述”中，依次选择“任务”、“编辑部署属性”。   
3. 选择“RD 许可”，然后选择部署的相应许可模式（“按设备”或“按用户”）。   
4. 输入 RD 许可证服务器的完全限定域名 (FQDN)，然后选择“添加”。 
5. 如果你有多个 RD 许可证服务器，请针对每个服务器重复步骤 4。 
    ![服务器管理器中的 RD 许可证服务器配置选项。](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>刷新 X509 证书注册表项

> [!IMPORTANT]  
> 请认真遵循本部分的说明。 如果错误地修改了注册表，可能会出现严重问题。 开始修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，这样就可以在出错时还原它。

若要解决此问题，请备份然后删除 X509 证书注册表项，重启计算机，然后重新激活 RD 许可服务器。 按以下步骤操作。

> [!NOTE]
> 在每个 RDSH 服务器上执行以下过程。

下面介绍如何重新激活 RD 许可服务器：

1. 打开注册表编辑器，导航到“HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM”。 
2. 在“注册表”菜单中，选择“导出注册表文件”。 
3. 在“文件名”框中输入 **exported- Certificate**，然后选择“保存”。  
4. 右键单击以下每个值，选择“删除”，然后选择“是”以确认删除：    
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. 退出注册表编辑器并重启 RDSH 服务器。