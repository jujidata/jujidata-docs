# 信号质量监测 (Signal Quality Detection)<no value>

# 信号质量监测使用说明

可以理解为「**听完整条音频**后，用星级给此音频信号质量打分」。与 [时间序列分析](../time-series-analysis/signal-quality/) 里在曲线上**按区段**评质量不同，本模版是**整段一条音频一个分数**。

## 标注核心作用

1.  `Header` 明确 1–10 分与「信号质量」的判定提示，便于培训对齐；
2.  `Rating` 绑定到 `Audio` 对象，导出时与 `audio` 字段同一任务；
3.  布局上评分在播放器上方，符合「先看说明 → 再听 → 再打分」的操作顺序（也可按产品调整顺序）。

## 基础操作步骤

1.  阅读顶栏说明，明确 1 分与 10 分分别对应什么现象；
2.  使用波形区播放、拖动进度，必要时反复听同一片段；
3.  在星标控件上点选 1–10 分；
4.  自检与规范一致后提交。

![信号质量监测标注示例](./images/signal-quality-detection-example.png)

说明：截图中①示意播放与波形；②示意 10 分制星级）。

## 注意事项

- `data.audio` 须可访问；示例路径中的 `video-timeline-segmentation.mp3` 仅为演示文件名，**建议替换**为与任务一致的真实音频资产；
- `maxRating="10"`、`icon="star"`、`size="medium"` 可按产品规范调整；
- 若需**分段质量**或**MOS 多维度**，本单评分模版需扩展为多条 `Rating` 或区段级标注；
- 与名称相近的**时间序列「信号质量」**模版区分：本页属 **音频/语音** 栏目。

## 模板预览

![信号质量监测模板预览](./images/signal-quality-detection.png)

## 模板配置
### 完整代码块

```html
<View>
  <Header value="请根据音频信号质量进行评分（1-10 分）" />
  <View style="margin-bottom: 18px;">
    <Rating name="rating" toName="audio" maxRating="10" icon="star" size="medium" />
  </View>
  <Audio name="audio" value="$audio"/>
</View>
```

### 配置代码说明

以上代码为「说明 + 星级 + 音频」自上而下排列。

1、说明：`Header` 可按项目改写评分维度和分制说明。

2、评分：`Rating name="rating" toName="audio"` 必须与被评对象 `Audio name="audio"` 的 `name` 一致；`maxRating="10"` 为十分制；`icon`、`size` 控制外观。

3、音频：`value="$audio"` 从任务数据的 **`audio` 字段**加载。

### 示例数据（简要）

```json
{
  "data": {
    "audio": "/static/templates/project-samples/video-timeline-segmentation.mp3"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- 请将 `audio` 路径替换为实际上传或静态资源地址；命名含 `video` 仅为示例，与音频格式无必然关系。
