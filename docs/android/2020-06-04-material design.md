---
layout:     post
title:      "material design概述"
subtitle:   "质感设计，让屏幕如纸张般自然"
date:       2020-06-04
author:     "Rain"
header-img: "img/post-bg-2020-06-04.jpg"
tags:
    - material design


---

# Contents

{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents" from above
{:toc}


## 什么是Material Design

这是谷歌提出的一套集合视觉、交互和前端的界面设计规范，其目标有两点，一是用视觉形式囊括经典的设计原则，并将将前沿创新和技术展现出来，二是以移动端为基础，将所有尺寸、类型的设备统一起来。设计文档本身，就提供了一种很好的方式，帮你从各个角度思考和构建自己产品的规范。

Material Design为Android应用提供了一套设计规范，以及一些开箱即用的UI组件。设计人员可以根据设计规范设计出符合规范的能够体现品牌效果的产品。就算没有设计人员，开发人员也可以根据这些UI组件或者参考案例编写出好看的App。

其核心思想在于把物理世界的体验带进屏幕。去掉现实中的杂质和随机性，保留其最原始纯净的形态、空间关系、变化与过渡，配合虚拟世界的灵活特性，还原最贴近真实的体验，达到简洁与直观的效果。**简而言之，就是使用户使用起来舒服，符合真实体验**。

## 打破思维局限

- **在触屏为主的时代，悬停状态要慢慢淡出舞台**。
- **永远不要以为用户的设备尺寸是你可以预期的**：在不同的尺寸屏幕上，需要自适应，尽量不要使用滚动条。
- **阴影的目的，是不是美观而是纵深**：以阴影来体现层级概念，在空间上有z轴。
- **为了易用，确定尺寸**。
- **隐藏与否取决于必要性，而不是空间是否足够**。比如应用栏如果菜单项过多，那么就需要隐藏一些不重要的。
- **弹出框没必要多复杂**
- **慎用卡片**

## 众多约束细节

### 动画

Material Desigin注重动画效果。但是动画不只是装饰，它有含义，能表达元素、界面之间的关系，具备功能上的作用。

动画也需要符合物理现实的规律，不能花里胡哨，并且需要快速，不能慢吞吞影响用户的体验。具有以下几个特性。

- **响应式的**：能让用户明确的知道响应的内容。动画不宜过短，也不宜过长。在移动设备上的长动画大约是300-400ms，短动画大概是150-200ms。

- **自然的**：应该考虑物体的上升、下降、摩擦力等物理因素。

  ![16dx20160512](/img/in-post/20200606/16dx20160512.png)

- **可察觉的**：一个物体的运动，应该要被周围的物体所察觉。具有联动的动画效果，过渡的元素和周围物体的运动也需要精心的安排。例如

  ![13dx20160512](/img/in-post/20200606/13dx20160512.png)

  ![12dx20160512](/img/in-post/20200606/12dx20160512.png)

- **有引导意义的**：转场动画有助于引导下一步交互。

  ![9dx20160512](/img/in-post/20200606/9dx20160512.png)

[中文版来了！新版Material Design 官方动效指南](https://www.uisdc.com/material-motion-design-guideline)

[中文版来了！新版MATERIAL DESIGN 官方动效指南（二）](https://www.uisdc.com/material-motion-design-guideline-2)

[中文版来了！新版MATERIAL DESIGN 官方动效指南（三）](https://www.uisdc.com/material-motion-design-guideline-3)

### 颜色

Material Design颜色的使用，需要满足几个原则：

- **层次感**：突出重要的，具有交互性的颜色
- **清晰的**：与背景相比，要清晰
- **具有表现力**：能体现品牌风格，颜色不宜过多

基本的颜色风格：

![basic color](/img/in-post/20200606/basic_color.png)

#### Primary color

是App组件和屏幕上使用最多的颜色，体现的是应用的主题颜色。包括dark和light两个变体颜色。

![color](/img/in-post/20200606/color.png)

#### Secondary color

主要是起到强调和区分的作用，可选。适合用于：

- Floating action buttons
- Selection controls, like sliders and switches
- Highlighting selected text
- Progress bars
- Links and headlines

![secondary color](/img/in-post/20200606/secondary.png)

#### Surface, background, and error colors

- **Surface**:影响Surface组件：例如卡片、表格和菜单等
- **background**：出现在可滚动组件的背后。基础Surface和background颜色是\#FFFFFF
- **error**：错误的颜色。例如错误文本，默认为\#B00020

![other color](/img/in-post/20200606/other color.png)

#### On Color

材料背景上的文字或者图标的颜色。

![On color](/img/in-post/20200606/On Color.png)

### 布局

所有可操作元素最小点击区域尺寸：48dp X 48dp。

栅格系统的最小单位是8dp，一切距离、尺寸都应该是8dp的整数倍。以下是一些常见的尺寸与距离：

- 顶部状态栏高度：24dp
- Appbar最小高度：56dp
- 底部导航栏高度：48dp
- 悬浮按钮尺寸：56x56dp/40x40dp
- 用户头像尺寸：64x64dp/40x40dp
- 小图标点击区域：48x48dp
- 侧边抽屉到屏幕右边的距离：56dp
- 卡片间距：8dp
- 分隔线上下留白：8dp
- 大多元素的留白距离：16dp
- 屏幕左右对齐基线：16dp
- 文字左侧对齐基线：72dp

### 组件

Material Design对众多控件的设计实现，都有着详细的约束要求。思维导图如下：

![img](/img/in-post/20200606/uisdc-materialdesign-20170919-1.png)

