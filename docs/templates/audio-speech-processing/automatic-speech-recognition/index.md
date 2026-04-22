# 自动语音识别 (Automatic Speech Recognition)<no value>

# 自动语音识别使用说明

可以理解为「识别音频的内容，并写成文字」。例如方言采集、客服录音质检或 ASR 模型评测场景中，人工听写作为参考文本，便于计算字错率或与模型输出对比。

## 标注核心作用

1.  `Audio` 提供波形、播放与 `zoom="true"` 缩放，便于定位含糊片段；
2.  `hotkey="ctrl+enter"` 可在部分环境中用于快捷提交（以平台实际支持为准）；
3.  `TextArea` 通过 `toName="audio"` 与音频对象绑定，导出时与同一任务对齐。

## 基础操作步骤

1.  使用播放、暂停与波形缩放反复听辨；
2.  在文本框中输入此音频的完整转写；
3.  点击「添加」等，即可保存本条转写；
4.  自检是否与音频一致后提交任务。

![自动语音识别标注示例](./images/automatic-speech-recognition-example.png)

说明：此模板是对一条音频的完整识别和转写。

## 注意事项

- `data.audio_path` 须为可访问的音频 URL 或静态资源路径；格式需为浏览器可解码的常见格式（如 WAV、MP3）；
- `maxSubmissions="1"` 表示该文本框通常只允许一条提交记录，多版本修订规则以平台为准；
- 若需**分段转写**或**说话人分离**，需扩展为多条 `TextArea`、时间轴标签或其它组件；
- `Header` 文案可按任务改为「提供翻译」等，与数据字段名 `audio_path` 无关。

## 模板预览

![自动语音识别模板预览](./images/automatic-speech-recognition.png)

## 模板配置
### 完整代码块

```html
<View>
  <Audio name="audio" value="$audio_path" zoom="true" hotkey="ctrl+enter" />
  <Header value="请把这段音频转写成文字" />
  <TextArea name="transcription" toName="audio"
            rows="4" editable="true" maxSubmissions="1" />
</View>
```

### 配置代码说明

以上代码为「音频播放器 + 说明 + 转写文本框」。

1、音频：`Audio name="audio" value="$audio_path"` 从任务数据的 **`audio_path` 字段**加载；`zoom="true"` 开启波形缩放；`hotkey` 为快捷键提示（是否生效依环境而定）。

2、提示：`Header` 可改写为项目专用说明。

3、转写：`TextArea name="transcription" toName="audio"` 将文本与 `audio` 对象关联；`rows="4"` 控制初始高度；`editable="true"` 允许编辑；`maxSubmissions="1"` 限制提交次数。

### 示例数据（简要）

```json
{
  "data": {
    "audio_path": "/static/templates/project-templates-config/audio-speech-processing/automatic-speech-recognition/china_fun_standard.wav"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- 请将示例路径替换为实际上传或可访问的音频地址，并保持字段名与 `value="$audio_path"` 一致。
