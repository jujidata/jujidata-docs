# PDF 分类 (PDF Classification)<no value>

# PDF 分类使用说明

可以理解为「先在阅读器中通读或扫读 PDF 正文，按项目标准选择最贴切的文章类型；最后依据规范给出星级评分」。例如范文库场景中区分「重要文章 / 新闻政要 / 高考范文」，并辅以 1–10 星表示推荐度或写作质量。它适合报刊归档、教辅筛选、内容安全分级等**整篇文档级**标注。

## 标注核心作用

1.  `Pdf` 内嵌阅读器，减少外链打开与格式错乱；
2.  `Rating` 与 `Choices` 均绑定 `pdf` 对象，导出时与同一任务对齐；
3.  单选类别与量表组合，兼顾粗粒度标签与细粒度排序信号。

## 基础操作步骤

1.  在 PDF 阅读器中阅读全文，可通过**工具栏**进行缩放、检索、高光等操作（具体以平台提供的工具为准），便于核对正文；
2.  在「重要文章」「新闻政要」「高考范文」中选择与文章最相符的类型；
3.  选定类型后，再按规范点选星级完成评分；
4.  自检是否与规范一致后提交。

![PDF 分类标注示例](./images/pdf-classification-example.png)

说明：推荐先完成类型选择再打星；截图中①②示意类别与评分；可借助工具栏做缩放、高光等辅助阅读。

## 注意事项

- `data.pdf` 须为可访问的 PDF 路径或 URL；大文件需关注加载耗时与浏览器内存；
- `maxRating`、`icon`、`size` 可按产品规范调整；改为 Likert 文案时需同步培训材料；
- 类别文案与数量变更时，请同步 `Choice` 与下游评测映射；
- 若需段落级标注，请改用支持区域或文本层的组件（以平台能力为准）。

## 模板预览

![PDF 分类模板预览](./images/pdf-classification.png)

## 模板配置
### 完整代码块

```html
<View>
  <Header value="请将文章分类并进行打分："/>
  <Rating name="rating" toName="pdf" maxRating="10" icon="star" size="medium" />

  <Choices name="choices" choice="single-radio" toName="pdf" showInline="true">
    <Choice value="重要文章"/>
    <Choice value="新闻政要"/>
    <Choice value="高考范文"/>
  </Choices>
  <Pdf name="pdf" value="$pdf"/>
</View>
```

### 配置代码说明

以上代码在界面上自上而下为「说明 + 星级 + 横向单选 + PDF 阅读器」。**建议操作流程**为：先利用阅读器（含工具栏缩放、高光等）通读正文，再选择文章类型，最后给出星级评分。

1、提示：`Header` 文案可按任务改写。

2、评分：`Rating name="rating" toName="pdf"` 绑定到 PDF 对象；`maxRating="10"` 为十分制；`icon`、`size` 控制星级外观。

3、分类：`Choices` 使用 `choice="single-radio"` 与 `showInline="true"`，三个互斥选项。

4、文档：`Pdf name="pdf" value="$pdf"` 从任务数据加载 PDF；阅读器工具栏提供的缩放、高光等功能以当前平台为准。

### 示例数据（简要）

```json
{
  "data": {
    "pdf": "/static/templates/project-templates-config/structured-data-parsing/pdf-classification/pdf-classification.pdf"
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 路径需与实际上传的静态资源或附件存储一致。