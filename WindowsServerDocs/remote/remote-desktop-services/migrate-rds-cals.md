---
title: 迁移远程桌面服务客户端访问许可证 (RDS CAL)
description: 本文介绍如何将在远程桌面服务客户端访问许可证迁移到新的 Windows Server 2016 许可证服务器。
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 3b809d4d624f21eaeae5a66277db683c95c79bdd
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034442"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>迁移远程桌面服务客户端访问许可证 (RDS CAL)

必须迁移您的 RDS Cal 的三个选项：
1. 自动连接方法：此建议的方法通过 TCP 端口 443 通过 internet 直接访问 Microsoft Clearinghouse 出站通信。  
2. 使用 web 浏览器：运行远程桌面授权管理器工具的服务器不具有 internet 连接，但管理员具有单独的设备上的 internet 连接时，此方法允许迁移。 管理 RDS Cal 向导中显示 Web 迁移方法的 URL。 
3. 使用电话：此方法允许管理员通过与 Microsoft 代表电话完成迁移过程。 按国家/地区，在服务器激活向导中选择并显示在管理 RDS Cal 向导确定适合的电话号码。

在本文中，[建立的 RDS CAL 迁移方法](#establish-rds-cal-migration-method)跨任何 RDS CAL 迁移方法，将突出显示常见的常规步骤虽然[迁移 RDS Cal](#migrate-rds-cals)突出显示了特定于每个迁移步骤方法。

无论哪种迁移方法，至少必须执行迁移步骤的本地管理员组的成员。

## <a name="establish-rds-cal-migration-method"></a>建立 RDS CAL 迁移方法

1. 在许可证服务器上打开**远程桌面授权管理器**。 (单击**开始 > 管理工具**。 输入**远程桌面服务**目录中，并启动**远程桌面授权管理器**。)
2. 验证远程桌面许可证服务器的连接方法： 右键单击你想要迁移 RDS Cal，然后单击该许可证服务器**属性**。 上**连接方法**选项卡上，验证是否**连接方法**-可以在下拉列表菜单中进行更改。 单击 **“确定”** 。
3. 右键单击你想要迁移 RDS Cal，然后单击该许可证服务器**管理 RDS Cal**。
4. 按照向导中的步骤**操作选择**页。 单击**RDS Cal 迁移从另一台许可证服务器向此许可证服务器**。
6. 选择迁移 RDS Cal 的原因，然后单击**下一步**。 您有下列选择：
    - 此许可证服务器将替换为源许可证服务器。
    - 源许可证服务器无法再正常运行。
7. 在向导的下一页取决于所选的迁移原因。
    - 如果选择了**此许可证服务器将替换为源许可证服务器**迁移 RDS Cal 的原因**源许可证服务器信息**显示页。
    
       在源许可证服务器信息页上，输入名称或 IP 地址的源许可证服务器。

       如果源许可证服务器在网络上可用，请单击**下一步**。 向导将联系源许可证服务器。 如果源许可证服务器运行的操作系统早于 Windows Server 2008 R2 或停用源许可证服务器，系统都会提醒，您必须删除 RDS Cal 手动从源许可证服务器向导完成后。 确认你已了解此要求之后,**获取客户端许可证密钥包**页将出现。

       如果源许可证服务器不在网络上可用，请选择**指定的源许可证服务器不是在网络上可用**。 指定操作系统的源许可证服务器正在运行，并提供的许可证服务器 ID 的源许可证服务器。 单击后**下一步**，提醒您，您必须删除 RDS Cal 手动从源许可证服务器向导完成后。 确认你已了解此要求之后,**获取客户端许可证密钥包**页将出现。

    - 如果选择了**源许可证服务器无法再正常运行**迁移 RDS Cal 的原因，会提示您，您必须删除 RDS Cal 手动从源许可证服务器向导完成后。 确认你已了解此要求之后,**获取客户端许可证密钥包**页将出现。

下一步是迁移 Cal-使用以下信息来完成该向导。 请注意，在向导中看到的内容取决于您在上面步骤 2 中标识的连接方法。

## <a name="migrate-rds-cals"></a>迁移 RDS Cal

有三种机制，以将许可证迁移到目标许可证服务器;继续与相对应的步骤**连接方法**验证在步骤 2:
  - [自动连接方法](#automatic-connection-method)
  - [使用 web 浏览器](#using-a-web-browser)
  - [使用电话](#using-a-telephone)

### <a name="automatic-connection-method"></a>自动连接方法

1. 上**许可计划**页上，选择相应的程序通过其购买 RDS Cal，然后单击**下一步**。
2. 输入所需的信息 (通常许可证代码或协议号码，具体取决于**许可计划**)，然后单击**下一步**。 请查阅购买 RDS Cal 时提供的文档。
4. 选择适当的产品版本、 许可证类型和在您的 RDS CAL 购买协议，根据环境的 RDS Cal 的数量，然后单击**下一步**。
5. 这将自动与 Microsoft Clearinghouse 联系并处理您的请求。 RDS Cal 然后迁移到许可证服务器上。
6. 单击**完成**完成 RDS CAL 迁移过程。

### <a name="using-a-web-browser"></a>使用 web 浏览器
1. 上**获取客户端许可证密钥包**页上，单击超链接可连接到远程桌面服务许可 Web 站点。
如果未建立 Internet 连接的计算机上运行远程桌面授权管理器，记下远程桌面服务许可 Web 站点的地址，然后从已建立 Internet 连接的计算机连接到该网站。 
2. 在远程桌面服务许可 Web 页上，在**选择选项**，选择**管理 Cal**，然后单击**下一步**。
3. 提供所需的以下信息，然后单击**下一步**:
    - **目标许可证服务器 ID**: 35 位的数字 5 个数字，将显示在一组**获取客户端许可证密钥包**管理 RDS Cal 向导中的页。
    - **用于恢复的原因**: 选择迁移 RDS Cal 的原因。
    - **许可证计划**: 选择通过其购买 RDS Cal 的程序。
4. 提供所需的以下信息，然后单击**下一步**:
    - 姓
    - 第一个名称或给定名称
    - 公司名称
    - 国家/地区

    您还可以请求，例如公司地址、 电子邮件地址和电话号码的可选信息。 在组织单位字段中，您可以描述此许可证服务器服务在组织内。

5. 您在前一页选择向许可证计划决定你需要在下一页上提供的信息。 在大多数情况下，必须提供许可证代码或协议编号。 请查阅购买 RDS Cal 时提供的文档。 此外，您需要指定哪种类型的 RDS CAL，想要迁移到许可证服务器的数量。
6. 在输入所需的信息后，请单击“下一步”  。
7. 验证所有输入的信息是否正确，然后单击**下一步**你请求提交给 Microsoft Clearinghouse。 Web 页面会显示 Microsoft Clearinghouse 生成的许可证密钥包 ID。

   > [!IMPORTANT] 
   > 保留一份许可证密钥包 id。 此信息与你有利于与 Microsoft Clearinghouse 之间的通信所需恢复 RDS Cal 的帮助。

8. 在同一**获取客户端许可证密钥包**页上，输入许可证密钥包 ID，然后单击**下一步**将迁移到您的许可证服务器的 RDS Cal。
9. 单击**完成**完成 RDS CAL 迁移过程。

### <a name="using-a-telephone"></a>使用电话
1. 上**获取客户端许可证密钥包**页上，使用显示的电话号码致电 Microsoft Clearinghouse。 为客户支持代表提供您的远程桌面许可证服务器 ID 和通过其购买 RDS Cal 的授权计划所需的信息。 客户支持代表处理您的请求将迁移 RDS Cal，然后为的 RDS Cal 提供唯一 ID。 此唯一 ID 称为**许可证密钥包 ID**。

   > [!IMPORTANT]
   > 保留一份许可证密钥包 id。 此信息与你有利于与 Microsoft Clearinghouse 之间的通信所需恢复 RDS Cal 的帮助。

2. 在同一**获取客户端许可证密钥包**页上，输入许可证密钥包 ID，然后单击**下一步**将迁移到您的许可证服务器的 RDS Cal。
3. 单击**完成**完成 RDS CAL 迁移过程。
