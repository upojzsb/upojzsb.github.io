---
layout:     post
title:      Exercises of CS:APP
subtitle:   Exercises for Computer Systems A Programmer's Perspective 3rd
date:       2024-07-29
author:     UPOJZSB
header-img: img/csapp3e-cover.jpg
catalog: true
tags:
    - Exercise
    - CSAPP
---

# Prologue

I bought *Computer Systems: A Programmer's Perspective* (CS:APP) 2e about 9 or 10 years ago but struggled to get through it. Recently, I purchased the 3rd edition of CS:APP and am now determined to read it. I believe that the best way to fully grasp the material is by working through the exercises. Therefore, I plan to tackle the exercises in CS:APP 3e to deepen my understanding of the book.

# Chapter 1

## Exercise 1.1

**Question:**

> Suppose you work as a truck driver, and you have been hired to carry a load of potatoes from Boise, Idaho, to Minneapolis, Minnesota, a total distance of 2,500 kilometers. You estimate you can average 100 km/hr driving within the speed limits, requiring a total of 25 hours for the trip
>> A. You hear on the news that Montana has just abolished its speed limit, which
constitutes 1,500 km of the trip. Your truck can travel at 150 km/hr. What
will be your speedup for the trip?
>
>> B. You can buy a new turbocharger for your truck at www.fasttrucks.com. They
stock a variety of models, but the faster you want to go, the more it will cost.
How fast must you travel through Montana to get an overall speedup for
your trip of 1.67×?

**Answer:**

*A:* Under this situation, the fraction of travelling distance in Montana would be $ \alpha = \frac{150\text{km}}{250\text{km}} = 0.6 $, and the improvement factor would be $ k = \frac{150\text{km/h}}{100\text{km/h}} = 1.5 $. Applying Amdahl's law, we can get: $ S = \frac{1}{(1-\alpha)+\alpha/k} = 1.25 $.

*B:* The fraction of travelling distance keeps unchange, which is $\alpha = 0.6$, and the total improvement factor is $ S = 1.67 $. So, we can get $ k $ by solving $ 1.67 = \frac{1}{(1-0.6)+0.6/k} $ and get $ k = 3 $. So the speed that spend in travelling Montana is $ v = 100km/h*3 = 300km/h $.

## Exercise 1.2

**Question:**

> A car manufacturing company has promised their customers that the next release of a new engine will show a 4× performance improvement. You have been assigned the task of delivering on that promise. You have determined that only 90% of the engine can be improved. How much (i.e., what value of k) would you need to improve this part to meet the overall performance target of the engine?

**Answer:**

In this question, $ S = 2 $ and $ \alpha = 0.8 $. By solving $ 2 = \frac{1}{(1-0.8)+0.8/k} $, the factor $ k $ would be $ 2.67 $.

# Chapter 2

# Chapter 3

# Chapter 4

# Chapter 5

# Chapter 6

# Chapter 7

# Chapter 8

# Chapter 9

# Chapter 10

# Chapter 11

# Chapter 12
