---
title: 将所需的元数据标记添加到 Windows Server 相关文章
description: 必须作为元数据标记添加到与 Windows Server 相关的文章顶部的信息的列表。 根据您的报表和团队要求，必需的标记可能会更改。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f0af6b48cd3fd28ae0a15752cb21bfe9a4abf14f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865088"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>将所需的元数据标记添加到 Windows Server 相关文章

在每篇文章的顶部，有一些特定的元数据必须包括在内，以便进行跟踪和 SEO。 根据报告要求，必需的标记可能会更改。 但是，如果需要添加/删除任何字段，则应收到通知。

其外观应如下所示，其中的顶部和底部包含三个连字符（---）：

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they're looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager's Microsoft alias

ms.topic: Type of article, including article, landing-page, get-started-article, or reference

ms.date: Date of change (MM/DD/YYYY)

---

```

## <a name="example"></a>示例

```markdown

---
title: What is Windows Admin Center?
description: Learn about the Windows Admin Center, a locally-deployed, browser-based management tool set that lets you manage your Windows Servers with no Azure or cloud dependency.
ms.prod: windows-server-threshold
ms.reviewer: alainch
author: danielle-github
ms.author: danielle
manager: alainch
ms.topic: article
ms.date: 07/06/2019
---

```