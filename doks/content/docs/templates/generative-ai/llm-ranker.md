---
title: "大语言模型排序器 (LLM Ranker)"
description: "大语言模型排序器模版，在提示词下将多条候选回答拖拽分桶，区分高相关与存在偏见或不当表述，适配列表级排序与内容安全筛选。"
lead: "将同一提示下的多条模型输出集中展示，通过 Ranker 与 Bucket 完成拖拽分档，便于构建 K 选排序、分面标签与安全审核数据集。"
date: 2026-04-13 08:00:00
lastmod: 2026-04-13 08:00:00
draft: false
weight: 4
menu:
  sidebar_docs:
    parent: "模版使用"
params:
  seo:
      title: "大语言模型排序器 (LLM Ranker)"
      description: "大语言模型排序器模版，在提示词下将多条候选回答拖拽分桶，区分高相关与存在偏见或不当表述，适配列表级排序与内容安全筛选。"
      keywords: ["标注模板", "LLM", "排序", "Ranker", "生成式 AI", "内容安全"]
      canonical: "https://docs.jujidata.com/docs/templates/generative-ai/llm-ranker"
      robots: "index, follow"
---

# 大语言模型排序器使用说明

可以理解为「先读任务说明和提示词，再把左侧全部候选卡片拖到中间『高相关』或右侧『偏见/不当』栏里」。例如讨论中国空间站时，客观、扣题的回答归入高相关，明显偏见或攻击性表述归入问题桶。它适合多候选重排序、批量质检与红队数据标注。

## 标注核心作用

1.  一次任务覆盖多条候选回答，减少重复打开样本的成本；
2. 结果直接对应「相关 / 风险」等离散标签，便于统计与训练；
3.  `List` + `Ranker` + `Bucket` 组合是常见的拖拽排序数据模型。

## 基础操作步骤

1.  阅读「任务」与「提示词」，明确排序与分桶标准；
2.  在「全部候选回答」中浏览每条卡片的标题与正文；
3.  将卡片拖拽至「高相关回答」或「存在偏见或不当表述」（以平台拖拽交互为准）；
4.  确认无遗漏、无放错桶后提交。

![大语言模型排序器标注示例](./images/llm-ranker-example.png)

说明：截图中①②与箭头为操作示意；卡片右上角小三角可能为展开或拖拽相关控件，以实际产品为准。

## 注意事项

- 导入字段 `items` 须与 `List` 的 `value="$items"` 一致；单条对象需包含展示所需字段（如 `title`、`body`、`id`），具体键名以平台 List 组件约定为准；
- 样式表中 `.htx-ranker-item p:last-child { display: none }` 会隐藏最后一项 `p` 节点，若正文丢失请检查 DOM 结构或删除该规则；
- CSS 变量（如 `--color-neutral-background`）依赖主题，缺失时需改为具体色值；
- 桶名 `relevant_results`、`biased_results` 会进入导出结果，改名需同步数据与质检规则。

## 模板预览

![大语言模型排序器模板预览](./images/llm-ranker.png)

## 模板配置
### 完整代码块

```html
<View>
  <View style="display: flex; align-items: center; font-size: 1em;">
    <View style="margin: var(--spacing-tight) var(--spacing-tight) 0 0;">
      <Header value="任务：" style="font-size: 1em;"/>
    </View>
    <Text name="task" value="$task"/>
  </View>
  <View style="display: flex; align-items: center; box-shadow: 0px 2px 5px 0px rgba(var(--color-neutral-shadow-raw) / 10%); padding: var(--spacing-tight); border-radius: 5px; background-color: var(--color-neutral-background); font-size: 1.25em; border: 1px solid var(--color-neutral-border); margin-bottom: var(--spacing-tight);">
    <View style="margin: 0 1em 0 0">
      <Header value="提示词："  />
    </View>
    <Text name="prompt" value="$prompt"/>
  </View>
  <View>
    <List name="answers" value="$items" title="全部候选回答" />
    <Ranker name="rank" toName="answers">
      <Bucket name="relevant_results" title="高相关回答" />
      <Bucket name="biased_results" title="存在偏见或不当表述" />
    </Ranker>
  </View>
</View>
```

### 配置代码说明

以上代码由任务说明、提示词、候选列表与拖拽排序区组成。

1、任务与提示：前两行 `View` 使用 flex 横向排列 `Header` 与 `Text`，分别绑定 `$task`、`$prompt`。

2、候选列表：`List name="answers" value="$items" title="全部候选回答"` 将数据中的 `items` 渲染为可排序条目集合，`name` 供 `Ranker` 引用。

3、样式：`Style` 内以 `.htx-ranker-column`、`.htx-ranker-item` 控制分栏与卡片外观。

4、排序：`Ranker name="rank" toName="answers"` 将列表项拖入两个 `Bucket`：`relevant_results`（高相关）与 `biased_results`（偏见或不当表述）。

说明
- 代码可直接复制到标注配置文件中使用；
- 桶数量、标题与 `items` 字段可在业务侧扩展；
- 若列表项渲染异常，请对照平台对 `List` 数据结构的文档核对键名。