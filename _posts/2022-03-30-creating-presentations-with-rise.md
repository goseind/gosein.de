---
layout: post
title:  "Creating presentations with RISE (Jupyter Notebook)"
date:   2022-03-30
categories: rise jupyter notebook python presentation
---

I recently did a lot of work in Jupyter Notebook. Jupyter Notebook offers great ways to convert your content to LaTex or PDF, but what if you want to hold a presentation with your Notebook without having to scroll through the entire notebook on your screen?

That's when I discovered [RISE](https://rise.readthedocs.io). With RISE you can turn your Juypter Notebook into a presentation without losing valuable content or having to copy things to a different tool.

Once you've installed RISE via `pip install RISE`, you can select a type for each cell in your notebook. With `slide` you can select the beginning of a new slide, then `subslide` or `fragment` are parts of the same slide. With type `skip` or `notes` you can hide cells and use them for other purposes. The full guide can be found here: https://rise.readthedocs.io/en/stable/usage.html#creating-a-slideshow.