---
title: 迁移远程桌面服务客户端访问许可证 (RDS CAL)
description: 本文介绍如何将远程桌面服务客户端访问许可证迁移到新的 Windows Server 2016 许可证服务器。
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
ms.openlocfilehash: c947375b58c0ad88781335b799055e101bd2a193
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447106"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>迁移远程桌面服务客户端访问许可证 (RDS CAL)

有以下三个选项可以迁移 RDS CAL：
1. 自动连接方法：此推荐方法通过 Internet 直接与 Microsoft Clearinghouse 出站（通过 TCP 端口 443）通信。  
2. 使用 Web 浏览器：此方法允许在运行远程桌面授权管理器工具的服务器没有 Internet 连接时进行迁移，但管理员在单独的设备上具有 Internet 连接。 Web 迁移方法的 URL 将显示在“管理 RDS CAL”向导中。 
3. 使用电话：此方法允许管理员通过与 Microsoft 代表通话来完成迁移过程。 相应电话号码由服务器激活向导中所选的国家/地区确定，并显示在“管理 RDS CAL”向导中。

在本文中，[建立 RDS CAL 迁移方法](#establish-rds-cal-migration-method)突出显示在任何 RDS CAL 迁移方法中通用的常规步骤，而[迁移 RDS CAL](#migrate-rds-cals) 突出显示特定于每个迁移方法的步骤。

无论采用何种迁移方法，至少必须是本地管理员组的成员才能执行迁移步骤。

## <a name="establish-rds-cal-migration-method"></a>建立 RDS CAL 迁移方法

1. 在许可证服务器上，打开远程桌面授权管理器  。 （单击“开始”>“管理工具”  。 输入远程桌面服务  目录，并启动远程桌面授权管理器  。）
2. 验证远程桌面许可证服务器的连接方法：右键单击要迁移 RDS CAL 的许可证服务器，然后单击“属性”  。 在“连接方法”  选项卡上，验证连接方法  - 可以在下拉列表菜单中进行更改。 单击“确定”  。
3. 右键单击要迁移 RDS CAL 的许可证服务器，然后单击“管理 RDS CAL”  。
4. 按照“操作选择”  页上的向导中的步骤操作。 单击“将 RDS CAL 从另一个许可证服务器迁移到此许可证服务器”  。
6. 选择迁移 RDS CAL 的原因，然后单击“下一步”  。 您有下列选择：
    - 用此许可证服务器替换源许可证服务器。
    - 源许可证服务器不再运行。
7. 向导中的下一页取决于所选的迁移原因。
    - 如果选择了“用此许可证服务器替换源许可证服务器”  作为迁移 RDS CAL 的原因，则将显示“源许可证服务器信息”  页。
    
       在“源许可证服务器信息”页上，输入源许可证服务器的名称或 IP 地址。

       如果源许可证服务器在网络上可用，请单击“下一步”  。 向导将联系源许可证务服器。 如果源许可证服务器运行的是早于 Windows Server 2008 R2 的操作系统或源许可证服务器已停用，则系统会提醒你必须在完成向导后手动从源许可证服务器中删除 RDS CAL。 确认你已了解此要求之后，将显示“获取客户端许可证密钥包”  页。

       如果源许可证服务器在网络上不可用，请选择“指定的源许可证服务器在网络上不可用”  。 指定源许可证服务器正在运行的操作系统，然后提供源许可证服务器的许可证服务器 ID。 单击“下一步”  后，系统会提醒你必须在完成向导后手动从源许可证服务器中删除 RDS CAL。 确认你已了解此要求之后，将显示“获取客户端许可证密钥包”  页。

    - 如果选择了“源许可证服务器不再运行”  作为迁移 RDS CAL 的原因，则系统会提醒你必须在完成向导后手动从源许可证服务器中删除 RDS CAL。 确认你已了解此要求之后，将显示“获取客户端许可证密钥包”  页。

下一步是迁移 CAL - 使用以下信息来完成该向导。 请注意，向导中看到的内容取决于上述步骤 2 中标识的连接方法。

## <a name="migrate-rds-cals"></a>迁移 RDS CAL

有三种机制可以将许可证迁移到目标许可证服务器；根据步骤 2 中验证的连接方法  继续执行步骤：
  - [自动连接方法](#automatic-connection-method)
  - [使用 Web 浏览器](#using-a-web-browser)
  - [使用电话](#using-a-telephone)

### <a name="automatic-connection-method"></a>自动连接方法

1. 在“许可证计划”  页上，选择购买 RDS CAL 所依据的相应计划，然后单击“下一步”  。
2. 输入所需的信息（通常为许可证代码或协议编号，具体取决于许可证计划  ），然后单击“下一步”  。 请查看你在购买 RDS CAL 时随之附带的文档。
4. 根据你的 RDS CAL 购买协议，选择适合于你的环境的 RDS CAL 的产品版本、许可证类型和数量，然后单击“下一步”  。
5. 这将自动与 Microsoft Clearinghouse 联系并处理您的请求。 之后，RDS CAL 将迁移到许可证服务器上。
6. 单击“完成”  以完成 RDS CAL 迁移过程。

### <a name="using-a-web-browser"></a>使用 Web 浏览器
1. 在“获取客户端许可证密钥包”  页上，单击超链接以连接到远程桌面服务授权网站。
   如果在没有 Internet 连接的计算机上运行远程桌面授权管理器，请记下远程桌面服务授权网站的地址，然后从具有 Internet 连接的计算机连接到该网站。 
2. 在远程桌面服务授权网页上的“选择选项”  下，选择“管理 CAL”  ，然后单击“下一步”  。
3. 提供下列必需信息，然后单击“下一步”  ：
    - **目标许可证服务器 ID**： 一个 35 位数（以 5 个数字为一组），显示在“管理 RDS CAL”向导中的“获取客户端许可证密钥包”  页上。
    - **恢复的原因**： 选择迁移 RDS CAL 的原因。
    - **许可证计划**： 选择用来购买 RDS CAL 的计划。
4. 提供下列必需信息，然后单击“下一步”  ：
   - 姓氏
   - 名字
   - 公司名称
   - 国家/地区

     还可以提供请求的可选信息，例如公司地址、电子邮件地址和电话号码。 在“组织单位”字段中，可以描述此许可证服务器为之提供服务的组织单位。

5. 你在上一页中选择的许可证计划将确定你在下一页上需要提供的信息。 在大多数情况下，你必须提供许可证代码或协议号码。 请查看你在购买 RDS CAL 时随之附带的文档。 此外，还需要指定 RDS CAL 的类型，以及要迁移到许可证服务器的数量。
6. 在输入所需的信息后，请单击“下一步”  。
7. 验证已输入的所有信息是否正确，然后单击“下一步”  以将你的请求提交到 Microsoft Clearinghouse。 之后，网页将显示 Microsoft Clearinghouse 生成的许可证密钥包 ID。

   > [!IMPORTANT] 
   > 请保留许可证密钥包 ID 的副本。 拥有此信息，便于你在恢复 RDS CAL 需要帮助时与 Microsoft Clearinghouse 进行通信。

8. 在相同的“获取客户端许可证密钥包”  页上，输入许可证密钥包 ID，然后单击“下一步”  以将 RDS CAL 迁移到你的许可证服务器。
9. 单击“完成”  以完成 RDS CAL 迁移过程。

### <a name="using-a-telephone"></a>使用电话
1. 在“获取客户端许可证密钥包”  页上，使用显示的电话号码致电 Microsoft Clearinghouse。 为客户服务代表提供远程桌面许可证服务器 ID 以及用来购买 RDS CAL 的授权计划所需的信息。 之后，客户服务代表将处理你的 RDS CAL 迁移请求，并为你提供一个唯一的 RDS CAL ID。 此唯一 ID 称为许可证密钥包 ID  。

   > [!IMPORTANT]
   > 请保留许可证密钥包 ID 的副本。 拥有此信息，便于你在恢复 RDS CAL 需要帮助时与 Microsoft Clearinghouse 进行通信。

2. 在相同的“获取客户端许可证密钥包”  页上，输入许可证密钥包 ID，然后单击“下一步”  以将 RDS CAL 迁移到你的许可证服务器。
3. 单击“完成”  以完成 RDS CAL 迁移过程。
