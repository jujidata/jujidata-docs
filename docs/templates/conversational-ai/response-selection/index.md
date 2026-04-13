# 响应选择 (Response Selection)<no value>

# 响应选择使用说明

可以理解为「先看完上面的对话历史，再在下面两条（或多条）候选回复里**单选**更恰当的一条」。它适合客服机器人、语音助手、开放域对话等需要**成对或成组比较回复质量**的数据构建。

## 标注核心作用

1.  将「上下文—候选回复—人工偏好」打包为一条样本，便于训练排序或判别模型；
2.  与 `Paragraphs` 绑定后，选择结果可与整段对话在导出结构中关联；
3.  每条候选独立 `Choices` + `single-radio`，避免两条同时误选（以平台行为为准）。

## 基础操作步骤

1.  自上而下阅读人机多轮对话，确认用户意图与约束；
2.  阅读「请选择合适的回复」下方的每条候选全文；
3.  在「回复一」「回复二」旁点选单选按钮，选出整体更合适的一条；
4.  若有均不合格等情况，按项目规范处理（如跳过或选「无法判断」类扩展项）后提交。

![响应选择标注示例](./images/response-selection-example.png)

说明：截图中箭头示意最终选中的候选；快捷键 `[1]` `[2]` 以平台显示为准。

## 注意事项

- 判定标准需在项目内统一（信息完整度、安全合规、风格一致等），避免标注员各执一词；
- `humanMachineDialogue` 必须为对象数组，且 `author` / `text` 与导入约定一致；
- `responseone`、`responsetwo` 为纯展示字段，修改字段名时需同步修改 `Text` 的 `value`；
- 若需三候选及以上，可按同样 `flex` 行结构复制一行并新增 `Choices` 与数据字段。

## 模板预览

![响应选择模板预览](./images/response-selection.png)

## 模板配置
### 完整代码块

```html
<View>
  <Paragraphs name="prg" value="$humanMachineDialogue" layout="dialogue" />
  <Header value="请选择合适的回复" />
  <View style="display:flex; align-items:center; justify-content:space-between;margin:5px 0;">
    <View style="flex: 1;">
      <Text name="text1" value="$responseone" />
    </View>
    <Choices name="selection_1" toName="prg" choice="single-radio" showInline="true">
      <Choice value="回复一" />
    </Choices>
  </View>
  <View style="display:flex; align-items:center; justify-content:space-between;margin:5px 0;">
    <View style="flex: 1;">
      <Text name="text2" value="$responsetwo" />
    </View>
    <Choices name="selection_2" toName="prg" choice="single-radio" showInline="true">
      <Choice value="回复二" />
    </Choices>
  </View>
</View>
```

### 配置代码说明

以上代码用于「对话历史 + 两条候选回复 + 各带单选」的响应选择界面。

1、对话区：`Paragraphs name="prg" value="$humanMachineDialogue" layout="dialogue"` 渲染多轮内容；名称 `prg` 供下方 `Choices` 的 `toName` 引用，以便标注结果与对话对象关联。

2、候选区：每行使用外层 `View` 的 flex 布局，左侧 `Text` 展示 `$responseone` / `$responsetwo`，右侧 `Choices` 仅含一个 `Choice`，配合 `choice="single-radio"` 形成该候选是否被选中的单选项。

3、标题：`Header` 提示标注任务；可按产品文案修改。

### 示例数据（简要）

以下示例与截图中的天气对话及两条回复一致。

```json
{
  "data": {
    "humanMachineDialogue": [
      {"author": "人类[H]", "text": "你好，帮我查一下明天北京的天气。"},
      {"author": "机器人[R]", "text": "好的，请问你想看白天还是晚上的天气？"},
      {"author": "人类[H]", "text": "白天的天气，再告诉我需不需要带伞。"}
    ],
    "responseone": "好的，我帮你查询明天北京的天气。最高气温 27°C，最低气温 16°C。",
    "responsetwo": "北京明天预计晴转多云，出门建议带一件薄外套，不用带伞。"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 若平台要求互斥「二选一」为单个 `Choices` 内两个 `Choice`，可改为一条 `Choices` 承载多选项，与本版「每候选旁一个单选」的布局不同，导出结构也会变化；
- 增加候选时，请同步扩展数据字段与 `Text` / `Choices` 的 `name`，避免重名。