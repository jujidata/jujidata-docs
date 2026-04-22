# 对话分析 (Conversational Analysis)<no value>

# 对话分析使用说明

可以理解为「先听整条对话录音，再对**每一句（或每一段话轮）选情感标签**」。与 [按片段的 ASR](./automatic-speech-recognition-using-segments/) 不同，话轮文本通常已由上游 ASR 写好，本任务侧重**对齐音频与话轮级标签**。

## 标注核心作用

1.  `Audio` 使用 `sync="text"`，与 `Paragraphs` 的 `sync="audio"` 配对，实现**播放进度与话轮列表**联动；
2.  `Paragraphs` 以 `layout="dialogue"` 展示气泡，`textKey` / `nameKey` 映射正文与说话人，`granularity="paragraph"` 以段为操作单元；
3.  `ParagraphLabels` 将 **积极 / 消极** 等标签绑定到 `name="text"` 的话轮对象，用于整段标注。

## 基础操作步骤

1.  用顶部波形与播放键听全段或逐段播放（单条气泡旁也可能有播放入口）；
2.  在「对话转录」中浏览说话人、时间与正文，可用「显示所有作者」与「自动滚动」辅助阅读；
3.  在「情感标签」中选择 **积极** 或 **消极**；
4.  直接在文字部分选区标注，或者点击气泡后面标识一键标注整句话。

![对话分析标注示例](./images/conversational-analysis-example.png)

说明：截图中①示意播放控制；②示意情感标签（如「积极」）；③示意已标注的话轮正文或选区（界面标号以实际为准）。

## 注意事项

- `data.text` 为**对象数组**，每条须包含 `author`、`text`、`start`、`end`（秒）等字段，并与 `data.audio` 时长大致一致；
- `audioUrl="$audio"` 与顶层 `Audio value="$audio"` 使用同一字段，路径须可访问；
- `contextscroll="true"` 有助于长对话随播放滚动；`position: sticky` 的底部标签栏便于始终看到情感选项；
- 若需更多情感类别或维度，请在 `ParagraphLabels` 中追加 `Label` 并更新培训材料。

## 模板预览

![对话分析模板预览](./images/conversational-analysis.png)

## 模板配置
### 完整代码块

```html
<View>
   <Audio name="audio" value="$audio"
          hotkey="space" sync="text"/>
   <Header value="对话转录"/>
   <Paragraphs audioUrl="$audio"
               sync="audio"
               name="text"
               value="$text"
               layout="dialogue"
               textKey="text"
               nameKey="author"
               granularity="paragraph"
               contextscroll="true" />
    <View style="position: sticky">
     <Header value="情感标签"/>
     <ParagraphLabels name="label" toName="text">
       <Label value="积极" background="#00ff00"/>
       <Label value="消极" background="#ff0000"/>
     </ParagraphLabels>
   </View>
</View>
```

### 配置代码说明

以上代码为「同步音频 + 对话式段落列表 + 话轮级标签」。

1、音频：`Audio` 的 `sync="text"` 与 `Paragraphs` 的 `sync="audio"` 成对使用，实现音轨与段落联动；`hotkey="space"` 为播放快捷键提示。

2、段落：`Paragraphs name="text"` 从 `$text` 加载 JSON 数组；`layout="dialogue"` 为气泡布局；`textKey`、`nameKey` 指定正文与说话人字段。

3、标签：`ParagraphLabels name="label" toName="text"` 将情感标签关联到段落对象；外层 `View` 使用 `position: sticky` 使标签区在滚动时保持可见（具体以浏览器与平台样式为准）。

### 示例数据（简要）

```json
{
  "data": {
    "text": [
      {
        "end": 4.3,
        "text": "主管，这个项目的投资回报测算我做完了。",
        "start": 0.1,
        "author": "赵钱"
      },
      {
        "end": 8.8,
        "text": "风险部分再核对一下，竞品数据要最新的。",
        "start": 4.3,
        "author": "王主管"
      },
      {
        "end": 12,
        "text": "好，我马上补充行业波动情况。",
        "start": 8.8,
        "author": "赵钱"
      },
      {
        "end": 16,
        "text": "嗯，下午三点前发我，我们一起过一遍。",
        "start": 12,
        "author": "王主管"
      },
      {
        "end": 19.3,
        "text": "明白，我尽快整理好准时提交。",
        "start": 16,
        "author": "赵钱"
      }
    ],
    "audio": "/static/templates/project-samples/conversation.mp3"
  }
}
```
数组里**每一条话轮**常用四个字段（与 `nameKey` / `textKey` 及时间对齐有关）：
- **`author`**：说话人显示名，对应配置里的 `nameKey="author"`；
- **`text`**：该段台词正文，对应 `textKey="text"`；
- **`start`**：该段在整段音频中的**起始时间**（秒），用于与波形、单段试听对齐；
- **`end`**：该段**结束时间**（秒）；与 `start` 共同界定本句在音轨上的范围，宜与实际分段一致，便于试听与质检。

说明

- 代码可直接复制到标注配置文件中使用；
- `start` / `end` 建议与音频实际分段一致，便于试听与质检。
