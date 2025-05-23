---
title: 现代浏览器工作原理
tags: 浏览器
---

浏览器渲染全流程：`ParseHTML`(解析html) -> `Recalculate Style`(样式计算) -> `Layout`(布局) -> `Layerize`(分层) -> `Rasterize Paint`(光栅化) -> `Composite`(合成) -> `Display`(显示)

<!--more-->

**当浏览器搜索框输入网址后，发生了什么？**

**当浏览器拿到 server 返回的`html`后，发生了什么？**

## 总结

| 阶段                        | 作用                                   | 执行角色 / 线程 |
| -------------------------------- | --------------------------------------------- | ---------------------------- |
| **Parse（解析）**                    | 解析 HTML 和 CSS，生成 DOM Tree 和 CSSOM Tree        | CPU：渲染主线程（Main Thread）       |
| **Construct Render Tree（构建渲染树）** | 合并 DOM 和 CSSOM，去掉不可见元素，生成 Render Tree         | CPU：渲染主线程                    |
| **Layout（布局）**                   | 根据渲染树计算每个节点的位置和大小，得到一个拥有几何信息的 Frame Tree      | CPU：渲染主线程                    |
| **Layering（分层）**                 | 根据特定规则（如 transform、overflow、z-index）对元素进行图层划分 | CPU：渲染主线程                    |
| **Paint（绘制）**                    | 为每个图层生成绘制指令（Display List / Paint Records）     | CPU：渲染主线程                    |
| **Tiling（分块）**                   | 将每个图层按块（tile）划分，为后续并行光栅化准备                    | CPU 或 GPU 辅助（由合成线程调度）        |
| **Rasterization（光栅化）**           | 将绘制指令转为位图像素，生成 GPU 可处理的图层纹理                   | GPU：由合成线程提交任务                |
| **Compositing（合成）**              | 将所有光栅化后的图层纹理进行变换（如缩放、旋转）并合成为一个帧               | GPU：由合成线程调度                  |
| **Display（显示）**                  | 将合成后的帧写入帧缓冲区，通过显示控制器输出到屏幕                     | GPU + 显示控制器硬件                |

## 小知识

- 图层创建条件

1. 3D变换（transform: translateZ、rotateX等）
2. 透明度动画（opacity变化）
3. CSS滤镜（filter属性）
4. 定位元素（position: fixed/sticky）
5. overflow不为visible的元素
6. will-change属性指定的元素

- 参考来源
- > [Blink Renderer Core](https://chromium.googlesource.com/chromium/src/+/main/third_party/blink/renderer/core/README.md)
- > [life of pixels](https://bit.ly/lifeofapixel)
- > [chromium RenderingNG](https://developer.chrome.com/docs/chromium/renderingng?hl=zh-cn)
