---
layout: post
title:  "Reverse Engineering Challenge: Broken Password Safe"
date:   2022-03-29
categories: reverse engineering c challenge
---

In the past semester, I took a lecture on Reverse Engineering. The goal of Reverse Engineering is to understand as much as possible about a piece of software or system while knowing very little about it. So in most cases, you start with just an executable. Those pieces of compiled software cannot simply be read and understood and in this blog post, I want to talk about the different approaches that can be used to analyze such files. I’ll be using a simple Capture the Flag Challenge I developed for others to try, to walk you through the process.

Let’s start with the tools we need/can use:

* [Ghidra](https://github.com/NationalSecurityAgency/ghidra)
* [Radare2](https://github.com/radareorg/radare2)

make sure you have the appropriate Java JDK installed. Additonaly you may need a Hex Editor for some challenges, not for mine though.

Now let's start with our analysis. Note that maybe 