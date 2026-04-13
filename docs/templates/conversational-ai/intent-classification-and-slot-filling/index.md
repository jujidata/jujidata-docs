# 意图分类和槽填充 (Intent Classification and Slot Filling)<no value>

# 意图分类和槽填充使用说明

可以理解为「上面用槽位类型在对话里圈出实体片段，下面为整段对话选一个意图标签」。例如点餐对话中，在用户话轮里标出「东方面馆」「东大门街道」「一份」等跨度，并在底部勾选「客户请求」。它适合任务型对话、智能客服、语音助手等需要同时产出**对话意图**与**槽位监督**的训练数据。

## 标注核心作用

1.  同屏完成**整段对话意图**与**轮内槽位跨度**，减少拆任务与对齐成本；
2.  `ParagraphLabels` 与 `Paragraphs` 绑定，槽位标注直接落在对话原文上；
3.  `Choices` 单选约束清晰，便于统计各类意图分布。

## 基础操作步骤

1.  自上而下阅读人机多轮对话，把握用户目标与关键信息；
2.  点选槽位标签，在对应话轮中标注对应文本片段；
3.  在底部「意图」区域点选一项，确认与整段对话语义一致；
4.  复核槽位边界与意图类别后提交。

![意图分类和槽填充标注示例](./images/intent-classification-and-slot-filling-example.png)

说明：截图中①—③示意槽位选择与意图勾选；快捷键序号以平台实际显示为准。

## 注意事项

- 槽位边界尽量只包含有效取值，避免带上「示例：」等提示性前缀（若项目约定保留则统一口径）；
- `choice="single"` 表示意图互斥，若业务需要多意图需调整配置与规范；
- `humanMachineDialogue` 须为**对象数组**，每条含默认的 `author` 与 `text`（或与 `nameKey` / `textKey` 一致），否则段落组件无法渲染；
- 轮次作者取值需与 `layout="dialogue"` 的展示约定一致（如人类 / 机器人），导入数据前请与工程侧对齐。

## 模板预览

![意图分类和槽填充模板预览](./images/intent-classification-and-slot-filling.png)

## 模板配置
### 完整代码块

```html
<View>
  <ParagraphLabels name="entity_slot" toName="dialogue">
    <Label value="人物" />
    <Label value="组织" />
    <Label value="地点" />
    <Label value="日期时间" />
    <Label value="数量" />
  </ParagraphLabels>
  <Paragraphs name="dialogue" value="$humanMachineDialogue" layout="dialogue" />
  <Choices name="intent" toName="dialogue"
       choice="single" showInline="true">
    <Choice value="问候"/>
    <Choice value="客户请求"/>
    <Choice value="闲聊"/>
  </Choices>
</View>
```

### 配置代码说明

以上代码用于「多轮对话展示 + 槽位跨度标注 + 对话级意图单选」。

1、槽位标签：`ParagraphLabels name="entity_slot" toName="dialogue"` 将人物、组织、地点等槽类型绑定到名为 `dialogue` 的段落对象；先选类型再在话轮内划选。

2、对话正文：`Paragraphs name="dialogue" value="$humanMachineDialogue" layout="dialogue"` 从任务数据读取对话数组并以人机对话版式展示；字段名需与导入 JSON 一致。

3、意图分类：`Choices name="intent" toName="dialogue"` 将意图选项关联到同一段对话对象；`choice="single"` 为单选；`showInline="true"` 为横向排列选项（若你所在环境使用其他拼写或属性名，请与平台文档保持一致）。

### 示例数据（简要）

以下结构与截图中的示例话轮一致；`author` 与 `text` 为默认键名，可按需在 `Paragraphs` 上配置 `nameKey`、`textKey`。

```json
{
  "data": {
    "humanMachineDialogue": [
      { "author": "人类", "text": "示例：嗨，机器人！" },
      { "author": "机器人", "text": "示例：很高兴见到你，人类！告诉我你想要什么。" },
      { "author": "人类", "text": "示例：给我从东方面馆点一份麻辣烫，送到东大门街道" },
      { "author": "机器人", "text": "示例：好的。你希望什么时候收到订单？" },
      { "author": "人类", "text": "示例：凌晨3点。" }
    ]
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 增删 `Label` 或 `Choice` 时，请同步更新数据字段、质检规则与标注培训材料；
- 若对话来自 ASR，可在对象中补充时间戳等字段（以平台支持的 `Paragraphs` 字段为准）。