---
title: 排查 ADBA 客户端问题
description: 演示排查 Windows 激活问题的过程
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b4e31cfa892019e4f3bbcd3b67dbb42751cc58dd
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963033"
---
# <a name="example-troubleshooting-active-directory-based-activation-adba-clients-that-do-not-activate"></a>示例：针对基于 Active Directory 的激活 (ADBA) 客户端无法激活的问题进行排查

> [!NOTE]
> 本文最初于 2018 年 3 月 26 日作为 TechNet 博客发布。

大家好！ 我是 Mike Kammer，我在 Microsoft 担任平台 PFE 已经两年多了。 我最近帮助一位客户在其环境中部署了 Windows Server 2016。 我们还利用此机会将其激活方法从 KMS 服务器迁移到[基于 Active Directory 的激活](https://docs.microsoft.com/previous-versions/windows/hh852637(v=win.10))。

作为进行所有更改的正确过程，我们首先在客户的测试环境中进行迁移。 我们按照 Charity Shelbourne 的一篇优秀的博客文章[基于 Active Directory 的激活与密钥管理服务](https://techcommunity.microsoft.com/t5/Core-Infrastructure-and-Security/Active-Directory-Based-Activation-vs-Key-Management-Services/ba-p/256016)中的说明开始部署。 测试环境中的域控制器均运行 Windows Server 2012 R2，因此我们不需要准备林。 我们将角色安装在 Windows Server 2012 R2 域控制器上，并选择了基于 Active Directory 的激活作为批量激活方法。 我们安装了 KMS 密钥，并将其命名为“KMS AD Activation ( ** LAB)”。 我们很大程度上都是按照该博客文章逐步操作。

我们首先构建四个虚拟机，即两个 Windows 2016 Standard 和两个 Windows 2016 Datacenter。 目前一切顺利，每个人都很高兴。 我们构建了运行 Windows 2016 Standard 的物理服务器，并且计算机已正确激活。 这样我们的操作就结束了。

哈哈！ 开个玩笑！ 事情永远不会那么简单。 老实说，设置和配置非常简单，因此该部分简单明了。 我星期一回到办公室，发现上周构建的所有虚拟机都未激活。 喂！ 这不正常！ 我查看了物理计算机，它是正常的。 我与客户联系，讨论发生了什么。 当然，第一个问题是“周末发生了什么变化？” 通常，回答是“无”。 此时，什么变化都没有，我们必须弄清楚这是怎么回事。

我查看其中一个存在问题的服务器，打开了命令提示符，并检查了来自 slmgr /ao-list 命令的输出  。 /ao-list 开关显示 Active Directory 中的所有激活对象  。

![显示 slmgr 命令的图像](./media/032618_1700_Troubleshoo1.png)

![显示 slmgr 命令结果的图像](./media/032618_1700_Troubleshoo2.png)

结果显示我们有两个激活对象：一个用于 Server 2012 R2，一个是新创建的 KMS AD Activation (** LAB)（我们的 Windows Server 2016 许可证）。 这证实了 Active Directory 已正确配置为激活 Windows KMS 客户端

知道 slmgr 命令有助于许可证激活之后，我继续使用不同的选项  。 我试用了 /dlv 开关，它将显示许可证详细信息  。 我觉得这看上去是正常的，我运行的是标准版本的 Windows Server 2016，有一个激活 ID、一个安装 ID、一个验证 URL 以及部分产品密钥。

![显示 slmgr /dlv 的结果的图像](./media/ActivationTroubleshoot2b.jpg)

有人发现我目前遗漏了什么吗？ 在我完成其他问题排查步骤后，我们将重新讨论该问题，但可以肯定地说答案在此屏幕截图中。

我的想法是，由于某种原因，密钥已损坏，因此我使用 /upk 开关，而它会卸载当前密钥  。 虽然这在删除密钥时有效，但通常不是最佳做法。 如果在获取新密钥之前重启服务器，则可能会使服务器处于不佳状态。 我发现使用 /ipk 开关（我将在稍后的问题排查中执行此操作）会覆盖现有密钥，这是一种更安全的方法  。 从失误中吸取教训！

![显示 slmgr /upk 命令及其结果的图像](./media/032618_1700_Troubleshoo3.png)

我再次运行了 /dlv 开关，以查看许可证详细信息  。 遗憾的是，这并未提供任何有用信息，只有一个未找到产品密钥的错误。 因为我刚刚卸载了密钥，因此没有密钥！

![显示 slmgr /dlv 命令及其结果的图像](./media/032618_1700_Troubleshoo4.png)

虽然我觉得成功的可能性不大，但我还是尝试了 /ato 开关，它应该针对已知的 KMS 服务器（或 Active Directory，具体视情况而定）激活 Windows  。 同样，只有一个未找到产品的错误。

![显示 slmgr /ato 命令及其结果的图像](./media/032618_1700_Troubleshoo5.png)

我接下来的想法是，有时在停止后再启动某项服务可能有用，因此我随后尝试了这种方法。 我需要停止后再启动 Microsoft 软件保护平台服务（SPPSvc 服务）。 通过管理命令提示符，我使用可信任的 net stop 和 net start 命令   。 我注意到，刚开始该服务不会运行，因此我认为这就是问题所在！

![显示 net stop 和 net start 命令结果的图像](./media/032618_1700_Troubleshoo6.png)

但这不是。 启动服务并再次尝试激活 Windows 之后，仍然出现未找到产品的错误。

然后，我查看了其中一个存在问题的服务器上的应用程序事件日志。 我发现一个与许可证激活相关的错误（事件 ID 8198），它包含代码 0x8007007B。

![显示事件 ID 8198 详细信息的图像](./media/032618_1700_Troubleshoo7.png)

在查找此代码时，我发现一篇文章，指出我的错误代码意味着文件名、目录名或卷标语法不正确。 通读文章中描述的方法后，似乎没有一种适用于我的情况。 运行 nslookup -type=all _vlmcs._tcp 命令时，我找到了现有的 KMS 服务器（环境中仍有许多 Windows 7 和 Server 2008 计算机，因此有必要保留它）以及五个域控制器  。 这表明不是 DNS 问题，是其他方面的问题。

![显示 nslookup 命令结果的图像](./media/032618_1700_Troubleshoo8.png)

因此我知道 DNS 是正常的。 Active Directory 正确配置为 KMS 激活源。 我的物理服务器已正确激活。 只是 VM 会出现问题吗？ 顺便提一下，有趣的是，我的客户告诉我，另一个部门的某个人也决定构建十几台 Windows Server 2016 虚拟机。 因此，我认为我还要再处理十几台无法激活的服务器。 但情况并非如此！ 这些服务器已正常激活。

我重新查看 slmgr 命令，想方设法激活这些棘手的服务器  。 这次，我将使用 /ipk 开关，这将允许我安装产品密钥  。 我转到[此站点](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11))以获取标准版本的 Windows Server 2016 的相应密钥。 我的一些服务器是数据中心，但我首先需要解决这一个的问题。

![显示 KMS 客户端安装密钥列表的图像](./media/032618_1700_Troubleshoo9.png)

我使用了 /ipk 开关来安装产品密钥，并选择 Windows Server 2016 Standard 密钥  。

![显示 slmgr /ipk 命令及其结果的图像](./media/032618_1700_Troubleshoo10.png)

从现在开始，我只捕获数据中心体验的结果，但它们是相同的。 我使用了 /ato 开关来强制激活  。 我们收到了好消息 - 产品已成功激活！

![显示 slmgr /ato 命令及其结果的图像](./media/032618_1700_Troubleshoo11.png)

再次使用 /dlv 开关时，我们可以看到，目前已由 Active Directory 激活  。

![显示 slmgr /dlv 命令及其结果的图像](./media/032618_1700_Troubleshoo12.png)

现在回顾一下，之前出现了什么问题？ 为什么必须删除已安装的密钥并添加这些通用密钥，才能正确激活这些计算机？ 为什么其他十几台计算机在激活时未出现问题？ 正如我之前所说，我在查找问题的初始阶段遗漏了某些关键内容。 我感到非常困惑，所以通过最初的博客文章联系了 Charity，看看她是否能帮助我。 她立即发现了问题，并帮助我了解了之前遗漏的内容。

当我第一次运行 /dlv 开关时，描述是关键  。 描述是“Windows® 操作系统，零售渠道”。 我看到过这个描述，我认为“零售渠道”意味着该计算机已购买并且具有有效密钥。

![显示 slmgr /dlv 结果的图像，其中突出显示渠道信息](./media/032618_1700_Troubleshoo13.png)

当我们查看正确激活的服务器中的 /dlv 开关输出时，请注意此时的描述是“VOLUME_KMSCLIENT 渠道”  。 这告诉我们，它确实是批量许可证。

![显示 slmgr /dlv 结果的图像，其中突出显示渠道信息](./media/032618_1700_Troubleshoo14.png)

那么零售渠道表示什么呢？ 它表示用于安装操作系统的媒体是 MSDN ISO。 我与客户联系，询问网络上是否可能有第二个 Windows Server 2016 ISO。 结果网络上的确有另一个 ISO，它已用于创建另外的十几台计算机。 他们比较了两个 ISO，并确定提供给我的用于构建虚拟服务器的是 MSDN ISO。 他们从其网络中删除了 MSDN ISO，现在我们已激活所有现有的服务器，无需担心在将来构建时激活失败。

我希望这有所帮助，并为你将来的操作节省时间！

Mike
