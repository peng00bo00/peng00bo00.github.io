---
layout: post
title: OMSCS-ML课程笔记05-Ensemble Learning
date: 2021-09-07
description: 集成学习
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍监督学习中集成学习的相关内容。
{: .block-preface }


我们之前介绍的机器学习算法都是使用单个模型在数据集上完成学习任务。当任务比较复杂时单个模型可能不会有非常理想的效果，而在**集成学习(ensemble learning)**中则是学习一系列的模型并把它们组合到一起共同完成机器学习的任务。每个单独的模型不需要有非常强的性能，但把它们结合到一起后就可以提升整体的学习效果。集成学习的典型算法包括bagging和boosting两类。

## Bagging

bagging是最简单的集成方法：在训练时每次只选取全部数据中的一部分子集进行训练，从而得到一系列的模型；而在进行预测时则是将数据输入到每个模型中并取它们输出的平均作为预测值。之所以只在数据集的子集上进行训练是的原因在于直接利用全部数据训练一个强大的模型是相对困难的，而只利用一部分数据训练一系列简单模型则是相对容易的。当我们用全部模型进行预测时每个模型会对其它的模型的缺陷进行补偿，从而提升整体模型的性能。bagging方法中最常用的是**随机森林(random forest)**算法，它通过训练大量的决策树来提升学习的效果。

## Boosting

bagging方法通过训练一系列相互独立的模型来提升性能，而boosting方法则是通过修改模型和数据的权重来提升性能。boosting方法在训练过程中会为每个模型赋予不同的权重，如果当前模型对某个样本分类错误则会提高该样本的权重，使得模型会更关注那些被错误分类的难分样本(hard example)；而在预测时对所有模型的输出计算加权平均。以二分类问题为例，boosting方法的训练流程如下：

1. 初始化样本的权重$$\mathbb{D}_1$$；
2. 利用当前的样本权重训练一个分类器$$h_t(x)$$；
3. 计算当前分类器在数据集上的误差$$\varepsilon_t$$，进而更新模型权重$$\alpha_t$$和样本权重$$\mathbb{D}_t$$；
4. 回到步骤2直到获得足够数量的分类器，最终的分类器为$$h_t(x)$$的线性组合：$$h(x) = \text{sign} \big( \sum_t \alpha_t h_t(x) \big)$$。

## AdaBoost

显然如何更新样本以及模型的权重对于boosting方法的效果起着至关重要的作用。boosting方法中最具代表性的是AdaBoost算法，它利用指数函数来自适应地更新权重，具体而言：

1. 初始化样本的权重为均匀权重$$\mathbb{D}_1(x_i) = \frac{1}{n}$$；
2. 利用当前的样本权重训练一个分类器$$h_t(x)$$；
3. 根据当前样本权重计算当前分类器在数据集上的加权误差：$$\varepsilon_t = \sum_{i=1}^n \mathbb{D}_t(x_i) I(h_t(x_i) \neq y_i)$$

4. 当前分类器的模型权重为：$$\alpha_t = \frac{1}{2} \log \frac{1 - \varepsilon_t}{\varepsilon_t}$$

5. 更新训练数据集上的样本权重：$$\mathbb{D}_{t+1}(x_i) = \frac{\mathbb{D}_{t}(x_i)}{Z_t} \exp \{-\alpha_t y_i h_t(x_i) \}$$; $$Z_t = \sum_{i=1}^n \mathbb{D}_{t}(x_i) \exp \{-\alpha_t y_i h_t(x_i) \}$$

6. 回到步骤2直到获得足够数量的分类器，最终的分类器为$$h_t(x)$$的线性组合：$$h(x) = \text{sign} \big( \sum_t \alpha_t h_t(x) \big)$$

结合AdaBoost算法的训练过程不难发现单个分类器的权重$$\alpha_t$$描述了该分类器在总体中的重要性，加权后分类误差$$\varepsilon_t$$越小分类器的的重要性就越大。同时在样本权重的更新中对于分类正确的样本其权重会以$$e^{-\alpha_t}$$进行衰减，而错误样本的权重则会以$$e^{\alpha_t}$$进行放大，这说明AdaBoost算法在不改变训练数据总量的情况下调整了数据的分布，使得每次训练单个分类器都会更加关注之前分类错误的样本。此外还可以证明AdaBoost算法的训练误差是以指数速率下降的，这表明AdaBoost算法的训练过程是非常高效的。

最后需要补充说明的是尽管boosting方法不太容易出现过拟合的问题，但仍然不能完全避免。如果基分类器出现了过拟合，那么组合之后的分类器一定会出现过拟合。同时如果数据集上存在粉红噪声(pink noise)，boosting相关的方法也容易出现过拟合的问题。

## Reference

- 第8章：提升方法，统计学习方法（第2版）
- [Wikipedia: Random forest](https://en.wikipedia.org/wiki/Random_forest)