---
Title: SMB:文件和打印机共享端口应该已打开
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: d761e4532a5be92d43e09904e9df8f2aa61b6bb8
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63738469"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB:文件和打印机共享端口应该已打开


更新日期：2011 年 2 月 2日日

适用于：Windows Server 2019，Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中，Windows Server 2008 R2

*本主题旨在解决最佳实践分析程序扫描发现的特定问题。您应在本主题中的信息仅适用于计算机的最佳做法分析器针对其运行文件服务，并且遇到问题的本主题。有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a)。


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>操作系统</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>产品/功能</strong></p></td>
<td><p>文件服务</p></td>
</tr>
<tr class="odd">
<td><p><strong>Severity</strong></p></td>
<td><p>错误</p></td>
</tr>
<tr class="even">
<td><p><strong>类别</strong></p></td>
<td><p>配置</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>问题

> *防火墙端口所必需的文件和打印机共享是打开 （端口 445 和 139）。*

## <a name="impact"></a>影响

> *计算机将不能访问共享的文件夹和此服务器上的其他基于服务器消息块 SMB 的网络服务。*

## <a name="resolution"></a>分辨率

> *启用文件和打印机共享，通过计算机的防火墙进行通信。*

**Administrators** 组中的成员身份或等效身份是完成此过程所需的最低要求。

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>若要打开防火墙端口以启用文件和打印机共享

1.  打开控制面板，单击**系统和安全**，然后单击**Windows 防火墙**。

2.  在左窗格中，单击**高级设置**，然后在控制台树中，单击**入站规则**。

3.  下**入站规则**，找到规则**文件和打印机共享 （NB 会话-接程序）** 并**文件和打印机共享 (Smb-in)** 。

4.  对于每个规则，请右键单击规则，然后依次**启用规则**。

## <a name="additional-references"></a>其他参考

[了解共享文件夹和 Windows 防火墙](http://technet.microsoft.com/en-us/library/cc731402.aspx)(http://technet.microsoft.com/en-us/library/cc731402.aspx)

