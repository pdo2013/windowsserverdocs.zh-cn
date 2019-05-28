---
title: 故障排除的激活错误代码
description: 了解如何激活错误代码进行故障排除
ms.topic: article
ms.date: 03/21/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft
ms.localizationpriority: medium
ms.openlocfilehash: 076fc3408810ba7b0f51c6a428955f99a66dd394
ms.sourcegitcommit: 7478dd3959d893bd42620e30e3b56f35407f1dec
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2019
ms.locfileid: "64880875"
---
# <a name="troubleshooting-activation-error-codes"></a>故障排除的激活错误代码

**家庭用户**这篇文章旨在用于通过支持工程师和 IT 专业人员。 如果您正在寻找有关 Windows 激活错误消息的详细信息，请转到以下 Microsoft 网站：

[获取有关 Windows 激活错误的帮助](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors) 

当您尝试使用多个激活密钥 (MAK) 或密钥管理服务 (KMS) 来执行批量激活一个或多个基于 Windows 的计算机上时，你会收到包含特定错误代码的错误消息。 本文介绍如何排查这些错误。

**错误代码和说明**

|错误代码 |错误消息 |激活类型 |可能的原因 |疑难解答步骤 |
|-----------|--------------|----------------|---------------|----------------------|
|    0xC004C001    |    激活服务器确定指定的产品密钥无效    |    MAK    |    输入了无效的 MAK。    |    验证该密钥是由 Microsoft 提供的 MAK。 有关阻止的激活问题，请联系[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)     |
|    0xC004C003     |    激活服务器确定指定的产品密钥被阻止    |    MAK    |    MAK 在激活服务器上被阻止。    |    请联系:[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)以获取新的 MAK，然后安装/激活系统。    |
|    0xC004C008     |    激活服务器确定无法使用指定的产品密钥。    |    KMS    |    KMS 密钥已超过激活限制。    |    KMS 主机密钥将激活最多 10 次六个不同的计算机上。 如果需要激活更多次，请联系[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004B100    |    激活服务器确定无法激活计算机。    |    MAK    |    如果该 MAK 不受支持，则可能会出现此问题。    |    若要解决此问题，请验证使用 MAK 是 Microsoft 提供的 MAK。 若要确认该 MAK 是否有效，请联系:[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C020     |    激活服务器报告多次激活密钥已超出其限制。    |    MAK    |    MAK 已超过激活限制。    |    MAK 在设计上只能激活有限的次数。   请联系[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)。    |
|    0xC004C021     |    激活服务器报告已超过多次激活密钥扩展限制。    |    MAK    |    MAK 已超过激活限制。    |    MAK 在设计上只能激活有限的次数。   请联系[Microsoft 许可激活中心](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers)    |
|    0xC004F009     |    软件保护服务报告宽限期已过期。    |    MAK    |    宽限期过期之前激活系统。 现在，系统处于“通知”状态。    |    请参阅“用户体验”一节。    |
|    0xC004F00F     |    软件授权服务器报告硬件 ID 绑定已超出容差级别。    |    MAK/KMS 客户端/KMS 主机    |    已更改硬件或驱动程序已更新系统上。    |    MAK：重新激活系统 OOT 宽限期使用联机激活或电话激活。      KMS：重新启动，或运行 slmgr.vbs /ato。    |
|    0xC004F014     |    软件保护服务报告产品密钥不可用    |    MAK/KMS 客户端    |    系统上未安装产品密钥。    |    安装 MAK 产品密钥，或者在安装介质上 \sources\pid.txt 中找到的 KMS 安装密钥。    |
|    0xC004F02C     |    软件保护服务报告脱机激活数据的格式不正确。    |    MAK/KMS 客户端    |    系统已检测到电话激活期间输入的数据无效。    |    确认已正确输入 CID。    |
|    0xC004F035     |    此错误代码相当于"软件保护服务报告计算机可能未激活使用批量许可证产品密钥..."错误文本正确，但不明确。 此错误表示计算机缺少中 BIOS — 提供在 OEM 系统，以指示传送，符合条件的版本的 Windows，这是必需的 KMS 客户端激活的计算机上的 Windows 标记。 错误： 无效批量许可证密钥在订单，若要激活，您需要将您的产品密钥更改为有效的多个激活密钥 (MAK) 或零售密钥。 您必须有合格的操作系统许可证和批量许可 Windows 7 升级许可证或完全许可证适用于 Windows 7 零售源的数据。 违反了您的协议和适用的版权法是此软件的任何其他安装。    |    KMS 客户端 /KMS 主机    |    Windows 7 批量版已获得升级仅授权。 不支持不具有合格的操作系统安装的计算机上安装卷操作系统。    |    安装要求的版本的 Microsoft 操作系统，并随后使用 MAK 来激活。    |
|    0xC004F038     |    软件保护服务报告无法激活计算机。 报告你的密钥管理服务 (KMS) 的计数是不够的。 请联系你的系统管理员。    |    KMS 客户端    |    KMS 主机上的计数不够大。 KMS 计数必须为 ≥5 适用于 Windows Server 或 Windows 客户端 ≥ 25 个。    |    若要激活的 KMS 客户端在 KMS 池中需要更多的计算机。 运行 Slmgr.vbs /dli 以获取 KMS 主机上的当前计数。    |
|    0xC004F039     |    软件保护服务报告无法激活计算机。 未启用密钥管理服务 (KMS)。    |    KMS 客户端    |    未应答某个 KMS 请求时发生此错误。    |    对 KMS 主机和客户端之间的网络连接进行故障排除。 请确保 TCP 端口 1688 （默认值） 未被防火墙阻止或否则筛选。    |
|    0xC004F041     |    软件保护服务确定未激活密钥管理服务器 (KMS)。 需要激活 KMS。    |    KMS 客户端    |    未激活 KMS 主机。    |    激活的 KMS 主机使用联机或电话激活。    |
|    0xC004F042     |    软件保护服务已确定不能使用指定的密钥管理服务 (KMS)。    |    KMS 客户端    |    KMS 客户端与 KMS 主机之间不匹配。    |    当 KMS 客户端联系 KMS 主机，不能激活客户端软件时，将发生此错误。 这可以是在包含应用程序和特定于操作系统的 KMS 主机，例如的混合环境中常见的。    |
|    0xC004F050     |    软件保护服务报告产品密钥无效。    |    KMS、KMS 客户端、MAK    |    这可能导致 KMS 密钥拼写错误或通过键入 Beta 密钥已发布版本的操作系统上。    |    在相应版本的 Windows 上安装相应的 KMS 密钥。 检查拼写。 如果密钥复制和粘贴，请确保长破折号线都不已替换为密钥中的短划线。    |
|    0xC004F051     |    软件保护服务报告产品密钥被阻止。    |    MAK/KMS    |    激活服务器上的产品密钥已被 Microsoft 阻止。    |    获取新的 MAK/KMS 密钥、 在系统上安装和激活。    |
|    0xC004F064     |    软件保护服务报告非正版宽限期已过期。    |    MAK    |    Windows 激活工具 (WAT) 已确定系统不是正版。    |    请参阅批量激活操作指南。    |
|    0xC004F065     |    软件保护服务报告应用程序正在运行有效的非正版期限内。    |    MAK/KMS 客户端    |    Windows 激活工具已确定系统不是正版。 系统将继续在非正版宽限期内运行。    |    获取和安装正版产品密钥，并在宽限期内激活系统。 否则，系统将进入通知状态的宽限期结束时。    |
|    0xC004F06C     |    软件保护服务报告无法激活计算机。 密钥管理服务 (KMS) 确定请求时间戳无效。    |    KMS 客户端    |    客户端计算机上的系统时间与 KMS 主机上的时间也不同。    |    时间同步非常重要的原因有多种系统和网络安全。 通过更改系统时间与 KMS 同步在客户端上的修复此问题。 建议使用网络时间协议 (NTP) 时间源或进行时间同步的 Active Directory 域服务。 此问题使用 UTP 时间，并且独立于选择的时区。    |
|    0x80070005     |    拒绝访问。 请求的操作需要提升的权限。    |    KMS 客户端/MAK/KMS 主机    |    用户帐户控制 (UAC) 阻止激活进程在非提升命令提示符下运行。    |    在提升的命令提示符下运行 slmgr.vbs。   右键单击 cmd.exe，然后单击“以管理员身份运行”。    |
|    0x8007232A     |    DNS 服务器失败。    |    KMS 主机    |    系统出现网络或 DNS 问题。    |    排查网络和 DNS 的问题。    |
|    0x8007232B     |    DNS 名称不存在。    |    KMS 客户端    |    KMS 客户端在 DNS 中找不到 KMS SRV RR。 如果 KMS 主机在网络上不存在，则应安装 MAK。    |    确认已安装 KMS 主机，并且 DNS 发布已启用 （默认值）。 如果 DNS 不可用，将 KMS 客户端指向 KMS 主机使用 slmgr.vbs /skms < 名 >。（可选） 获取并安装 MAK;然后，激活系统。 最后，排查 DNS 问题。    |
|    0x800706BA     |    RPC 服务器不可用。    |    KMS 客户端    |    KMS 主机上未配置防火墙设置或 DNS SRV 记录已过时。    |    确保 KMS 主机上启用密钥管理服务防火墙例外。 确保 SRV 记录指向有效的 KMS 主机。 排查网络连接问题。    |
|    0x8007251D     |    对于 DNS 查询找不到记录。    |    KMS 客户端    |    KMS 客户端在 DNS 中找不到 KMS SRV RR。    |    排查网络连接和 DNS 的问题。    |
|    0xC004F074     |    软件保护服务报告无法激活计算机。 无法不联系任何密钥管理服务 (KMS)。 有关其他信息，请参阅应用程序事件日志。    |    KMS 客户端    |    所有 KMS 主机系统都返回错误。    |    对每个事件 id 12288 尝试激活相关联的错误进行故障排除。    |
|    0x8004FE21     |    此计算机未运行正版的 Windows。    |    MAK/KMS 客户端    |    有几个原因可能会出现此问题。 最可能的原因是语言包 (MUI) 已运行的其他语言包未获得许可的 Windows 版本的计算机上进行安装。 （请注意这不一定是相对值的指示被篡改。   某些应用程序可以安装多语言支持该版本的 Windows 不能用于这些语言包，即使。）如果 Windows 已修改了恶意软件，以允许要安装其他功能，也可能出现此问题。 如果某些系统文件已损坏，也可能出现此问题。    |    若要解决此问题，必须重新安装操作系统。    |
|    0x80092328     |    0x80092328 DNS 名称不存在。    |    KMS 客户端    |    如果 KMS 客户端找不到 KMS SRV 资源记录在 DNS 中，可能会出现此问题。    |    若要解决此问题，请按照以下 Microsoft 知识库文章中的步骤操作：929826 出现错误消息时尝试激活 Windows Vista Enterprise、 Windows Vista Business、 Windows 7 或 Windows Server 2008:"代码 0x8007232b"    |
|    0x8007007b    |    0x8007007b DNS 名称不存在。    |    KMS 客户端    |    如果 KMS 客户端找不到 KMS SRV 资源记录在 DNS 中，可能会出现此问题。    |    若要解决此问题，请按照以下 Microsoft 知识库文章中的步骤操作：929826 出现错误消息时尝试激活 Windows Vista Enterprise、 Windows Vista Business、 Windows 7 或 Windows Server 2008:"代码 0x8007232b"    |
|    0x80070490    |您输入的产品密钥不起作用。  检查产品密钥和重试，或输入一个不同。 |MAK |由于输入了无效的 MAK，或由于 Windows Server 2019 中的已知问题，会出现此问题。 |若要解决此问题，激活计算机，使用命令行**slmgr ipk \<5x5 密钥\>**|
