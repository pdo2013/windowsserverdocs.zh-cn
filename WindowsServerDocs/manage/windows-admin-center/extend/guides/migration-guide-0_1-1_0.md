---
title: 从 Windows Admin Center SDK 0.1 到 1.0 迁移
description: 本指南将帮助你从 Windows Admin Center SDK 0.1 到 1.0 版迁移
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886468"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>从 Windows Admin Center SDK 0.1 到 1.0 迁移

>适用于：Windows Admin Center 预览版

本指南将帮助你从 Windows Admin Center SDK 版本 0.1 到 1.0 迁移。  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1.了解开发人员指南扩展名的新控件

Windows Admin Center 1902年和更高版本包括**开发人员指南**扩展，可用于查找控件 （包括新的可用控件） 和方案，以帮助您构建自己的扩展的示例。  适用于开发人员指南将替换**开发人员工具**扩展名从早期版本的 SDK。

### <a name="use-the-dev-guide-in-windows-admin-center"></a>在 Windows Admin Center 中使用适用于开发人员指南

适用于开发人员指南是可用作 Windows Admin Center 版本 1902年中的解决方案和更高版本。  适用于开发人员指南已预装，但它需要通过设置来启用。

**启用 Windows Admin Center 中的开发人员指南：**

* 打开 Windows Admin Center （1902 和更高版本）
* 单击**设置**中窗口右上角的图标
* 选择**高级**选项卡
* 下*实验密钥*，单击**添加**
* 输入新值```msft.sme.shell.devguide```由上一步创建的空字段中
* 单击**保存和重新加载**

**在 Windows Admin Center 中打开开发人员指南：**

* 打开 Windows Admin Center （1902 和更高版本）
* 单击左上角显示所有解决方案类型中的下拉列表
* 选择**开发人员指南**解决方案 
    * 如果没有看到列出的解决方案，请确保已启用适用于开发人员指南 （请参阅上面部分），并且必须重新加载 Windows Admin Center。
* 通过选择其中一个选项卡浏览开发人员指南的内容
    * **登录：** 包含的代码示例*管理方式*并*通知*方案
    * **控件：** 包含每个 SDK 中的可用控件的示例
    * **管道：** 包含可用的转换器和格式化程序函数的示例
    * **样式：** 包含在 SDK 中提供的 CSS 样式的示例
    * **MsftSme:** 包含示例和对高级方案的指导 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>浏览 GitHub 上的开发人员指南的源代码

您可以浏览[源代码](https://github.com/Microsoft/windows-admin-center-sdk/)以找到 HTML、 CSS 和 TypeScript 的代码示例的示例的 GitHub 上的开发人员指南。

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2.准备开发环境以便使用最新的 SDK

安装或更新 node.js 版本[10.15.1 LTS 或更高版本](https://nodejs.org/en/)。

更新到最新版本的 Windows Admin Center CLI:

[//]: # "npm 卸载-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

对这些版本更新全局依赖项：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3.使用最新 SDK 创建新项目

使用 Windows Admin Center CLI 创建新的项目面向```next```版本 (SDK 1.0):

[//]: # "wac create--公司 Contoso Inc.-工具 ' 管理 Foo Works'-实验性版本"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下来，将目录更改为刚创建的文件夹，然后通过运行安装所需的本地依赖项```npm install ```。

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4.修改现有项目，以使用最新的 SDK

重要：在继续之前请备份您的项目。

修改以下行中的```package.json```到目标```next```版本 (SDK 1.0):

[//]: # "@microsoft/windows-admin-center-sdk: 实验"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然后运行```npm install```更新整个项目的引用。

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5.使用 SDK CLI 来修复迁移的常见问题

重要：在继续之前请备份您的项目。

从你的项目的根文件夹，以便自动修复常见的迁移问题在项目上运行以下 CLI 命令：

``` cmd
wac updateSeven --update
```

此 CLI 命令自动解决以下问题：

* 重新生成 ```package-lock.json```
* 更新在 angular 编译环境中的文件：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6.使用 SDK CLI 来了解常见的迁移问题

从你的项目的根文件夹，运行以下的 CLI 命令，以审核你的项目并查找常见需要手动解决的迁移问题：

``` cmd
wac updateSeven --audit
```

这将在项目中查找以下问题的实例：

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>将这些 sme 类替换为以下 CSS 类的用法：

| 旧的 CSS 类 | 新的 CSS 类 |
| -- | -- |
| .auto-flex-size |  .sme-position-flex-auto |
| .border-all |  .sme 边框嵌入 sm 和.sme-边框的颜色-基本-90 |
| .border-bottom |  .sme 边框底部 sm 和.sme-border-bottom-color-base-90 |
| .border-horizontal |  .sme 边框水平 sm 和.sme-border-horizontal-color-base-90 |
| .border-left |  .sme 边框左侧 sm 和.sme-border-left-color-base-90 |
| .border-right |  .sme 边框右 sm 和.sme-border-right-color-base-90 |
| .border-top |  .sme 边框顶部 sm 和.sme-border-top-color-base-90 |
| .border-vertical |  .sme 边框垂直 sm 和.sme-border-vertical-color-base-90 |
| .break-word |  .sme-arrange-ws-wrap |
| .btn |  .sme 按钮或按钮 |
| .btn 主 |  .sme button.sme 按钮主 OR.button.sme-按钮-主 |
| .color-dark |  .sme-color-alt |
| .color-light |  .sme-color-base |
| .color-light-gray |  .sme-color-base-90 |
| .fixed-flex-size |  .sme-position-flex-none |
| .flex-layout |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .highlight |  .sme-background-color-yellow |
| .horizontal |  .sme-arrange-stack-h |
| .no-scroll |  .sme-position-flex-auto |
| .nowrap |  .sme-arrange-stack-h OR .sme-arrange-stack-v |
| .relative |  .sme-layout-relative |
| .relative-center |  .sme-layout-absolute .sme-position-center |
| .reverse |  .sme-arrange-stack-reversed |
| .stretch-absolute |  .sme-layout-absolute .sme-position-inset-none |
| .stretch-fixed |  .sme-layout-fixed .sme-position-inset-none |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .sme-position-stretch-h |
| .vertical |  .sme-arrange-stack-v |
| .vertical-scroll-only |  .sme-排列-溢出-隐藏-x sme-排列的溢出-自动-y |
| .wrap |  .sme-arrange-wrapstack-h OR .sme-arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>将这些 sme 组件替换为以下组件的使用情况：

| 旧的组件 | 新组件 |
| -- | -- |
| .alert |  sme-alert |
| .alert-danger |  sme-alert |
| .breadCrumb |  sme-alert |
| .checkbox |  sme-form-field[type="checkbox"] |
| .combobox |  sme-form-field[type="select"] |
| .dashboard |  sme-layout-content-zone-padded sme-arrange-stack-h |
| .details-panel |  sme-property-grid |
| .details-panel-container |  sme-property-grid |
| .details-tab |  sme 属性网格或 sme 透视 |
| .details-wrapper |  sme-property-grid |
| .disabled |  sme-disabled |
| .form-buttons | sme-form-field |
| .form-control | sme-form-field |
| .form-controls | sme-form-field |
| .form-group | sme-form-field |
| .form-group-label | sme-form-field |
| .form-input | sme-form-field |
| .form-stretch | sme-form-field |
| .input-file | sme-form-field |
| .nav-tabs |  sme-pivot |
| .radio |  sme-form-field[type="radio"] |
| .required-clue | sme-form-field |
| .searchbox |  sme-form-field[type="search"] |
| .toggle-switch |  sme-form-field[type="toggle-switch"] |
| .tool-container |  sme 布局的内容区域或 sme 布局的内容-区域-填充 |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>这些 CSS 类已弃用并不再受支持：

| 旧类 | 已弃用 |
| -- | -- |
| .acceptable | （已弃用） |
| .color-error | （已弃用） |
| .color-info | （已弃用） |
| .color-success | （已弃用） |
| .color-warning | （已弃用） |
| .delete-button | （已弃用） |
| .details-content | （已弃用） |
| .error-cover | （已弃用） |
| .error 消息 | （已弃用） |
| .guided-pane-button | （已弃用） |
| .header-container | （已弃用） |
| .icon win | （已弃用） |
| .indent | （已弃用） |
| .invalid | （已弃用） |
| .item-list | （已弃用） |
| .modal-scrollable | （已弃用） |
| .multi-section | （已弃用） |
| .no-action-bar | （已弃用） |
| .overflow-margins | （已弃用） |
| .overflow-tool | （已弃用） |
| .progress-cover | （已弃用） |
| .right 面板 | （已弃用） |
| .rollup | （已弃用） |
| .rollup-status | （已弃用） |
| .rollup-title | （已弃用） |
| .rollup-value | （已弃用） |
| .searchbox-action-bar | （已弃用） |
| .size-h-1 | （已弃用） |
| .size-h-2 | （已弃用） |
| .size-h-3 | （已弃用） |
| .size-h-4 | （已弃用） |
| .size-h-full | （已弃用） |
| .size-h-half | （已弃用） |
| .size-v-1 | （已弃用） |
| .size-v-2 | （已弃用） |
| .size-v-3 | （已弃用） |
| .size-v-4 | （已弃用） |
| .status-icon | （已弃用） |
| .svg-16px | （已弃用） |
| .table-indent | （已弃用） |
| .table-sm | （已弃用） |
| .thin | （已弃用） |
| .tile | （已弃用） |
| .tile 正文 | （已弃用） |
| .tile-content | （已弃用） |
| .tile-footer | （已弃用） |
| .tile-header | （已弃用） |
| .tile-layout | （已弃用） |
| .tile-table | （已弃用） |
| .toolbar | （已弃用） |
| .tool-bar | （已弃用） |
| .tool-header | （已弃用） |
| .tool-header-box | （已弃用） |
| .tool-pane | （已弃用） |
| .usage-bar | （已弃用） |
| .usage-bar-area | （已弃用） |
| .usage-bar-background | （已弃用） |
| .usage-bar-title | （已弃用） |
| .usage-bar-value | （已弃用） |
| .usage-chart | （已弃用） |
| .usage 消息 | （已弃用） |
| .usage-message-area | （已弃用） |
| .usage-message-title | （已弃用） |
| .warning | （已弃用） |
| .white 空间 | （已弃用） |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7.了解和解决问题的可观测对象

### <a name="update--rxjs-function-use-for-observable-objects"></a>更新```rxjs```函数可观测对象的使用

这些是一些常见的函数名称已更改，可能有其他人在你的项目中。

* 更新```Observable.empty()```到 ```empty()```
* 更新```Observable.of()```到 ```of()```
* 更新```.switchMap()```到 ```.pipe(switchMap())```
* 更新```.map()```到 ```.pipe(map())```
* 更新```flatMap()```到 ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>解决运行时的问题```.map()```和```.filter()```可观测对象的函数

如果编译器不能正确识别```observable```对象的类型```.map()```并```.filter()```函数从```array```对象可能会改为映射到您的对象，从而导致在运行时错误。  请确保你的函数返回```observable```对象，它指定显式数据类型为避免此问题。

```any``` 并且没有返回类型可以导致此问题，查找与这些模式的代码：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8.解决其他常见的问题

这些技术将帮助解决其他常见的问题：

* 运行```ng lint --fix```以解决 lint 常见问题
* 运行```gulp build```反复要以增量方式解决问题的```gulp build```可以自动解决

## <a name="9-build-and-serve-your-project"></a>9.构建并提供你的项目

运行以下命令，以构建并提供你的项目 (SDK 1.0) 的最新版本：

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10.开启 Windows Admin Center 中的深色主题

若要打开 Windows Admin Center 版本中的深色主题 1902年及更高版本，请按照下列步骤：

* 打开 Windows Admin Center （1902 和更高版本）
* 单击**设置**中窗口右上角的图标
* 选择**高级**选项卡
* 下*实验密钥*，单击**添加**
* 输入新值```msft.sme.shell.personalization```由上一步创建的空字段中
* 单击**保存和重新加载**
* 设置现在具有新的选项卡**个性化设置**。  选择此选项卡
* 更改**颜色**到**深色模式 （预览版）**
