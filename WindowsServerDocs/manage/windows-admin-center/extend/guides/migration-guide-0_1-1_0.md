---
title: 从 Windows Admin Center SDK 0.1 到 1.0 迁移
description: 本指南将帮助你从 Windows Admin Center SDK 版本 0.1 到 1.0 迁移
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116581"
---
# 从 Windows Admin Center SDK 0.1 到 1.0 迁移

>适用于： Windows Admin Center 预览版

本指南将帮助你从 Windows Admin Center SDK 版本 0.1 到 1.0 迁移。  

## 1.了解有关开发人员指南扩展名的新控件

Windows Admin Center 1902 及更高版本包括**开发人员指南**扩展，它可用于查找的控件 （包括新可用控件） 和可帮助你生成自己的扩展应用场景的示例。  开发人员指南将替换从较早版本的 SDK 的**开发人员工具**扩展。

### 在 Windows Admin Center 中使用开发人员指南

开发人员指南可用于为在 Windows Admin Center 版本 1902年解决方案及更高版本。  开发人员指南已预安装的但它需要启用通过设置。

**启用 Windows Admin Center 中的开发人员指南：**

* 打开 Windows Admin Center （1902 及更高版本）
* 单击窗口右上角中的**设置**图标
* 选择**高级**选项卡
* 在*实验键*，单击**添加**
* 输入新值```msft.sme.shell.devguide```由在上一步创建的空字段
* 单击**保存并重新加载**

**在 Windows Admin Center 中打开开发人员指南：**

* 打开 Windows Admin Center （1902 及更高版本）
* 单击下拉列表中顶部左侧显示所有解决方案类型
* 选择的**开发人员指南**解决方案 
    * 如果你没有看到列出的解决方案，请确保你已启用开发人员指南 （请参阅上面部分） 并且已重新加载 Windows Admin Center。
* 通过选择一个选项卡中浏览内容的开发人员指南
    * **登录：** 包含用于*管理方式*和*通知*方案的代码示例
    * **控件：** 包含在 SDK 中每个可用控件的示例
    * **管道：** 包含的可用的转换器和格式化程序函数的示例
    * **样式：** 包含的 CSS 样式 SDK 中提供的示例
    * **MsftSme:** 包含示例和高级方案指南 

### 浏览 GitHub 上的开发人员指南的源代码

你可以浏览以查找 HTML、 CSS 和 TypeScript 代码示例的示例在 GitHub 上的开发人员指南的[源代码](https://github.com/Microsoft/windows-admin-center-sdk/)。

## 2.为最新的 SDK 准备开发环境

安装或更新 node.js 版本[10.15.1 LTS 或更高版本](https://nodejs.org/en/)。

更新到最新版本的 Windows Admin Center CLI:

[//]: # "npm 卸载-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

更新你的全局依赖项到这些版本：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3.创建新项目使用最新的 SDK

使用 Windows Admin Center CLI 创建新的项目面向```next```版本 (SDK 1.0):

[//]: # "wac 创建-公司 Contoso Inc-管理 Foo 的工作原理-版本实验性工具"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下来，将目录更改到刚创建的文件夹，然后通过运行安装所需的本地依赖项```npm install ```。

## 4.修改现有项目以使用最新的 SDK

重要提示： 进行在继续之前备份你的项目。

修改中的以下行```package.json```目标```next```版本 (SDK 1.0):

[//]: # "@microsoft/windows-admin-center-sdk: 实验"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然后运行```npm install```更新整个项目的引用。

## 5.使用 SDK CLI 解决迁移的常见问题

重要提示： 进行在继续之前备份你的项目。

从你的项目的根文件夹，在你的项目以自动修复常见迁移问题上运行以下 CLI 命令：

``` cmd
wac updateSeven --update
```

此 CLI 命令自动解决以下问题：

* 重新生成 ```package-lock.json```
* 更新角度编译环境中的文件：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6.使用 SDK CLI 来了解迁移的常见问题

从你的项目的根文件夹中运行以下 CLI 命令审核你的项目，并查找需要手动解决的常见迁移问题：

``` cmd
wac updateSeven --audit
```

这将在你的项目中查找以下问题的实例：

### 使用这些 sme 类替换以下 CSS 类的用法：

| 旧 CSS 类 | 新的 CSS 类 |
| -- | -- |
| .auto 弹性大小 |  .sme 位置弹性自动 |
| .border 所有 |  .sme 边框嵌入 sm 和.sme-边框的颜色的基本-90 |
| .border 底部 |  .sme 边框底部 sm 和.sme-border-bottom-color-base-90 |
| .border 水平 |  .sme 边框水平 sm 和.sme-border-horizontal-color-base-90 |
| .border 左 |  .sme 边框左 sm 和.sme-border-left-color-base-90 |
| .border 右 |  .sme 边框右 sm 和.sme-border-right-color-base-90 |
| .border 顶部 |  .sme 边框顶部 sm 和.sme-border-top-color-base-90 |
| .border 垂直 |  .sme 边框垂直 sm 和.sme-border-vertical-color-base-90 |
| .break word |  .sme 排列的 ws-换行 |
| .btn |  .sme 按钮或按钮 |
| .btn 主要 |  .sme button.sme 按钮主要 OR.button.sme 的按钮的主要 |
| .color 深色 |  .sme 颜色 alt |
| .color 光 |  .sme 颜色基础 |
| .color 浅灰色 |  .sme 颜色基本 90 |
| .fixed 弹性大小 |  .sme-位置的弹性-无 |
| .flex 布局 |  .sme-排列-堆栈-h 或.sme-排列-堆栈-v |
| .font 粗体 |  强调-1 字体.sme |
| .highlight |  .sme 背景颜色黄色 |
| .horizontal |  .sme-排列-堆栈-h |
| 无滚动 |  .sme 位置弹性自动 |
| .nowrap |  .sme-排列-堆栈-h 或.sme-排列-堆栈-v |
| .relative |  .sme 布局相对 |
| .relative 中心 |  .sme 布局绝对.sme 位置中心 |
| .reverse |  .sme-排列的堆栈的反转 |
| .stretch 绝对 |  .sme-布局的绝对.sme-位置的嵌入-无 |
| .stretch 固定 |  .sme 布局固定.sme-位置的嵌入-无 |
| .stretch 垂直 |  .sme 位置 stretch v |
| .stretch 宽度 |  .sme 位置 stretch h |
| .vertical |  .sme-排列-堆栈-v |
| 仅.vertical 滚动 |  .sme-排列-溢出的隐藏-sme 的排列-溢出-自动-x-y |
| .wrap |  .sme-排列-wrapstack-h 或.sme-排列-wrapstack-v |

### 替换这些 sme 组件使用以下组件：

| 旧组件 | 新的组件 |
| -- | -- |
| .alert |  sme 警报 |
| .alert 危险 |  sme 警报 |
| .breadCrumb |  sme 警报 |
| .checkbox |  sme 窗体字段 [类型 ="复选框"] |
| .combobox |  sme 窗体字段 [类型 ="选择"] |
| .dashboard |  sme 布局的内容的区域的填充 sme-排列-堆栈-h |
| .details 面板 |  sme 属性网格 |
| .details 面板容器 |  sme 属性网格 |
| .details 选项卡 |  sme 属性网格 OR sme 透视表 |
| .details 包装器 |  sme 属性网格 |
| .disabled |  sme 禁用 |
| .form 按钮 | sme 窗体字段 |
| .form 控件 | sme 窗体字段 |
| .form 控件 | sme 窗体字段 |
| .form 组 | sme 窗体字段 |
| .form 组标签 | sme 窗体字段 |
| .form 输入 | sme 窗体字段 |
| .form stretch | sme 窗体字段 |
| .input 文件 | sme 窗体字段 |
| .nav 选项卡 |  sme 透视表 |
| .radio |  sme 窗体字段 [类型 ="单选"] |
| .required 线索 | sme 窗体字段 |
| .searchbox |  sme 窗体字段 [类型 ="搜索"] |
| .toggle 开关 |  sme 窗体字段 [类型 ="切换开关"] |
| .tool 容器 |  sme 布局的内容区域或 sme 布局的内容的区域的填充 |

### 这些 CSS 类已弃用，并且不再受支持：

| 旧类 | 已弃用 |
| -- | -- |
| .acceptable | （已弃用） |
| .color 错误 | （已弃用） |
| .color 信息 | （已弃用） |
| .color 成功 | （已弃用） |
| .color 警告 | （已弃用） |
| .delete 按钮 | （已弃用） |
| .details 内容 | （已弃用） |
| .error 涵盖 | （已弃用） |
| .error 消息 | （已弃用） |
| .guided 窗格按钮 | （已弃用） |
| .header 容器 | （已弃用） |
| .icon win | （已弃用） |
| .indent | （已弃用） |
| .invalid | （已弃用） |
| .item 列表 | （已弃用） |
| .modal 可滚动 | （已弃用） |
| 。 多部分 | （已弃用） |
| 无操作栏 | （已弃用） |
| .overflow 边距 | （已弃用） |
| .overflow 工具 | （已弃用） |
| .progress 涵盖 | （已弃用） |
| .right 面板 | （已弃用） |
| .rollup | （已弃用） |
| .rollup 状态 | （已弃用） |
| .rollup 标题 | （已弃用） |
| .rollup 值 | （已弃用） |
| .searchbox 操作栏 | （已弃用） |
| .size-h-1 | （已弃用） |
| .size-h-2 | （已弃用） |
| .size-h-3 | （已弃用） |
| .size-h-4 | （已弃用） |
| .size h 完全 | （已弃用） |
| .size h 半 | （已弃用） |
| .size-v-1 | （已弃用） |
| .size-v-2 | （已弃用） |
| .size-v-3 | （已弃用） |
| .size-v-4 | （已弃用） |
| .status 图标 | （已弃用） |
| .svg 16px | （已弃用） |
| .table 缩进 | （已弃用） |
| .table sm | （已弃用） |
| .thin | （已弃用） |
| .tile | （已弃用） |
| .tile 正文 | （已弃用） |
| .tile 内容 | （已弃用） |
| .tile 页脚 | （已弃用） |
| .tile 标头 | （已弃用） |
| .tile 布局 | （已弃用） |
| .tile 表 | （已弃用） |
| .toolbar | （已弃用） |
| .tool 栏 | （已弃用） |
| .tool 标头 | （已弃用） |
| .tool 标头框 | （已弃用） |
| .tool 窗格 | （已弃用） |
| .usage 栏 | （已弃用） |
| .usage 栏区域 | （已弃用） |
| .usage 条背景 | （已弃用） |
| .usage 栏标题 | （已弃用） |
| .usage 栏值 | （已弃用） |
| .usage 图 | （已弃用） |
| .usage 消息 | （已弃用） |
| .usage 消息区域 | （已弃用） |
| .usage 消息标题 | （已弃用） |
| .warning | （已弃用） |
| .white 空间 | （已弃用） |

## 7.了解和解决问题的可观测对象

### 更新```rxjs```函数用于可观察到的对象

这些是一些常见的函数名称已发生更改，可能有其他人在项目中。

* 更新```Observable.empty()```到 ```empty()```
* 更新```Observable.of()```到 ```of()```
* 更新```.switchMap()```到 ```.pipe(switchMap())```
* 更新```.map()```到 ```.pipe(map())```
* 更新```flatMap()```到 ```mergeMap()```


### 解决运行时的问题```.map()```和```.filter()```可观测对象上的功能

如果编译器不能正确识别```observable```对象的类型```.map()```和```.filter()```函数从```array```对象可能会改为映射到你的对象，这会导致在运行时错误。  请确保你函数返回```observable```指定要避免此问题显式数据类型的对象。

```any``` 此外，并且没有返回类型可以导致此问题，查找与这些模式的代码：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8.解决其他常见的问题

这些技术有助于解决其他常见的问题：

* 运行```ng lint --fix```若要解决棉绒常见问题
* 运行```gulp build```反复以增量方式修复问题的```gulp build```可以自动解决

## 9.生成并提供你的项目

运行以下命令，以生成和使用最新版本 (SDK 1.0) 为你的项目提供服务：

``` cmd
gulp build
gulp serve --port 4201
```

## 10.打开 Windows Admin Center 中的深色主题

若要打开 Windows Admin Center 版本中的深色主题，1902年及更高版本，请按照下列步骤：

* 打开 Windows Admin Center （1902 及更高版本）
* 单击窗口右上角中的**设置**图标
* 选择**高级**选项卡
* 在*实验键*，单击**添加**
* 输入新值```msft.sme.shell.personalization```由在上一步创建的空字段
* 单击**保存并重新加载**
* 设置都将具有一个新选项卡，**个性化设置**。  选择此选项卡
* 将**颜色**更改为**深色模式 （预览版）**
