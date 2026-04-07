# 机器翻译 (Machine Translation)<no value>

# 机器翻译使用说明

机器翻译：标注员先阅读中文源文本，再在右侧输入框中填写西班牙语译文。该模板适合通用文本翻译、行业语料构建、术语库校对等场景，常用于多语种翻译模型训练与质量评估。

## 标注核心作用

1.  构建高质量平行语料：源文与译文一一对应，可直接用于训练与评测；
2.  保持术语一致性：通过统一译法降低模型输出波动；
3.  提升人工可控性：支持人工逐条翻译与复核，便于质量管控。

## 基础操作步骤

1.  阅读左侧中文段落，理解整体语义和句间逻辑；
2.  在右侧输入框填写西班牙语译文，注意检查是否漏译、误译或语法不通顺；
3.  点击添加按钮提交译文；
4.  提交后可以点击编辑键进行编辑，或者点击删除键以删除译文。

![机器翻译标注示例](./images/machine-translation-example.png)

说明：建议优先保证“忠实原意 + 语句通顺”；若项目有术语表，请严格按术语表翻译。

## 注意事项

- `required="true"` 表示译文必填，空内容无法提交；
- `maxSubmissions="1"` 限制每条样本只提交一版译文，避免重复记录；
- 涉及专有名词时，优先使用项目约定译法并保持全量数据一致。

## 模板预览

![机器翻译模板预览](./images/machine-translation.png)

## 模板配置
### 完整代码块

```html
<View>
  <View style="display: grid; grid-template: auto/1fr 1fr; column-gap: 1em">
    <Header value="请阅读以下中文：" />
    <Header value="请翻译为西班牙语：" />

    <Text name="chinese" value="$chinese" />

    <TextArea name="spanish" toName="chinese" showSubmitButton="true"
              maxSubmissions="1" editable="true"
              required="true"/>
  </View>
</View>
```

### 机器翻译配置代码说明

以上代码用于实现“源语言阅读 + 目标语言录入”的双栏翻译流程。

1、布局：内层 `View` 使用 `grid` 将界面拆成左右两栏，左侧源文、右侧译文，阅读与输入更直观。

2、源文本：`Text name="chinese" value="$chinese"` 用于展示中文原文。

3、译文输入：`TextArea name="spanish" toName="chinese"` 绑定源文本；`showSubmitButton="true"` 显示提交按钮；`editable="true"` 允许编辑；`required="true"` 要求必填。

### 示例数据（简要）

```json
{
  "data": {
    "chinese": "科技的发展让人类探索宇宙的脚步不断向前。从望远镜观测星空到探测器登陆火星，从卫星环绕地球到空间站驻留太空，每一次技术突破都在拓宽我们的认知边界。宇宙的浩瀚与神秘吸引着无数人追寻，人类以科技为舟，在星辰大海中前行，既追寻宇宙的本源答案，也书写着属于自身的探索史诗。"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 可将 `chinese/spanish` 替换为任意语种字段名（如 `en/fr`），并保持 `toName` 绑定关系正确；
- 若需多人复译，可通过任务分配策略实现多轮质检，不建议直接放开单条多次提交。
