---
layout: page
title: Projects
description: "A curated archive of my engineering projects across embedded systems, C++, networking, and backend development."
icon: fas fa-archive
order: 1
---

<div style="display:flex; align-items:center; gap:20px;">
  <img src="/assets/img/icons/compose-ui.svg"
       alt="ComposeUI logo"
       width="200"
       height="200"
       class="no-shimmer"
       style="flex-shrink:0;">
  <div style="line-height:1.4;">
    <a href="https://github.com/QWERTivore/ComposeUI.git" style="color:#4A90E2">ComposeUI</a> is a declarative framework built on top of <a href="https://lvgl.io/" style="color:#4A90E2">LVGL</a>, designed for embedded systems. The framework avoids dynamic allocation entirely — no heap, no RTTI, and no virtual dispatch. Widgets are defined declaratively using explicit models for geometry, style, behavior, and content. A small registry and pool handle instantiation so UI logic stays separate from LVGL calls. 
  </div>
</div>
