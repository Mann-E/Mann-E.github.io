---
layout: post
title:  "How did we make image models?"
date:   2026-02-17 16:40:00 +0330
categories: blog
author: Muhammadreza Haghiri
---

Since 2023, when we started as a platform/business we always had our own models. First model which was actually pitched in different meetings (mostly personal/friendly) vas based on VQGAN+CLIP and later, we moved to the one and only _Stable Diffusion_ for a long time. Fine tuning these models is not an easy task if you want to get a good output, specially when _Midjourney_ has those aesthetic images and stable diffusion - despite being the best open source option - is still lagging behind the aesthetic war. 

However, lucky us, LoRA (Low Rank Adapters) and Dreambooth (a finetuning method) and also _model merging_ started to emerge. On the other hand, there was a crap ton of scripts to tune and merge SD models before these techniques ever existed. Stable Diffusion 1.5 and Stable Diffusion XL (specially turbo version) were basically the best bases for what we've been working on. First tries on SD 1.5 and them becoming viral was around January/February 2023 ([Interview with me on the topic is available here.](https://devm.io/machine-learning/ai-tools-haghiri)) and the goal was simply _to try to have closest open source model to Midjourney models at the time_. 

In this post, we explore how we went through this on tuning different models (Stable Diffusion 1.5 and derivatives, SDXL and derivatives and finally Flux.). So you can find a path to your generative AI journey if you have similar goals in mind. 

## Scraping The Web

## Knowledge Distillation 

## Features for future

## Conclusion