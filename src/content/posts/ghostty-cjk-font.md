---
title: "Ghostty 在 macOS 上的中文渲染问题：原因与修复"
description: >-
  macOS 上 Ghostty 的中文回退会选中 90 年代的老字体，导致字大小不一、全角标点偏上。
  用 font-family 显式指定中文回退字体即可修复。
pubDatetime: 2026-08-19
modDatetime: 2026-08-19
draft: true
tags: ["guide"]
---

## 背景

我在 macOS 上使用 Ghostty 终端时，遇到了中文渲染异常：同一段文字里有些字大、有些字小，句号等全角标点浮在文字上方。排查后发现这不是配置了某个奇怪字体，而是 Ghostty 的中文字体 fallback 机制选错了字体。

## 问题

Ghostty 对主字体缺少的字形做回退时，会调用 CoreText 列出支持该字符的全部字体，按字形数量降序排序，取第一个能加载的。在我的系统上排序结果前三名是：

- STHeitiSC-Light（华文黑体细体，52268 个字形）
- STHeitiSC-Medium（51835 个字形）
- Arial Unicode MS（50377 个字形）

PingFang SC 排不进前五。STHeiti 和 Arial Unicode MS 都是 90 年代的老字体，度量与现代字体差异很大。实测 STHeiti 的句号字形位于基线上方（y=+0.2），而 PingFang 的在基线下方（y=-1.5），这就是句号浮在上方的原因。

一个细节放大了问题：Ghostty 对汉字的回退走的是另一条路径（CTFontCreateForString，能正确返回 PingFang），对全角标点走的才是上述按字形数量排序的路径。一段文本实际用哪条路径，取决于这段文本第一个缺失的字符是什么。所以同一行里有时正常、有时异常，观感就是字大小不一。

这个现象在 macOS 26.5（2026 年 5 月）系统更新后集中出现，Ghostty 的 issue #12916 有同样的报告。按字形数量排序的逻辑本身是 2023 年就有的（PR #811），所以这不是 Ghostty 版本更新引入的，而是 macOS 系统更新改变了字体回退行为，与 Ghostty 的机制相互作用的结果。

## 解决方案

最简单的方式是显式指定中文回退字体。Ghostty 的 `font-family` 可以重复配置，第一行是主字体，后面的行按顺序作为回退：

```ini
font-family = Menlo
font-family = PingFang SC
```

显式指定的回退字体不经过上述排序，会稳定命中 PingFang。这个方案也是官方文档和社区（包括小红书上的笔记）推荐的做法。

如果主字体和回退字体之外还有个别符号想精确控制，可以再叠加 `font-codepoint-map` 按 Unicode 区间强制指定字体：

```ini
font-codepoint-map = U+2013-U+2030=PingFang SC
font-codepoint-map = U+3000-U+303F=PingFang SC
font-codepoint-map = U+4E00-U+9FFF=PingFang SC
font-codepoint-map = U+FF01-U+FF5E=PingFang SC
```

另外两个可选动作：给 Ghostty 换一个自带汉字的等宽字体作为主字体（例如更纱黑体 Sarasa Mono SC），这样汉字和标点都在主字体里，根本不触发回退；如果 `/Library/Fonts` 下存在 `Arial Unicode.ttf` 的软链（某些工具会创建），它会因为注册顺序靠前而进一步抬高 Arial Unicode MS 的地位，可以一并删掉。
