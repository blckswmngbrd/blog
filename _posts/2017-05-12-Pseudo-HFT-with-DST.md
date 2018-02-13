---
published: false
layout: post
title: Pseudo HFT with DST
---

## Trading 10 to 20 second timeframes with the Dempster-Shaffer Theory and Interactive Brokers

Affectionately known as the DST Market Mind, a "High Frequency" style trading algo based on the Dempster-Shafer fusion theory in C# using the Interactive Brokers API, was concieved at the Illinois Institute of Technology-Stuart School of Business by Randall Campbell and Francisco Hernandez.  

## Understanding The Dempster-Shaffer Theory 

Developed by Glenn Shafer into a general framework for modeling epistemic uncertainty—a mathematical theory of evidence.

The theory allows one to combine evidence from different sources and arrive at a degree of belief
(represented by a mathematical object called belief function) that takes into account all the available evidence. 


![DempsterEquation.png]({{site.baseurl}}/_posts/DempsterEquation.png)


[(wiki)](https://en.wikipedia.org/wiki/Dempster%E2%80%93Shafer_theory)

The goal is to used the Dempter-Shafer Theory (DST) to combine the data from multiple sources of
information to obtain a competitive advantage in entering/exiting positions in the market over other
traders. The strategy uses a mass function that encapsulates data from an exponential moving average
(EMA) and a mass function encapsulating data from bid/ask and bid/ask sizes. We combine the mass
functions of these two sources of information using Depster’s rule of combination to obtain a better idea
of what our position should be than just using either of them individually.

• Event based automated trading strategy

• Use's Dempster’s rule of combination to fuse information from multiple independent sources

• User/Subject Matter Experts select parameters used to generate mass functions.

• Mass function 1: market microstructure(spread, order book depth, queue, short interest, etc.)

Elements of Market Depth evaluated: Bid Ask Depth and Thickness Spread > Average Spread Bid Price Improvement Degree in which the Bid price improves Ask Price Improvement Degree in which the Ask price improves Ask Size Improvement vs. Average Ask Size Improvement Bid Size Improvement  vs. Bid Ask Size Improvement

• Mass function 2: Event based Moving Average (EMA) shrinkage and crossovers

• Assigns weights solely on what information is available. Not hindered by missing information.

• Utilize's Interactive Brokers TWS C# API

• Fully functional GUI

• Estimated refresh rate: 5-15 seconds

Developed by

Randall Campbell and Francisco Hernandez,Phd. @ The Illinois Institute of Technology - Stuart School of Business
