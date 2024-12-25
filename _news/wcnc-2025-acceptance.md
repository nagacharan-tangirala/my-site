---
layout: post
title: Regular Article accepted at WCNC 2025
date: 2024-12-20 08:00:00
inline: false 
related_posts: false
---

The regular article titled **Optimizing Very Large Scale ITS Applications With Fast Fitness Evaluation** is accepted for presentation at the IEEE WCNC (Wireless Communications and Networking Conference) 2024 conference. This is my second article in the series of articles regarding my PhD topic. I have introduced my VANET simulator called [Disolv](https://github.com/nagacharan-tangirala/disolv) in my first article. This article utilizes the simulator to conduct an optimization study for a network planning. The goal is to highlight the benefits of Disolv in speeding up the optimization workflows by enabling fast fitness function evaluations.

Optimization workflows are commonly employed in infrastructure planning studies within the context of VANET scenarios. However, the scale of the studies are limited to smaller regions because of the expensive computation efforts required. Although the method validation is achieved by the authors using a small-scale scenario, what it fails to answer is if the method scales up well for a city-scale scenario. A solution to this is to incorporate high computing resources and conduct city-scale scenario evaluation. This may not be possible or accessible for every researcher. That is where Disolv can come for assistance. Instead of using an expensive simulator for large-scale scenarios, a simplified simulator like Disolv can be used. This allows for a large-scale evaluation without the need for HPC resources.

In this paper, we use RSU placement study carried out using a genetic algorithm. Each mutation is evaluated through a simulator run using Disolv. As a result, a single placement run can take a few thousands of evaluations. I compare the execution runs from Disolv with that of ns-3. The savings in time can be easily appreciated. With ns-3, the execution time is extrapolated linearly to reach around 278 days. Disolv takes just 10 hours. More details are in the article, and I will share them upon the presentation.
