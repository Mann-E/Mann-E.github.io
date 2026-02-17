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

The very first (and somehow, most logical way) of gathering data with midjourney's aesthetic was scraping the web and websites sharing art or photographs with similar aesthetics. But it calls for a lot of problems which I describe here. First one is always copyrights. We do not know what is legal status of using those pictures in terms of copyrights and it can become a legal problem in future. The second one is that not all pictures from the internet are in good shape or quality. 

In my personal experience, _Stable Diffusion 1.5_ and its derivatives (Like DreamShaper) have tendency of inheriting the low quality from a low quality source data. So low quality input could easiyl ruin the data. On the other hand, Stable Diffusion (from 1.5 to SD, the versions we worked on before migration to Flux models as a base) has a strict rule of feeding on fixed size images and images must have been cropped and resized to 512x512 (for 1.5) or 768x768 or 1024x1024 (for XL) and we couldn't easily get our hands on fixed size images easily while scrapping the data from the web. 

Although this method worked perfectly for one of our later projects (which was a SVG image generator) but wasn't good for Midjourney imitation goal we've got in our heads. So we just found another target, midjourney itself!

## Knowledge Distillation 

At that point, we've got a lot of good SD 1.5 tunes, and we'll discuss that in the next section. But we still needed one thing and that was _that goddamn midjourney aesthetic_ and at this time, one idea came to my mind. The idea was good enough for me to think about it multiple times and try to rewrite it and review it multiple times. So what was this __once in a lifetime idea__ I got in my head? 

Well, going to Midjourney, get an account and start making _picture-prompt pairs_. This is what has done by a lot of other startup companies and even big names such as _DeepSeek_. They did this on text, we've done it on images. Best collections and prompt combinations just collected. I personally went after a lot of tutorials on Midjourney's prompt engineering and I had my prompts ready. 

After a few days of getting ready, I just got myself into the midjourney and got myself an account (which was $25/mo at the time) and started prompting. I collected images and wrote scripts in order to do resizing and renaming the files in prompt-picture pairs as intended. And finally, using Dreambooth, we managed to have a pretty good model. But there was a big problem which we needed to solve. Although it got pretty close to Midjourney we still had some problems identifying the secret behind Midjourney. 

At this time I personally wanted to give up on it and go with the mediocre model I got and run platform on it. Fortunately, I went on full forensics mode and started gathering information on how midjourney works under the hood. 

## Midjourney had a secret.

Midjourney is closed source and even if they have used open models such as SD, they're not giving up their under the hood models. But as far as I could find from people's assumptions on reddit or other internet forums, they basically had multiple models (and they have two separate confirmed models, one is main one and the other is _Niji_ which generates anime content) and based on your prompts, it decides to go with which one. 

So at this point, I found out that fine tune isn't enough and started another round of development which was merging the model with good checkpoints with good licensing (like Apache 2.0) we could find on the internet. This is how it went for a long time. Most of the models tuned with _Dreambooth_ technique have a _trigger word_ and it was enough to develop a detector (and later using a local LLM) to find which trigger word is better for the prompt. 

After a few months of training and working on the models, we found out something. LoRA's are now something we can use on `diffusers` library and even [AUTOMATIC1111](https://github.com/automatic1111) UI can load them perfectly. So using a LoRA became our number one priority. We had a base model and a repository of LoRAs. 

For now, since we're working on FLUX based models (which are accessible through our APIs since the platform is taken down) the base aesthetic is good but we still work on multiple LoRAs to add more to the base model. This is how we're going on and we've got plans for the future when we may restart as a generative AI platform. For now, since we have a lot of problems to face we decided to keep things as smooth as possible. 

## Features for the future

If you want to compete in this field in 2026 you have to provide something more than _yet another midjourney wannabe_ and honestly that makes everything much more difficult than the past few years. Your models or tools must have image-to-image abilities similar to Nano Banana or something similar. What is the answer then? Also a lot of people are mad about Qwen Image and models like that which are open but despite being open, are impossible to run on consumer machines as well. 

So the future plan of ours is simply _an open source small model which has image to image abilities_ in order to make it local and again, we will be able to run it on cheap infrastructure. This is how we planned the future and we hope this future comes as soon as possible. 

## Conclusion