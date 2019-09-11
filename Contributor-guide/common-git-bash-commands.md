---
title: 用于 GitHub 的常用 Git Bash 命令
description: 使用 GitHub 时，Git Bash 中的一些最常用命令的列表。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864995"
---
# <a name="common-git-bash-commands"></a>常见的 Git Bash 命令

这些是 Git Bash 中的一些最常用命令，具体取决于你将在内容创建和编辑过程中使用它们的时间。

## <a name="master-branch-related"></a>主分支-相关

对于任何新分支，都必须始终使用 master 作为基准。

| Command | 描述 |
|---------|-------------|
| `git checkout master` | 从任何其他分支切换到 master |
| `git pull upstream master` | 从生产存储库更新 master 的本地副本 |

## <a name="branch-related"></a>与分支相关

| Command | 描述 |
|---------|-------------|
| `git branch` | 查看现有分支 |
| `git checkout -B <name-of-branch>` | 创建新分支 |
| `git checkout <name-of-branch>` | 更改为其他分支 |
| `git status` | 检查分支中发生的情况 |
| `git branch -D <name-of-branch>` | 删除现有分支（确保你不在其中） |

## <a name="check-in-related-done-as-a-group-in-order"></a>签入-相关（按顺序按顺序执行）

| Command | 描述 |
|---------|-------------|
| `git add --all` | 保存工作后，将其添加到分支 |
| `git commit -m “public comment, including quotes”` | 将更改提交到分支 |
| `git pull upstream master` | 从生产存储库更新 master 的本地副本 |
| `git push origin <name-of-branch>` | 推送到本地分支的远程版本 |