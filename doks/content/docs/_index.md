---
title: "文档指南"
description: "据吉文档的主页面，包含所有指南、参考和资源。"
summary: ""
date: 2023-09-07T16:12:03+02:00
lastmod: 2023-09-07T16:12:03+02:00
draft: false
weight: 999
toc: true
layout: "single"      # Doks uses 'list' for the main section, but sub-pages use 'docs'
type: "docs"        # This tells Doks to use the documentation sidebar logic
params:
  seo:
    title: "" # custom title (optional)
    description: "" # custom description (recommended)
    canonical: "" # custom canonical URL (optional)
    robots: "" # custom robot tags (optional)
  section:
    title: "Documentation"
    iconName: "book"
    startUrl: "/docs"
---
<style>
/* 提示框背景 */
.docs-alert-box {
    background-color: #f8f9fa;
    border: 1px solid #e9ecef;
    border-radius: 8px;
    padding: 1rem 1.5rem;
    color: #4b5563;
    margin-bottom: 2rem;
    font-size: 0.95rem;
}

/* 卡片基础样式 */
.docs-nav-card {
    border-radius: 12px;
    transition: all 0.2s ease-in-out;
    border: 1px solid #e5e7eb;
    background-color: #ffffff;
    height: 100%;
    display: flex;
    flex-direction: column;
    text-decoration: none !important;
}

/* 卡片悬浮效果 */
.docs-nav-card:hover {
    border-color: #0d6efd;
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -2px rgba(0, 0, 0, 0.025);
    transform: translateY(-2px);
}

/* 高亮状态 (类似截图中的 Overview) */
.docs-nav-card.active-card {
    border-color: #0d6efd;
    background-color: #f0f7ff;
}

/* 卡片内部元素 */
.docs-nav-card .card-body {
    padding: 2rem 1.5rem;
}

.docs-icon-wrapper {
    color: #0d6efd;
    margin-bottom: 1rem;
    display: flex;
    justify-content: center;
}

.docs-nav-card h5 {
    font-weight: 600;
    color: #111827;
    margin-bottom: 0.5rem;
    font-size: 1.1rem;
}

.docs-nav-card p {
    color: #6b7280;
    font-size: 0.875rem;
    margin-bottom: 0;
    line-height: 1.5;
}
</style>
<div class="docs-alert-box">
  帮您梳理如何使用此文档
</div>

<div class="row row-cols-1 row-cols-md-2 row-cols-lg-4 g-4 mb-5 text-center">

  <div class="col">
    <a href="/docs/quick_guides/" class="docs-nav-card active-card">
      <div class="card-body">
        <div class="docs-icon-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" viewBox="0 0 16 16">
            <path d="M8 0a8 8 0 1 1 0 16A8 8 0 0 1 8 0zM2.04 4.326c.325 1.329 2.532 2.54 3.717 3.19.48.263.793.434.743.484-.08.08-.162.158-.242.234-.416.396-.787.749-.758.775.04.034.41-.303.805-.733.398-.435.794-.86.76-.893-.03-.031-.383.332-.77.74-.383.404-.755.793-.787.76-.03-.031.29-.406.67-.84.384-.438.745-.85.713-.884-.033-.034-.41.332-.82.748-.415.422-.794.8-.826.766-.032-.034.25-.436.598-.902.348-.466.657-.88.625-.914-.031-.033-.377.307-.75.694-.373.387-.714.723-.746.689-.033-.035.21-.468.514-.984.303-.516.568-.973.535-1.008-.032-.034-.33.284-.64.63-.31.345-.588.64-.62.606-.032-.035.168-.5.427-1.066.26-.567.48-.1.448-1.034-.033-.035-.286.264-.542.58-.256.315-.483.58-.516.545-.033-.036.12-.533.328-1.15.207-.617.382-1.132.349-1.167-.033-.036-.234.246-.432.544-.2.298-.378.544-.411.51-.032-.036.069-.567.213-1.24.143-.674.264-1.238.23-1.272-.033-.037-.175.228-.31.504-.135.275-.255.498-.288.463-.032-.036.012-.605.088-1.33.076-.723.14-1.328.106-1.363-.033-.037-.107.208-.178.461-.07.252-.132.454-.165.418-.033-.038-.052-.647-.05-1.425v-1.42l-.128.14c-.07.078-.13.136-.164.1-.033-.038-.124-.694-.21-1.54-.084-.845-.155-1.537-.189-1.537s-.087.165-.119.366c-.031.2-.058.366-.092.366-.032 0-.22-1.084-.44-2.583-.22-1.498-.403-2.724-.403-2.724s0 .324.004.72c.003.395.004.719-.03.719-.032 0-.306-1.173-.642-2.793z"/>
          </svg>
        </div>
        <h5>快速上手指南</h5>
        <p>包含甲方接入指南、供应商操作手册等核心流程介绍。</p>
      </div>
    </a>
  </div>

  <div class="col">
    <a href="/docs/templates/" class="docs-nav-card">
      <div class="card-body">
        <div class="docs-icon-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" viewBox="0 0 16 16">
            <path d="M4 0h5.293A1 1 0 0 1 10 .293L13.707 4a1 1 0 0 1 .293.707V14a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V2a2 2 0 0 1 2-2zm5.5 1.5v2a1 1 0 0 0 1 1h2l-3-3zM4.5 6a.5.5 0 0 0 0 1h7a.5.5 0 0 0 0-1h-7zm0 2.5a.5.5 0 0 0 0 1h7a.5.5 0 0 0 0-1h-7zm0 2.5a.5.5 0 0 0 0 1h4a.5.5 0 0 0 0-1h-4z"/>
          </svg>
        </div>
        <h5>模版使用</h5>
        <p>浏览并学习如何使用 2D 矩形框等多种标注工具模板。</p>
      </div>
    </a>
  </div>

  <div class="col">
    <a href="/docs/concepts/" class="docs-nav-card">
      <div class="card-body">
        <div class="docs-icon-wrapper">
          <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" viewBox="0 0 16 16">
            <path d="M8.5 1.5a1 1 0 1 0-2 0 1 1 0 0 0 2 0zm-1.898 5.467A1.5 1.5 0 0 0 7 7.5v6.5a.5.5 0 0 0 1 0V7.5a.5.5 0 0 1 1 0v6.5a.5.5 0 0 0 1 0V7.5a1.5 1.5 0 0 0-2.602-.933l-1.898 1.9a1.5 1.5 0 0 0 2.122 2.12l.617-.616a.5.5 0 1 1 .707.708l-.617.616a2.5 2.5 0 0 1-3.536-3.536l1.898-1.9z"/>
          </svg>
        </div>
        <h5>概念解析</h5>
        <p>深入理解平台底层架构与系统级模型原理。</p>
      </div>
    </a>
  </div>

</div>

