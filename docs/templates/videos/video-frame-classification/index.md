# 视频帧分类 (Video Frame Classification)<no value>

# 视频帧分类使用说明

可以理解为「选择一种时间轴标签，并在时间线上标出对应片段」。与整段只选一个类的「视频分类」不同，本模版强调**随时间变化的帧级/段级**标注。它适合体育剪辑、监控行为、镜头节奏分析等需要细粒度时序标签的场景。

## 标注核心作用

1.  `TimelineLabels` 与 `Video` 共用 `toName`，标签与时间轴轨道联动；
2.  多色 `Label` 便于在轨道上区分不同类别；
3.  同一视频可包含多段、多类标注，导出为时序监督数据。

## 基础操作步骤

1.  在顶部点选要使用的类别（如「慢动作」），或使用快捷键 `[1]` `[2]` `[3]`（以平台为准）；
2.  播放或拖动时间轴到目标区间；
3.  按平台交互在时间线上创建区段，亦可拖拽改变区间长度；
4.  逐段完成标注后，检查轨道与画面是否一致，再提交。

![视频帧分类标注示例](./images/video-frame-classification-example.png)

说明：截图中①—③示意选标签、控制条与时间轴区段。

## 注意事项

- 导入任务时请将视频路径放在 **`data.video`** 下（见示例）；注释里仅写 `"video": "..."` 时需包一层 `data`；
- 片段边界、是否允许重叠、最短片段长度等，须在项目规范中约定；
- `height`、`timelineHeight` 可按素材分辨率与操作习惯调整；
- 类别名称与颜色可按业务修改 `Label` 的 `value`、`background`。

## 模板预览

![视频帧分类模板预览](./images/video-frame-classification.png)

## 模板配置
### 完整代码块

```html
<View style="padding: 0; margin: 0;">
  <TimelineLabels name="videoLabels" toName="video">
    <Label value="动作" background="#c813ec"/>
    <Label value="静止" background="#1d81cd"/>
    <Label value="慢动作" background="#54d651"/>
  </TimelineLabels>
  <Video name="video" value="$video" height="360" timelineHeight="80" style="display: block; margin: 0; padding: 0;" />
</View>
```

### 配置代码说明

以上代码在播放器上方提供时间轴标签条，下方为视频与时间线。

1、时间轴标签：`TimelineLabels name="videoLabels" toName="video"` 将可选类别绑定到名为 `video` 的视频对象；每条 `Label` 定义一类可绘制在时间轴上的片段样式。

2、视频：`Video name="video" value="$video"` 加载任务数据中的视频 URL；`height`、`timelineHeight` 控制画面与时间轴区域高度。

### 示例数据（简要）

示例路径中的 `?1` 等为可选查询参数，多在路径不变时用来刷新浏览器或 CDN 缓存；更新样例素材时改掉该数字即可，并非 mp4 格式要求。

```json
{
  "data": {
    "video": "/static/templates/project-samples/video-frame-classification.mp4?1"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 与 [视频分类](./video-classification/) 相比，本模版侧重**时间轴分段**而非整段单选；
- 若需多轨道或属性扩展，请在平台文档允许范围内增改组件。