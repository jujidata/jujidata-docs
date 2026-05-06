# 声音事件监测 (Sound Event Detection)<no value>

# 声音事件监测使用说明

可以理解为「在一条音频里，把不同事件在时间线上标出来」。与 [意图分类](./intent-classification/)（区段 + 语言意图）不同，本模版**只标声学与时间边界**，不依赖文本；与 [使用片段的自动语音识别](./automatic-speech-recognition-using-segments/) 相比，这里区段表示**不同事件**而不是「语音/噪音 + 转写」。

## 标注核心作用

1.  `Labels` 提供至少两类可区分颜色（如红 / 绿），在波形上形成**可重叠、可多段**的区段（以平台能力为准）；
2.  `Audio` 承担播放与**时间轴**展示，便于精调区段起止；
3.  配置极简单，适合作为 **SED / 音频强监督** 的起点。

## 基础操作步骤

1.  在顶部选择当前要标的事件类型；
2.  在波形上拖选起止时间；可切换标签标注另一类事件，重复多段；
3.  自检区段与听感一致后提交。

![声音事件监测标注示例](./images/sound-event-detection-example.png)

说明：截图中①示意当前选中的事件标签；②示意可调整的区段边界或区段内范围。

## 注意事项

- `data.audio` 须可访问；`Labels` 上的 `zoom`、`hotkey` 是否生效以当前平台对 `Labels` 属性的支持为准，若与预期不符可将 `zoom` 改挂到 `Audio`（`zoom="true"`）并查文档；
- 仅两个事件为示例，可按场景增加 `Label`（多类 SED）；
- 若需**区段内文字**或**每段情感**，需叠加 `TextArea`、`Choices` 等组件。

## 模板预览

![声音事件监测模板预览](./images/sound-event-detection.png)

## 模板配置
### 完整代码块

```html
<View>
  <Labels name="label" toName="audio" zoom="true" hotkey="ctrl+enter">
    <Label value="事件一" background="red"/>
    <Label value="事件二" background="green"/>
  </Labels>
  <Audio name="audio" value="$audio"/>
</View>
```

### 配置代码说明

以上代码为「事件类型标签 + 音频」。

1、标签：`Labels name="label" toName="audio"` 声明在音频时间轴上可创建的区域类型；`background` 为区段与按钮配色。

2、音频：`Audio name="audio" value="$audio"` 从任务数据的 **`audio` 字段**加载。

### 示例数据（简要）

```json
{
  "data": {
    "audio": "/static/templates/project-samples/sound-event-detection.mp3"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- 请将示例路径替换为实际上传或可访问的音频地址。
