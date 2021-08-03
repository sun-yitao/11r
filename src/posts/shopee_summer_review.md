---
layout: post
title: Y2 Internship Review
excerpt: 'Having just concluded my SIP final presentation, I thought I ought to give a review of my past 3 months working as a software engineer at Shopee.'
date: 2021-08-03
tags:
  - post
  - tech
---

Having just concluded my SIP final presentation, I thought I ought to give a review of my past 3 months working as a software engineer at Shopee. I was assigned to the search sub-team of the Data Engineering Execution Production team, where our job is to optimise Shopee’s search algorithm.

The purpose of e-commerce platforms don’t simply provide a place to host merchant’s items. They also have to provide an efficient way for users to find what they want to buy, whether or not the user knows exactly what they want. However, search queries are generic and often do not capture all the product characteristics that a user may be looking for, for example: price range, brand and product features. This is why it may be useful to use the users’ clicks in the current session to provide additional information to the search algorithm so the user can find what he or she wants faster. The feature that I was working on was to insert 4 items similar to the user’s last clicked item into the next batch of items yet to be shown in the search ranking.

To enable this, an algorithm to calculate item-item similarity was required. I was tasked to first do a literature review on collaborative filtering (CF), recommendation systems and query relevance to present to the team. Afterwards, we decided on the Swing algorithm [1] proposed by Alibaba’s researchers in 2020 based on its empirical success. The Swing algorithm basically works by considering 2 users clicking on the same 2 items as a “Swing”, which provides a stronger signal compared to traditional CF that only considers a single user clicking 2 items. This however, comes with a performance penalty of O(N²) compared to the O(N) required in traditional CF. The image below describes the "brute force" approach to calculating Swing, but the actual implementation is different in order to parallelise using MapReduce since even 1 day of data is too large to run sequentially.

![swing](/posts/shopee_summer_review/swing_algo.png)
*Swing algorithm*

I worked on implementing the Swing algorithm using Hadoop MapReduce and Spark with one additional modification: a Swing score will only be computed if the 2 items clicked by 2 users are also in the same item category. This reduces computation and provides cleaner results. We had similar offline metrics with the original paper, and the qualitative evaluation from sampled data also looks good with very few bad cases. I productionised the data for live A/B testing and also wrote some further modifications for incrementing and decaying the Swing scores as new data gets added.

To sum up, it has been a rather meaningful internship at Shopee this summer, I learnt many new big data tools such as Hadoop, Hive and Spark and got a lot more familiar with Golang. I also realised that I really enjoyed working at the intersection of research and engineering, being able to constantly learn new things and get a chance to implement and deploy them to improve the current system is a great opportunity to have.

References:
<br>
[[1]](https://arxiv.org/abs/2010.05525) X. Yang, Y. Zhu, Y. Zhang, X. Wang, and Q. Yuan, ‘Large Scale Product Graph Construction for Recommendation in E-commerce’, ArXiv, 2020.
