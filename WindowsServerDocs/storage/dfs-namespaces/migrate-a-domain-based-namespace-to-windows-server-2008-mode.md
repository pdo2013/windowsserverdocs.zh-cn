---
title: "将基于域的命名空间迁移到 Windows Server 2008 模式"
description: "本文介绍如何将基于域的命名空间迁移到 Windows Server 2008 模式"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ef356e4f2abccb375732b7ec60a374c64dc0e7f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>将基于域的命名空间迁移到 Windows Server 2008 模式

> 适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

对于基于域的命名空间，Windows Server 2008 模式包括支持执行基于访问的枚举和增强可伸缩性。

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>将基于域的命名空间迁移到 Windows Server 2008 模式

若要将基于域的命名空间从 Windows 2000 Server 模式迁移到 Windows Server 2008 模式，必须将该命名空间导出到文件，删除该命名空间，并在 Windows Server 2008 模式下重新创建它，然后导入命名空间字符串。 为此，请按照以下过程操作：

1.  打开命令提示符窗口并键入以下命令，以将命名空间导出到文件。其中，\\\\*domain*\\*namespace* 是相应域和命名空间的名称，而 *path\\filename* 是要导出的文件的路径和文件名：
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  记下每个命名空间服务器的路径 (\\\\*server* \\*share*)。 你必须将命名空间服务器手动添加到重新创建的命名空间，因为 Dfsutil 无法导入命名空间服务器。
3.  在 DFS 管理中，右键单击命名空间，然后再单击**删除**，或者在命令提示符下键入以下命令， <br /> 其中，\\\\*domain*\\*namespace* 是相应域和命名空间的名称：
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  在 DFS 管理中，重新创建同名的命名空间，但使用 Windows Server 2008 模式，或者在命令提示符下键入以下命令，其中， <br /> \\\\*server*\\*namespace* 是命名空间根目录的相应服务器和共享的名称：
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  若要从导出文件导入命名空间，请在命令提示符下键入以下命令，其中， <br /> \\\\*domain*\\*namespace* 是相应域和命名空间的名称，而 *path\\filename* 是要导入的文件的路径和文件名：
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > 若要最大限度减少导入大型命名空间所需的时间，请在命名空间服务器上本地运行 **Dfsutil** 根目录导入命令。
6.  将其余的所有命名空间服务器添加到重新创建的命名空间，方法是在 DFS 管理中右键单击该命名空间，然后单击**添加命名空间服务器**，或者在命令提示符下键入以下命令，其中， <br /> \\\\*server*\\*share* 是命名空间根目录的相应服务器和共享的名称：
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > 你可以在导入命名空间前添加命名空间服务器，但是，这样做可能会导致命名空间服务器以增量方式下载该命名空间的元数据，而不是在添加为命名空间服务器后立即下载整个命名空间。

## <a name="see-also"></a>另请参阅
-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [选择命名空间类型](choose-a-namespace-type.md)