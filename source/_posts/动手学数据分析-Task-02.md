---
title: 动手学数据分析 Task-02
tags:
  - python
abbrlink: 74b1b7e6
date: 2021-09-15 13:14:36
category: 数据分析
---
## 第二章： 数据清洗及特征处理
### **第一节** 
#### **观察处理缺失值**

数据集中含有缺失值是很正常的，常见的几个对缺失值的处理方法有：
1. 忽略 
2. 删除
3. 填充  

在第一章中通过`.info()`方法观察到Age， Cabin，Embarked三列数据有缺失值，利用`isnull().sum()`统计一下三个特征中缺失值的数量得到  
* **`Age`中有`177`个缺失值，`Cabin`中有`687`个缺失值，`Embarked`中有`2`个缺失值**  

缺失值对数据的影响是比较大的所以不可以忽略，对于Age中的177个缺失值可以采用填充的方法。对于Cabin，一般当缺失值的数量大于50%的时候不会采取填充的方法，所以只能删除这个特征。对于Embarked来说，只有2个缺失值，采取填充的方法会更好一些。  
填充的数值一般有几个方法来确定<!--more-->