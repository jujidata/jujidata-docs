# HTML 命名实体标注(HTML NER Tagging)<no value>

# HTML 命名实体标注使用说明

可以理解为「把一条样本里的正文（常为 HTML）渲染出来，再标注实体」。例如多轮对话场景中，将「王五先生」等称谓标为「人」，便于构建**社区贡献的 NER** 数据集或与标准 NLP 模版对照实验。

## 标注核心作用

1.  `HyperText` 将 `$text` 渲染为可交互文本层，支持在可见内容上划选；
2.  `HyperTextLabels` 仅两类标签，快捷键通常为 **1 / 2**，操作路径短；
3.  与 `structured-data-parsing` 下「HTML 实体识别」等模版同属 HyperText 系，可迁移标签体系或数据字段。

## 基础操作步骤

1.  阅读任务说明，明确不同的实体标签的边界；
2.  在顶部选择 **人** 或 **组织**等不同标签；
3.  在正文区拖选对应片段；若存在预标注，仅做核对与修正；
4.  自检漏标、错标后提交。

![HTML 命名实体标注示例](./images/html-ner-tagging-person-and-organization-example.png)


## 注意事项

- `data.text` 须为合法 HTML 或平台支持的富文本字符串；JSON 中需正确转义引号与换行；
- 标签仅「人」「组织」两类，若需 LOCATION 等类型请扩展 `Label` 列表；
- 外层 `View` 的边框与圆角仅影响排版，可按项目统一视觉规范调整；

## 模板预览

![HTML 命名实体标注模板预览](./images/html-ner-tagging-person-and-organization.png)

## 模板配置
### 完整代码块

```html
<View>
  <HyperTextLabels name="ner" toName="text">
    <Label value="人" background="green"/>
    <Label value="组织" background="blue"/>
  </HyperTextLabels>

  <View style="border: 1px solid #CCC;
               border-radius: 10px;
               padding: 5px">
    <HyperText name="text" value="$text"/>
  </View>
</View>
```

### 配置代码说明

以上代码为「实体标签栏 + 带边框的正文区」，正文区渲染任务数据中的 HTML。

1、标签：`HyperTextLabels name="ner" toName="text"` 声明「人」「组织」两类；`background` 为标注高亮色。

2、绑定：`toName="text"` 与 `HyperText name="text"` 一致，表示标签作用于该文本对象。

3、正文：`HyperText name="text" value="$text"` 从任务数据的 **`text` 字段**加载内容并渲染；外层 `View` 的 `style` 控制边框、圆角与内边距。

### 示例数据（简要）

```json
{
  "data": {
    "text": "<div><p>张三：不不，王五先生，不是那样的。非常感谢您的帮助。</p><p>李四：王五先生，我很尊重您。我只是有点不喜欢别人对我发号施令，仅此而已。</p><p>王五：如果我说话直接，是因为时间紧迫……</p></div>"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- 实际语料请替换为业务 HTML，并保持字段名与 `value="$text"` 一致。
