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

<div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-4 mb-5 text-center">

<div class="col">
  <a href="/docs/quick_guides/" class="docs-nav-card active-card">
    <div class="card-body">
      <div class="docs-icon-wrapper" style="color: #28a745;">
        <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" viewBox="0 0 1024 1024">
          <path d="M362.581333 853.546667h298.837334A278.272 278.272 0 0 1 512 1002.965333a278.272 278.272 0 0 1-149.418667-149.418666zM768 631.893333l85.333333 96.768v82.218667H170.666667v-82.218667l85.333333-96.768V384.213333c0-148.608 106.837333-275.072 256-321.92 149.162667 46.848 256 173.312 256 321.92v247.68z m-256-162.346666a85.333333 85.333333 0 1 0 0-170.666667 85.333333 85.333333 0 0 0 0 170.666667z"/>
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
      <div class="docs-icon-wrapper" style="color: #087990;">
        <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" fill="currentColor" viewBox="0 0 16 16">
          <path d="M2 6a6 6 0 1 1 10.174 4.12c-.2.354-.502.596-.746.815L11 11.031V12.5a.5.5 0 0 1-.5.5h-5a.5.5 0 0 1-.5-.5v-1.469l-.428-.38C4.326 10.428 3.854 10.185 3.654 9.83A5.99 5.99 0 0 1 2 6zm5.174 9.174a1 1 0 0 0 1.652 0l.441-.674H6.733l.441.674z"/>
        </svg>
      </div>
      <h5>功能介绍</h5>
      <p>具体了解每一个功能的细节和使用方法</p>
    </div>
  </a>
</div>

</div>

