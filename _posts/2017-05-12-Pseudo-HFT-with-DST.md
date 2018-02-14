---
published: true
layout: post
title: Pseudo HFT with DST
category: Trading
tags:
  - HFT
  - Dempster-Shafer
  - 'C#'
  - Interactive_Brokers
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
information to obtain a competitive advantage in entering/exiting positions in the market over other partcipants. Each mass function encapsulates data from a parameter ( Event based Moving Average, full book Level 2 data). Mass functions are then combined using Depster’s rule of combination to provide a information about the market, it's participants actions, and help informed buy/sell and sizing decisions. 

## Early Prototype of the code to create the Mass Functions  

'''r

m1<-c()   
    
m1[0]<-0

x<-seq(-10,10,by=.10)

for(i in 1:11){integrand<-function(x){dlogis(x,2,param_m1)}

m1[i]  <- integrate(integrand,lower = 2*i -13, upper = 2*i - 11)

m1[12] <- 1- do.call(sum,m1[1:11]) 

'''

										Click here for the [R(prototype)](https://github.com/blckswmngbrd/Dempster-Shafer-Trading-Algo/blob/master/DST_Early_Prototype.r) and [C#(working demo)](https://github.com/blckswmngbrd/Dempster-Shafer-Trading-Algo/tree/master/.vs/WindowsFormsApplication3) code 
                                        

## The Strategies Major Elements  

• Event based automated trading strategy

• Coded in C# with minimal usage of third party packages 

• User/Subject Matter Experts select parameters used to generate mass functions.

• Mass function 1: market microstructure(spread, order book depth, queue, short interest, etc.)

Elements of Market Depth evaluated: 
Bid Ask Depth and Thickness Spread > Average Spread Bid Price 
Ask Size Improvement vs. Average Ask Size Improvement 
Bid Size Improvement  vs. Bid Ask Size Improvement
etc..

• Mass function 2: Event based Moving Average (EMA) shrinkage and crossovers

• Assigns weights solely on what information is available. Not hindered by missing information.

• Utilize's Interactive Brokers TWS C# API

• Fully functional GUI

• Estimated refresh rate: 5-15 seconds


### Trade Flow 

Below is a detailed diagram that illustrates the logic of the DST Market Mind system: 



![Tradeflow.png]({{site.baseurl}}/_posts/Tradeflow.png)





## Ongoing project. Look for future updates.