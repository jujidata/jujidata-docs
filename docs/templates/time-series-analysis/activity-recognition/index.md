# 活动识别 (Activity Recognition)<no value>

# 活动识别使用说明

可以理解为「在同一段时间上同时看多路曲线，并在的时间段上涂色并打上活动类型」。例如运动监测场景中，将「跑」「走」等标签对应到时间区间，便于构建**时序分割 + 分类**的监督数据集。

## 标注核心作用

1.  `TimeSeries` 从 `valueType="url"` 指向的 CSV 加载数据，多 `Channel` 纵向堆叠且**共享同一时间轴**，缩放与游标联动；
2.  `TimeSeriesLabels` 提供活动类别与颜色，通常配合快捷键（如 1–5）切换当前标签后再划段；
3.  `overviewChannels` 在底部生成缩略概览，便于在长序列上定位与检查已标片段。

## 基础操作步骤

1.  阅读任务说明，明确各标签含义及是否允许重叠、最小片段长度等规则；
2.  在顶部选择当前要打的**活动类型**；
3.  在主图中沿时间拖动选择起止时间；必要时用底部概览条平移或缩放视窗；
4.  自检片段是否与曲线语义一致后提交。

![活动识别标注示例](./images/activity-recognition-example.png)

说明：截图中①示意当前选中的标签（如「走」），②示意该标签在时间轴上对应的彩色区段；多通道曲线会同步显示同一区段。

## 注意事项

- CSV 须包含配置中的时间列与通道列；`sep`、列名、`timeFormat` 须与文件实际格式一致，否则无法解析或错位；
- `timeDisplayFormat` 仅影响轴与提示中的展示，不改变解析规则；
- `overviewChannels` 填写的通道名（如 `velocity`）用于底部缩略图，需与某条 `Channel` 的 `column` 一致；
- 大文件或极高采样率可能影响浏览器性能，宜在数据侧降采样或分片任务。

## 模板预览

![活动识别模板预览](./images/activity-recognition.png)

## 模板配置
### 完整代码块

```html
<View>
    <!-- 控制标签：区域标注 -->
    <TimeSeriesLabels name="label" toName="ts">
        <Label value="跑" background="red"/>
        <Label value="走" background="green"/>
        <Label value="飞" background="blue"/>
        <Label value="游" background="#f6a"/>
        <Label value="骑" background="#351"/>
    </TimeSeriesLabels>

    <!-- 对象标签：时间序列数据源 -->
    <TimeSeries name="ts" valueType="url" value="$timeseriesUrl"
                sep=","
                timeColumn="time"
                timeFormat="%Y-%m-%d %H:%M:%S.%L"
                timeDisplayFormat="%Y-%m-%d"
                overviewChannels="velocity">

        <Channel column="velocity"
                 units="公里/小时"
                 displayFormat=",.1f"
                 strokeColor="#1f77b4"
                 legend="速度"/>

        <Channel column="acceleration"
                 units="公里/小时^2"
                 displayFormat=",.1f"
                 strokeColor="#ff7f0e"
                 legend="加速度"/>
    </TimeSeries>
</View>
```

### 配置代码说明

以上代码为「活动标签栏 + 双通道时间序列 + 底部概览」。

1、标签：`TimeSeriesLabels name="label" toName="ts"` 声明可在时间轴上创建的区段类型；`Label` 的 `value` 为导出中的类别名，`background` 为区段填充色。

2、数据：`TimeSeries name="ts" value="$timeseriesUrl"` 从任务数据的 `timeseriesUrl` 字段加载 CSV；`sep` 为列分隔符；`timeColumn` 为时间列名；`timeFormat` 使用 strftime 风格解析时间字符串。

3、展示：`timeDisplayFormat` 控制时间轴/提示中的日期展示粒度；`overviewChannels` 指定用于底部缩略概览的通道。

4、通道：每个 `Channel` 的 `column` 对应 CSV 列名；`units`、`legend`、`strokeColor`、`displayFormat` 分别控制单位、图例、曲线颜色与数值格式。

### 示例数据（简要）

```json
{
  "data": {
    "timeseriesUrl": "/static/templates/project-templates-config/time-series-analysis/activity-recognition/timeseries.csv"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- CSV 需至少包含 `time`、`velocity`、`acceleration` 等列，且时间格式与 `timeFormat` 一致；请将示例路径替换为实际上传或可访问的静态资源地址。
