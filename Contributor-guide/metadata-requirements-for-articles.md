---
title: 将必需的元数据标记添加到您的 Windows Server 相关文章
description: 信息的列表必须添加元数据标记为在 Windows Server 相关的文章的顶部。 所需的标记会有所更改，根据你报告和团队的要求。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: f7c514def1353d44386b1bc53c8cabffe1e31fda
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461645"
---
# <a name="add-the-required-metadata-tags-to-your-windows-server-related-article"></a>将必需的元数据标记添加到您的 Windows Server 相关文章

在每篇文章的顶部，没有特定的元数据必须包含用于跟踪和 SEO 目的的。 所需的标记会有所更改，基于报告要求。 但是，您应通知如果您需要添加/删除的任何字段。

它应如下所示，其中包括三个连字符 （-），在顶部和底部：

```markdown

---
title: The title of the article should go here. This is used in SEO and search results.

description: A description for the article should go here. This is used in search results, to provide users with information about whether the article has the information they’re looking for.

ms.prod: Use this specific text, windows-server-threshold

ms.reviewer: The Microsoft alias for the primary PM for the feature/functionality

author: Your GitHub alias

ms.author: Your Microsoft alias

manager: Your manager’s Microsoft alias

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