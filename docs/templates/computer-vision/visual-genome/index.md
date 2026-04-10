# 视觉基因组 (Visual Genome)<no value>

# 视觉基因组使用说明

视觉基因组可以理解为“先依次选好区域、属性、关系，再框选图像”：按顺序在「区域」「属性」「关系」三栏中分别点选对应标签后，再在图像上用矩形框标出目标区域；框选完成后，系统会结合所选标签自动生成与该区域相关的描述信息。它适合复杂场景理解任务，比如图像描述生成、视觉问答、关系推理与多模态训练等场景。

## 标注核心作用

1.  同时建模对象与语义关系：不仅知道“有什么”，还知道“是什么样”和“彼此关系”；
2.  支持分栏标签管理：区域、属性、关系分组展示，降低复杂标注任务的操作负担；
3.  提升数据表达能力：相比单一检测标注，更适合高阶语义理解任务。

## 基础操作步骤

1.  在「区域」栏选择目标描述标签；
2.  在「属性」栏选择属性标签；
3.  在「关系」栏选择关系标签；
4.  在图像上框选对应区域；框选完成后，系统会自动生成与该区域相关的描述信息。

![视觉基因组标注示例](./images/visual-genome-example.png)

说明：请按「区域 → 属性 → 关系」的顺序依次选择标签后再框选，便于生成描述与标签一致；若需标注多个目标，可重复上述流程。

## 注意事项

- 区域标签应准确对应可见目标，避免一个标签覆盖多个不相关对象；
- 属性标签需与对象本身一致，避免将关系信息写入属性；
- 关系标签建议使用统一表达口径，减少同义描述带来的数据噪声。

## 模板预览

![视觉基因组模板预览](./images/visual-genome.png)

## 模板配置
### 完整代码块

```html
<View>
  <View style="display: flex; flex-wrap: wrap;">
    <View className="label-column">
      <Header value="区域"/>
      <RectangleLabels name="regions" toName="image" value="$regions"/>
    </View>
    <View className="label-column">
      <Header value="属性"/>
      <RectangleLabels name="attributes" toName="image" value="$attributes"/>
    </View>
    <View className="label-column">
      <Header value="关系"/>
      <RectangleLabels name="relationships" toName="image" value="$relationships"/>
    </View>
  </View>
  <Image name="image" value="$image_path"/>
  <Style>
  .label-column .lsf-labels {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  </Style>
</View>
```

### 视觉基因组配置代码说明

以上代码用于实现“区域/属性/关系”三类标签的分栏展示与标注，适合语义层次较丰富的图像任务。

1、分栏标签区：通过多个 `RectangleLabels` 组件分别承载区域、属性、关系标签，并使用 `value="$regions"`、`value="$attributes"`、`value="$relationships"` 动态注入标签集合。

2、图片组件：`Image` 用于加载待标注图像。

3、样式组件：`Style` 将每个分栏内标签改为纵向排列，提升可读性和选择效率。

### 示例数据（简要）

以下示例数据用于说明如何向模板传入图像地址和三类标签列表，可按项目需要替换具体内容。

```json
{
  "data": {
    "image_path": "/static/templates/project-templates-config/computer-vision/visual-genome/visual-genome.png",
    "regions": [
      { "value": "这有一只老虎" },
      { "value": "这有一捆木头" },
      { "value": "这是草地" }
    ],
    "attributes": [
      { "value": "把老虎框起来" },
      { "value": "木头是棕色的" },
      { "value": "草地是绿色的" }
    ],
    "relationships": [
      { "value": "木头在老虎后面" },
      { "value": "老虎趴在草地上" }
    ]
  }
}
```

说明
- 代码可直接复制到标注配置文件中使用；
- 三类标签可按业务增删，保持字段名与模板配置一致即可；
- 建议先统一标签命名规范，再批量开展标注。
