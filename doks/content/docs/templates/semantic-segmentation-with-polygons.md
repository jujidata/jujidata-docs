---
title: "基于多边形的语义分割 (Polygon Semantic Segmentation)"
description: "高精度语义分割标注模版，支持复杂轮廓目标标注，适配多场景语义分割需求。"
lead: "基于多边形的语义分割是计算机视觉中高精度目标分割的核心标注方式，适用于轮廓不规则、边缘复杂的目标标注，据吉网通过优化多边形绘制交互，兼顾标注精度与效率，助力语义分割模型训练。"
date: 2026-03-22T08:00:00+08:00
lastmod: 2026-03-22T08:00:00+08:00
draft: false
weight: 2
menu:
  sidebar_docs:
    parent: "模版使用"
params:
  seo:
      title: "基于多边形的语义分割 (Polygon Semantic Segmentation)"
      description: "高精度语义分割标注模版，支持复杂轮廓目标标注，适配多场景语义分割需求。"
      keywords: ["标注模板", "基于多边形的语义分割"]
      canonical: "https://www.jujidata.com"
      robots: "index, follow"
---

# 基于多边形的语义分割使用说明

基于多边形的语义分割，核心是通过绘制不规则多边形，精准框选目标物体的完整轮廓，实现像素级的语义标注。区别于矩形框的粗略定位，适用于车辆、行人、动植物、建筑等轮廓复杂的目标标注场景，广泛应用于自动驾驶、图像识别、医学影像分析等领域。

## 标注核心作用

1.  实现像素级目标分割：精准捕捉目标边缘，区分目标与背景，为语义分割模型提供高精度训练数据；
2.  适配复杂轮廓场景：解决矩形框标注无法精准覆盖不规则目标的问题，提升标注数据的准确性；
3.  支持多目标、多类别标注：可同时标注多个不同类别的目标，每个目标单独绘制多边形，关联对应标签与属性。

## 基础操作步骤

1. 选择不同颜色的标签，以区分不同目标
2. 点击图像边缘关键点，依次绘制多边形，贴合目标轮廓
3. 完成绘制后，再次点击第一个关键点，即可自动生成该目标的语义分割标注

![带掩码的语义分割示例](./images/semantic-segmentation-with-polygons-example.png)

说明：可使用右侧工具栏放大或缩小图片，便于进行精准标注。

## 注意事项

- 绘制多边形时，关键点尽量贴合目标边缘，避免遗漏或多余标注，确保轮廓完整；
- 若目标轮廓复杂，可多添加关键点，提升标注精度；
- 不同类别的目标需选择对应标签，避免标签混淆，影响模型训练效果；
- 标注过程中可随时撤销操作，或修改已绘制的多边形轮廓。

## 模板预览

![基于多边形的语义分割使用说明](./images/semantic-segmentation-with-polygons.png)


## 模板配置
### 完整代码块

```html
<View>
    <Header value="选择标签并且点击图片开始标注"/>
    <Image name="image" value="$image_path" zoom="true"/>
    <PolygonLabels name="label" toName="image"
                   strokeWidth="3" pointSize="small"
                   opacity="0.9">
      <Label value="人物一" background="red"/>
      <Label value="人物二" background="blue"/>
      <Label value="人物三" background="green"/>
      <Label value="人物四" background="yellow"/>
      <Label value="人物五" background="purple"/>
    </PolygonLabels>
</View>
```

### 多边形语义分割标注配置代码说明
以下代码用于实现多边形语义分割标注功能，可直接复制使用，关键部分可根据实际标注需求修改。

1、标注页面顶部引导提示，可修改为符合自身需求的引导文字 

```html
<Header value="选择标签并且点击图片开始标注"/>
```


2、加载需要标注的图片，zoom="true"表示支持图片缩放，无需修改
```html
<Image name="image" value="$image_path" zoom="true"/>
```

3、多边形标注核心配置，控制多边形样式和标注标签
<!--TODO: 以下关键字和标签等，可设计为跳转链接，跳转到本网站的相关内容，例如：[lable]() https://github.com/jujidata/jujidata-docs/issues/9 -->

- 可以通过改变 strokeWidth 的值，来修改多边形线条宽度，可选2、4等数值
- 可以通过改变 pointSize 的数值按需调整关键点大小，可选small、large
- 多边形透明度 opacity ，0.5-1.0之间可调，不遮挡图片为宜
- 标注标签 Label ，value为标签名称，background为标签颜色，可添加、删除、修改标签及颜色

```html
<PolygonLabels name="label" toName="image" strokeWidth="3" pointSize="small" opacity="0.9">
      <Label value="人物一" background="red"/>
      <Label value="人物二" background="blue"/>
      <Label value="人物三" background="green"/>
      <Label value="人物四" background="yellow"/>
      <Label value="人物五" background="purple"/>
    </PolygonLabels>
</View>
```

说明：代码可直接复制到标注配置文件中，主要修改引导提示、标签名称及颜色、多边形样式即可适配不同标注场景。
