---
title: 中小企业应打开文件和打印机共享端口
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 22cd926fdb873538631a6f6850157dceb5a020d7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385830"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>中小企业应打开文件和打印机共享端口


更新日期：2011年2月2日

适用于：Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，windows server 2012，Windows Server 2008 R2

@no__t 0This 主题旨在解决由最佳做法分析器扫描标识的特定问题。只应将本主题中的信息应用到运行文件服务最佳做法分析器的计算机，并遇到本主题中所述的问题。有关最佳做法和扫描的详细信息，请参阅 @ no__t-0[最佳做法分析器](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a)。


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
<td><p>Error</p></td>
</tr>
<tr class="even">
<td><p><strong>类别</strong></p></td>
<td><p>配置</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>问题

> *文件和打印机共享所需的防火墙端口未打开（端口445和139）。*

## <a name="impact"></a>影响

> *计算机将无法访问此服务器上的共享文件夹和其他基于服务器消息块（SMB）的网络服务。*

## <a name="resolution"></a>分辨率

> *启用文件和打印机共享以便通过计算机的防火墙进行通信。*

**Administrators** 组中的成员身份或等效身份是完成此过程所需的最低要求。

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>打开防火墙端口以启用文件和打印机共享

1.  打开 "控制面板"，单击 "**系统和安全**"，然后单击 " **Windows 防火墙**"。

2.  在左窗格中，单击 "**高级设置**"，然后在控制台树中单击 "**入站规则**"。

3.  在 "**入站规则**" 下，找到规则**文件和打印机共享（"NB-会话中"）** 以及 "**文件和打印机共享" （SMB）** 。

4.  对于每个规则，右键单击规则，然后单击 "**启用规则**"。

## <a name="additional-references"></a>其他参考

[了解共享文件夹和 Windows 防火墙](https://technet.microsoft.com/library/cc731402.aspx)（ https://technet.microsoft.com/library/cc731402.aspx)

