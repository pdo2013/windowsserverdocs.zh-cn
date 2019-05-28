---
title: 用于 GitHub 常见 Git Bash 命令
description: 某些在 Git Bash 中使用 GitHub 时最常使用的命令的列表。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461690"
---
# <a name="common-git-bash-commands"></a>常见 Git Bash 命令

这些是一些在 Git Bash 中，基于时将用您的内容创建和编辑过程的最常用的命令。

## <a name="master-branch-related"></a>Master 分支相关

必须始终作为基准 master 使用的任何新的分支。

| Command | 描述 |
|---------|-------------|
| `git checkout master` | 从任何其他分支切换到 master |
| `git pull upstream master` | 更新 master 从生产存储库的本地副本 |

## <a name="branch-related"></a>分支相关

| Command | 描述 |
|---------|-------------|
| `git branch` | 查看现有分支 |
| `git checkout -B <name-of-branch>` | 创建一个新的分支 |
| `git checkout <name-of-branch>` | 将更改为另一个分支 |
| `git status` | 检查你的分支中正在运行的内容 |
| `git branch -D <name-of-branch>` | 删除现有分支 （确保您不在其中） |

## <a name="check-in-related-done-as-a-group-in-order"></a>检查在相关 （作为一个组，按顺序执行的）

| Command | 描述 |
|---------|-------------|
| `git add --all` | 保存所做的工作后，将其添加到分支 |
| `git commit -m “public comment, including quotes”` | 将更改提交到你的分支 |
| `git pull upstream master` | 更新 master 从生产存储库的本地副本 |
| `git push origin <name-of-branch>` | 推送到你的本地分支的远程版本 |