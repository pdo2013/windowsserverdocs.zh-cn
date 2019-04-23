# <a name="metadata-and-version-identifiers"></a>元数据和版本标识符

下面是您需要了解的有关商标、 版本和 windowsserverdocs pr 存储库中的文章的元数据。 文章作者应负责确保其项目符合这些标准和要求。

## <a name="trademarks"></a>商标
技术库中的文章中的产品引用之后，不使用商标符号。 因为 technet.microsoft.com、 docs.microsoft.com 和其他发布通道的官方 Microsoft 包括不需要这些[商标](https://www.microsoft.com/trademarks)页脚链接到一系列 Microsoft 商标。 该链接可满足法律要求。 有关背景信息，请参阅指导[CELA](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage)，在"网站"和 Microsoft 写作风格指南下面[版权和商标](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696)下"Microsoft.com 网页"页 

## <a name="versioning"></a>版本控制
几种类型的版本控制适用于此存储库中的文章： 

-  通过两种方式进行版本控制，该值指示一篇文章适用于的产品版本：
    - 手动上是显示为已发布的项目上的文本的文章中的单个行。
    - 由元数据，如下所述。
-  通过 Github 的文件的历史记录来处理源内容版本控制。 

## <a name="metadata-structure-and-format"></a>元数据结构和格式

- 将元数据放在上面的 H1 标题的文件的顶部。
- 通过在块的第一个和最后一个行中使用只有三个短划线将文件内容的其余部分分开块。 不要将任何其他文本放在这些行上。
- 每个元数据名称/值对放入单独的行。
- 使用以下语法模式，具体取决于元数据是否需要预定义的值或接受自定义值之一。 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>元数据名称和值

虽然建议但不是必需的其他元数据中 TechNet 技术库中，作为项目发布的所有文件需要特定的元数据。 在某些情况下，仅限特定值允许特定的元数据。 

|名称|ReplTest1|
|---|---|
|title|匹配的 H1 标题的自定义值。 确定在浏览器选项卡中显示的内容。|
|description|在搜索结果中显示，并且会影响 SEO。 包括的相应关键字，但将保留在不超过 160 个字符长度。|
|作者|主要作者的文章的 Github 别名|
|ms.author|文章的作者或内容开发人员负责的文章中介绍的技术区域的 MSFT 别名。|
|ms.date|最后一次的内容更新的日期。 未更新对仅元数据的更改。 使用格式 mm/dd/yyyy。|
|ms.prod|BI 报表，根据预定义标识的 Windows Server 版本[值](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)。|
|ms.technology|BI 报表，根据预定义标识技术领域[值](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|有关 BI 报告跨所有区域设置中标识的项目的 GUID。 从早期创作文章迁移系统已经有此。 对于在 Github 中创建的项目使用工具，如[ https://guidgenerator.com/ ](https://guidgenerator.com/)。| 

## <a name="troubleshooting"></a>疑难解答

源文件出现格式错误时发生的已发布项目的顶部显示的元数据。 一些常见错误是：

- 缺少在第一个和最后一个多行块或连字符的数目不正确的三个连字符。
- 元数据不会按照所需的语法：\<名称\>:\<单一空间\>\<值 >

这些资源和任何其他明显的错误，检查你的文件并提交 PR 以将其发布更新的文件。 如果你遇到，电子邮件的拉取请求审阅者别名： wssc-pra@microsoft.com

## <a name="see-also"></a>请参阅
使用此存储库中的元数据基于内容的服务和国际中使用元数据\(CSI\)。 详细信息，包括可选的元数据，位于[ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta)。
商业智能资源，请参阅 CSI BI team [wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx)。