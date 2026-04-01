---
title: "关键点标注 (Keypoints)"
description: "关键点（Keypoints）标注模版，支持为目标关键部位逐点标注，并输出关键点名称与坐标，适配人脸、人体姿态、动物与零部件定位等场景。"
lead: "关键点标注是计算机视觉中用于描述目标关键部位位置的重要标注方式，通过在图像中逐点选择关键点并绑定类别名称，为姿态估计、人体关键部位检测、动物部位定位以及细粒度缺陷分析等模型训练提供标准化数据。"
date: 2026-04-01 08:00:00
lastmod: 2026-04-01 08:00:00
draft: false
weight: 5
menu:
  sidebar_docs:
    parent: "模版使用"
params:
  seo:
      title: "关键点标注 (Keypoints)"
      description: "关键点（Keypoints）标注模版，支持为目标关键部位逐点标注，并输出关键点名称与坐标，适配人脸、人体姿态、动物与零部件定位等场景。"
      keywords: ["标注模板", "关键点标注使用说明", "Keypoints"]
      canonical: "https://docs.jujidata.com/docs/templates/keypoints"
      robots: "index, follow"
---

# 关键点标注使用说明

关键点标注的核心是在图像上为目标的关键部位逐点点击标记，并为每个关键点绑定名称标签。区别于矩形框的粗定位与分割的像素级覆盖，关键点标注关注的是关键部位的相对位置关系，适用于人脸五官、人体姿态（眼睛、鼻子、耳朵等关键部位）、动物特征点以及工业零件关键结构点的定位任务，广泛应用于姿态估计、视觉跟踪、检测与精细化识别等领域。

## 标注核心作用

1.  为姿态与部位检测提供训练数据：逐点标记关键部位位置，并绑定关键点名称，为关键点检测类模型提供监督信号；
2.  支持多目标、多关键点标注：同一图像可为多个目标分别标注，关键点列表可包含多种部位名称；
3.  兼顾标注效率与细粒度表达：在不需要轮廓级精度的情况下，通过关键点坐标实现较高的结构化信息输出。

## 基础操作步骤

1.  在底部标签栏选择要标注的关键点标签
2.  在图像上依次点击关键点位置（例如眼睛、鼻子、耳朵）
3.  根据需要继续为同一目标或其他目标添加关键点

![关键点标注示例](./images/keypoints-example.png)

说明：可使用右侧工具栏放大或缩小图片，便于进行精准点击标注。

## 注意事项

- 关键点应尽量落在关键部位的中心或特征点位置，避免偏移导致模型学习误差；
- 同一关键点类别需保持一致的标注位置定义（例如“眼睛”是瞳孔中心还是眼角中心）；
- 标注过程中可随时撤销、修改或删除已绘制的关键点。

## 模板预览

![关键点标注模板预览](./images/keypoints.png)

## 模板配置
### 完整代码块

```html
<View>
    <Image name="image" value="$image_path" zoom="true"/>
    <KeyPointLabels name="kp-1" toName="image">
      <Label value="眼睛" background="red" />
      <Label value="鼻子" background="green" />
      <Label value="耳朵" background="blue" />
    </KeyPointLabels>
</View>
```

### 关键点标注配置代码说明

以下代码用于实现关键点标注功能，可直接复制使用，关键部分可根据实际关键点需求修改。

1、图片加载组件：加载需要标注的图片，zoom="true" 表示支持图片缩放，无需修改。

```html
<Image name="image" value="$image_path" zoom="true"/>
```

2、关键点标注核心配置

<!--TODO: 以下关键字和标签等，可设计为跳转链接，跳转到本网站的相关内容，例如：[lable]() https://github.com/jujidata/jujidata-docs/issues/9 -->

- `KeyPointLabels`  是关键点标注组件，用于绑定关键点集合并关联图片组件；
- `value` 表示关键点名称（本示例包含“眼睛/鼻子/耳朵”，可按业务修改为任意关键点名称）；
- `background` 控制关键点在界面中的显示颜色，可使用常见颜色名或颜色值。

```html
<KeyPointLabels name="kp-1" toName="image">
  <Label value="眼睛" background="red" />
  <Label value="鼻子" background="green" />
  <Label value="耳朵" background="blue" />
</KeyPointLabels>
```

说明
- 代码可直接复制到标注配置文件中使用
- 只需修改关键点名称和颜色即可适配不同场景
- 若需增加更多关键点，直接复制 `Label` 行并修改 `value`/`background`

