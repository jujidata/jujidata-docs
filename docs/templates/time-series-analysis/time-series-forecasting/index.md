# 时间序列预测 (Time Series Forecasting)<no value>

# 时间序列预测使用说明

可以理解为「先在曲线上标出你认为**适合用来预测**的一段（或多段），再进行预测」。例如量化或教学场景中，用历史股价序列构造「可预测窗口 + 方向标签」，便于训练或评测**预测辅助**类模型。

## 标注核心作用

1.  `Header` 分两处说明**框选目的**与**趋势选项含义**，降低歧义；
2.  `TimeSeriesLabels` 与 `TimeSeries` 通过 `toName="stock"`（名称可改）绑定，支持在「股价」曲线上拖选红色「区域」；
3.  `Choices` 未使用 `perRegion="true"` 时，通常表示**每条任务一次**的「上涨 / 下跌 / 持平」判断，与区段标注组合导出（具体字段结构以平台为准）。

## 基础操作步骤

1.  阅读顶部「在时间序列中框选可预测区间」及项目细则，明确可预测区间的定义；
2.  选择 **区域**标签；
3.  在主图与概览上拖选一处或多处区间；
4.  阅读「预测下一趋势」说明，在 **上涨 / 下跌 / 持平** 中择一。

![时间序列预测标注示例](./images/time-series-forecasting-example.png)

说明：趋势预测多为单选，请注意项目要求。

## 注意事项

- 任务数据字段为 **`csv`**，须含 `time` 与数值列 **`value`**（与 `Channel column` 一致）；时间格式须与 `timeFormat` 匹配；
- `overviewChannels="value"` 表示底部缩略图跟随 `value` 通道；若改列名请同步修改；
- 若业务需要「**每个区域各选一种趋势**」，应将 `Choices` 改为 `perRegion="true"` 并调整培训说明；当前配置更偏向**整题一个趋势标签**；
- 股价等金融数据仅作示例，实际项目请替换列名、图例与合规文案。

## 模板预览

![时间序列预测模板预览](./images/time-series-forecasting.png)

## 模板配置
### 完整代码块

```html
<View>
    <!-- 控制标签：区域标注 -->
    <Header value="在时间序列中框选可预测区间："/>
    <TimeSeriesLabels name="predictable" toName="stock">
        <Label value="区域" background="red" />
    </TimeSeriesLabels>

    <!-- 对象标签：时间序列数据源 -->
    <TimeSeries name="stock" valueType="url" value="$csv"
                sep=","
                timeColumn="time"
                timeFormat="%Y-%m-%d %H:%M:%S.%L"
                timeDisplayFormat="%Y-%m-%d"
                overviewChannels="value">

        <Channel column="value"
                 displayFormat=",.1f"
                 strokeColor="#1f77b4"
                 legend="股价"/>
    </TimeSeries>
    <Header value="预测下一趋势："/>
    <Choices name="trend_forecast" toName="stock">
        <Choice value="上涨"/>
        <Choice value="下跌"/>
        <Choice value="持平"/>
    </Choices>
</View>
```

### 配置代码说明

以上代码为「说明 + 区域标签 + 单通道时间序列 + 说明 + 趋势三选一」。

1、提示：两个 `Header` 分别引导框选区间与趋势判断。

2、区段：`TimeSeriesLabels name="predictable" toName="stock"` 声明可编辑区域；`Label value="区域"` 为导出中的类型名。

3、序列：`TimeSeries name="stock"` 与数据字段、`toName` 一致；`Channel column="value"` 对应 CSV 数值列，`legend="股价"` 为图例文案。

4、趋势：`Choices name="trend_forecast" toName="stock"` 将选项关联到同一时间序列对象；未设 `perRegion` 时多为**任务级**单选（与平台行为一致时可直接使用）。

### 示例数据（简要）

```json
{
  "data": {
    "csv": "/static/templates/project-templates-config/time-series-analysis/time-series-forecasting/timeseries.csv"
  }
}
```

说明

- 代码可直接复制到标注配置文件中使用；
- 请将示例 URL 替换为实际上传或可访问的静态资源地址；CSV 须包含 `time`、`value` 列且时间格式与 `timeFormat` 一致。
