# 文本分类 (Text Classification)<no value>

# 文本分类使用说明

文本分类可以理解为「读一句话或一段字，选一个最贴切的标签」：界面先展示 `$text` 绑定的正文，再在下方卡片中从若干互斥选项里点选一项（如情感分析中的积极 / 消极 / 中性）。它适合评论舆情、客服意图、新闻主题等场景，常用于情感分类、主题标注与意图识别训练。

## 标注核心作用

1.  输出标准类别标签：与文本一一对应，便于直接训练分类器；
2.  单选约束清晰：`choice="single"` 避免一条样本多标签冲突（除非业务改为多选）；
3.  布局集中：正文与选项分区展示，阅读与点选路径短。

## 基础操作步骤

1.  阅读上方待分类文本，把握整体语气或主题；
2.  在下方卡片标题「选择文本情感」对应区域，点选一项，可自动提交。

![文本分类标注示例](./images/text-classification-example.png)

说明：若项目对「中性」与「无明显情感」有区分，需在标注规范中写清边界；快捷键 `[1]` `[2]` `[3]` 以平台实际显示为准。

## 注意事项

- 类别名称、数量应与 `Choices` 内 `Choice` 配置一致，修改后需同步培训标注员；
- 待分类正文建议控制在业务可读长度内，过长时可拆条或换用长文本模版；
- 外层 `View` 的 `style` 仅影响卡片样式，可按品牌规范调整阴影与圆角。

## 模板预览

![文本分类模板预览](./images/text-classification.png)

## 模板配置
### 完整代码块

```html
<View>
  <Text name="text" value="$text"/>
  <View style="box-shadow: 2px 2px 5px #999;
               padding: 20px; margin-top: 2em;
               border-radius: 5px;">
    <Header value="选择文本情感"/>
    <Choices name="sentiment" toName="text"
             choice="single" showInline="true">
      <Choice value="积极"/>
      <Choice value="消极"/>
      <Choice value="中性"/>
    </Choices>
  </View>
</View>
```

### 文本分类配置代码说明

以上代码用于实现「展示文本 + 单选分类」流程，下面对常用属性作简要说明。

1、正文：`Text name="text" value="$text"` 用于加载待分类字符串。

2、选项区：内层 `View` 使用行内样式做出卡片效果；`Header` 为操作提示标题。

3、分类组件：`Choices name="sentiment" toName="text"` 将选择与正文对象绑定；`choice="single"` 表示单选；`showInline="true"` 表示选项横向排列，便于快速点选。

### 示例数据

以下示例与界面中的影评句一致，可按业务替换 `text` 与类别文案。

```json
{
  "data": {
    "text": "这部3D电影的视觉效果令人惊叹，效果非常逼真。"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 可将 `Choice` 改为业务类别（如主题、意图），并保持 `name` / `toName` 与数据字段一致；
- 若需多标签，可将 `choice` 调整为多选模式（以平台配置项为准）并更新质检规则。
