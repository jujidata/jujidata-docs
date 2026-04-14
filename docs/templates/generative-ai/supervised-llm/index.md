# 监督式语言模型微调 (Supervised LLM Fine-tuning)<no value>

# 监督式语言模型微调使用说明

可以理解为「根据上面指令或题目，下面在文本框里写出模型应当学会模仿的标准答案」。根据提示提示，标注员在答案区补全完整可运行代码或者文本。它适合指令微调、代码生成、长文作答等需要**成对 (prompt, response)** 数据的任务。

## 标注核心作用

1.  将提示与参考答案约束在同屏，减少跨字段对齐错误；
2.  `required` 与 `requiredMessage` 可降低空交率；
3.  通过 `Style` 与 `className` 区分提示区与作答区，提升长文本阅读体验。

## 基础操作步骤

1.  阅读蓝色提示区中的 `prompt` 全文，明确任务类型与输出格式要求；
2.  在下方答案区撰写或粘贴完整回复（如代码需包含必要函数体与返回值）；
3.  点击「添加」即可提交。

![监督式语言模型微调标注示例](./images/supervised-llm-example.png)

说明：提交之后，可点击右侧删除按钮，删除此答案并重新输入。

## 注意事项

- `TextArea` 的 `toName="prompt"` 将答案与提示对象关联，导出结构以平台为准；
- `maxSubmissions="1"` 通常表示每条任务仅保留一条最终答案，多稿场景需调整配置；
- 样式表中的 `.answer-input` 作用于外层容器，内置富文本/输入控件的实际背景色可能与截图不完全一致；
- 提示与答案中的引号、换行在导入 JSON 时需正确转义。

## 模板预览

![监督式语言模型微调模板预览](./images/supervised-llm.png)

## 模板配置
### 完整代码块

```html
<View className="root">
    <View className="container">
      <View className="prompt">
        <Text name="prompt" value="$prompt" />
      </View>
      <View className="answer-input">
        <TextArea name="instruction" toName="prompt"
                  maxSubmissions="1" required="true"
                  requiredMessage="您必须提供对提示的响应"
                  placeholder="在此输入您的答案..."
                  rows="10"
        />
      </View>
    </View>
</View>
```

### 配置代码说明

以上代码通过外层 `root` 注入全局字体与背景，内层 `container` 承载白底卡片；`prompt` 区域只读展示 `$prompt`；`answer-input` 包裹 `TextArea` 收集标注结果。

1、样式：`Style` 内定义 `.root`、`.container`、`.prompt`、`.answer-input` 等类，用于区分提示条与作答区；可按品牌色修改 `#0084ff` 等取值。

2、提示：`Text name="prompt" value="$prompt"` 从任务数据读取提示文本，一般不在此区域编辑。

3、答案：`TextArea name="instruction" toName="prompt"` 绑定到同名提示对象；`required="true"` 与 `requiredMessage` 用于必填校验；`placeholder`、`rows` 控制占位与初始高度。

说明
- 代码可直接复制到标注配置文件中使用；
- 若字段改名为 `instruction` 等，请同步修改 `value="$prompt"` 与文档说明；
- 复杂 HTML 或代码块中的特殊字符在导入时需按 JSON 规范转义。