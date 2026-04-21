# OCR 发票预命名实体（BIO 格式） (OCR Invoices — Pre-NER BIO)<no value>

# OCR 发票预命名实体（BIO 格式）使用说明

可以理解为「先在**图像**上把每个要参与分词/序列标注的片段框出来，再在框里**打字或改 OCR 结果**」。产出的是「框 + 文本」对，下游可拼成一行或多行纯文本，再套用 [发票命名实体标注（BIO 形式）](./ner-tagging-invoices-bio-format/) 这类模版做实体类型标注，从而形成 **OCR → 词界 → BIO NER** 的流水线。本配置源自社区模版库。

## 标注核心作用

1.  `Image` 开启 `zoomControl`，便于核对小字与表线；
2.  `RectangleLabels` 仅一类「分词单元」，与 `choice="single"` 配合，强调**单元化**框选；
3.  `TextArea` 设 `perRegion="true"`，**每个矩形一条转写**，`required="true"` 要求提交前填写。

## 基础操作步骤

1.  阅读任务说明，明确「分词单元」粒度（单字、词组还是整行表头）；
2.  选择 **分词单元**的标签；
3.  在图像上拖出矩形覆盖目标文字；
3.  再次点击已标单元，在对应输入框中录入或修正 OCR 文本；
4.  重复直至覆盖计划中的单元，自检后提交。

![OCR 发票预命名实体标注示例](./images/ocr-invoices-pre-ner-bio-format-example.png)

说明：输入完成后，点击输入框外即可自动保存；每个分词单元均可有多个文本输入。

## 注意事项

- `data.image` 须为可访问的图片 URL 或静态路径；大图需关注加载与内存；
- 若上游 **OCR 模型已输出框**，导入任务时需映射为与 `RectangleLabels` 兼容的预标格式（以平台预标能力为准）；
- `rows="1"` 适合短词；长段落可增大 `rows` 或改为多框拆分；
- BIO 实体类型**不在本步完成**，本步只产出词界与转写；类型标注通常在下游文本任务中完成；
- 配置注释中的 **awesome-label-studio-config** 路径仅供参考，以源仓库更新为准。

## 模板预览

![OCR 发票预命名实体模板预览](./images/ocr-invoices-pre-ner-bio-format.png)

## 模板配置
### 完整代码块

```html
<View>
  <Image name="image" value="$image" zoomControl="true"/>
  <RectangleLabels name="label" toName="image" choice="single">
    <Label value="分词单元" background="#FFA500"/>
  </RectangleLabels>
  <TextArea name="transcription"
            toName="image"
            perRegion="true"
            editable="true"
            rows="1"
            required="true"
            placeholder="Type or correct OCR text…"/>
</View>
```

### 配置代码说明

以上代码为「可缩放图像 + 矩形标签 + 按框转写」。

1、图像：`Image name="image" value="$image"` 加载任务数据中的图片字段；`zoomControl="true"` 显示缩放控件。

2、框选：`RectangleLabels name="label" toName="image"` 在图像上绘制矩形；`choice="single"` 表示标签工具行为以单选类为主（与平台文档一致时生效）。

3、转写：`TextArea name="transcription" toName="image" perRegion="true"` 为每个矩形区域提供独立输入；`editable="true"` 允许修改；`required="true"` 提交前须填写。

### 示例数据（简要）

```json
{
  "data": {
    "image": "/static/templates/project-templates-config/community-contributions/ocr-invoices-pre-ner-bio-format/ocr-invoices-pre-ner-bio-format.png"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 请将示例路径替换为实际上传或可访问的图片地址；若与静态资源目录约定不一致，请同步修改部署路径。
