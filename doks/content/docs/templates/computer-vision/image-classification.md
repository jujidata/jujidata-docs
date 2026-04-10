---
title: "图像分类 (Image Classification)"
description: "图像分类标注模版，通过单选/多选方式为整张图片选择类别标签，适配场景分类、内容审核、材质识别等任务。"
lead: "图像分类是计算机视觉中最基础的标注方式之一，核心是为整张图片选择一个或多个类别标签，以形成「图片 + 类别」的训练数据。据吉网提供直观的选择交互，适合快速构建分类数据集并用于图像分类模型训练。"
date: 2026-04-01 08:00:00
lastmod: 2026-04-01 08:00:00
draft: false
weight: 6
menu:
  sidebar_docs:
    parent: "模版使用"
params:
  seo:
      title: "图像分类 (Image Classification)"
      description: "图像分类标注模版，通过单选/多选方式为整张图片选择类别标签，适配场景分类、内容审核、材质识别等任务。"
      keywords: ["标注模板", "图像分类标注使用说明", "Image Classification"]
      canonical: "https://docs.jujidata.com/docs/templates/computer-vision/image-classification"
      robots: "index, follow"
---

# 图像分类使用说明

图像分类其实就是“看整张图，选对应标签”：先判断图片整体表达的内容，再选择对应标签，输出图片所属类别。它适合关注整体语义的任务，比如场景识别（高山/大海/沙漠等）、内容审核、材质与品类识别等，广泛应用于检索、推荐与通用分类模型训练等领域。

## 标注核心作用

1.  快速生成分类训练数据：为图片选择类别标签，形成标准化的「图片 + 类别」数据；
2.  支持多类别选择：可按业务需要配置多个候选类别标签供标注人员选择；
3.  标注效率高：无需绘制形状或逐点标注，适合大规模数据快速生产。

## 基础操作步骤

1.  查看图片内容并判断所属类别
2.  在底部选项区勾选对应类别标签

![图像分类标注示例](./images/image-classification-example.png)

说明：可使用右侧工具栏放大或缩小图片，便于更准确判断图片类别。

## 注意事项

- 类别定义需清晰且互斥规则一致（例如“高山/大海/沙漠”的判定边界），避免同图多义造成训练噪声；
- 若允许多选，请在标签体系中明确多选场景与组合规则，保证数据一致性；
- 标注过程中可随时取消或重新选择类别。

## 模板预览

![图像分类标注模板预览](./images/image-classification.png)

## 模板配置
### 完整代码块

```html
<View>
  <Image name="image" value="$image_path" zoom="true"/>
  <Choices name="choice" toName="image">
    <Choice value="高山"/>
    <Choice value="大海" />
    <Choice value="沙漠" />
  </Choices>
</View>
```

### 图像分类标注配置代码说明

以下代码用于实现图像分类标注功能，可直接复制使用，关键部分可根据实际分类需求修改。

1、图片加载组件：加载需要标注的图片，zoom="true" 表示支持图片缩放，无需修改。

```html
<Image name="image" value="$image_path" zoom="true"/>
```

2、分类选择核心配置

- `Choices` 是分类选择组件，用于定义可选类别集合并关联图片组件；
- `Choice` 用于声明一个可选类别；
- `value` 表示类别名称（本示例包含“高山/大海/沙漠”，可按业务修改为任意类别名称）。

```html
<Choices name="choice" toName="image">
  <Choice value="高山"/>
  <Choice value="大海" />
  <Choice value="沙漠" />
</Choices>
```

说明
- 代码可直接复制到标注配置文件中使用
- 只需增删 `Choice` 行并修改 `value` 即可扩展或调整分类标签

