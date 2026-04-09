---
title: "ASR 假设选择 (ASR Hypotheses)"
description: "ASR 假设选择标注模版，结合波形播放器与多条候选转写，支持在听清音频后勾选最贴近真实发音的一条结果。"
lead: "该任务用于从多条自动语音识别（ASR）候选中选出最可信的一条：先借助播放器反复听辨，再与候选文本逐字对照，输出可用于纠错评测与模型迭代的标注结果。"
date: 2026-04-01 08:00:00
lastmod: 2026-04-01 08:00:00
draft: false
weight: 21
menu:
  sidebar_docs:
    parent: "模版使用"
params:
  seo:
      title: "ASR 假设选择 (ASR Hypotheses)"
      description: "ASR 假设选择标注模版，结合波形播放器与多条候选转写，支持在听清音频后勾选最贴近真实发音的一条结果。"
      keywords: ["标注模板", "ASR", "语音识别", "假设选择", "ASR Hypotheses"]
      canonical: "https://docs.jujidata.com/docs/templates/asr-hypotheses"
      robots: "index, follow"
---

# ASR 假设选择使用说明

ASR 假设选择可以理解为「先听清，再选对」：同一段音频会对应多条略有差异的转写候选，标注员需要结合听觉判断，选出与说话内容最一致的一条。

这个模板适合语音识别纠错、候选重排序、听写评测等任务。播放器支持音量调节、语速调节、前进/后退，以及在波形上选区循环播放，适合对可疑片段反复比对后再选择答案。

## 标注核心作用

1.  为 ASR 多候选输出提供人工裁决，形成高质量监督数据；
2.  帮助识别音近词、形近词与语义干扰，提升模型鲁棒性；
3.  支持检索难例与误识别模式，反哺模型迭代。

## 基础操作步骤

1.  先完整听一遍音频，了解整体语义；
2.  用前进、后退、调速和选区循环精听关键片段；
3.  在候选列表中勾选与音频最一致的一项；
4.  提交前再次核对关键差异词。


说明：建议优先以音频事实为准，不要仅凭文本流畅度做判断。

## 注意事项

- 听不清时优先降低语速并重复播放问题区间；
- 关注专有名词、术语和否定词，避免“听起来像”但语义错误；
- 候选列表应与音频内容同域同任务，避免无效干扰项。

## 模板预览



## 模板配置
### 完整代码块

```html
<View>
  <Audio name="audio" value="$audio_path" zoom="true" hotkey="ctrl+enter" />
  <Choices name="transcriptions" toName="audio" value="$transcriptions" selection="highlight"/>
  <Style>
    .lsf-choice__item {
      padding: var(--spacing-tighter) var(--spacing-tight);
      box-shadow: 0 2px 4px rgba(var(--color-neutral-shadow-raw) / calc(16% * var(--shadow-intensity))) ;
      background-color: var(--color-neutral-background);
      border-radius: var(--corner-radius-small);
      margin: 0 var(--spacing-tighter) var(--spacing-tighter) var(--spacing-tighter);
      line-height: 1.6rem;
      transition: all 150ms ease-out;
    }

    .lsf-choice__item:hover {
      background-color: var(--color-primary-emphasis-subtle);
    }
  </Style>
</View>
```
### ASR 假设选择配置代码说明

1、音频：`Audio` 加载 `$audio_path`；`zoom="true"` 便于放大波形细听；`hotkey` 可绑定常用提交快捷键（以平台为准）。
2、候选：`Choices` 通过 `value="$transcriptions"` 注入多条转写；`toName="audio"` 将选择与音频任务绑定；`selection="highlight"` 用于高亮当前选中项。
3、样式：`Style` 内自定义候选项卡片外观与悬停效果，可按品牌规范调整。

说明

- 代码可直接复制到标注配置文件中使用；
- `transcriptions` 为对象数组，每项需包含 `value` 字段；
- 若需扩展为 N 条候选，保持数组结构一致即可。
