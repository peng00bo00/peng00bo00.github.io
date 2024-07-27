---
layout: post
title: OMSCS-SIM课程笔记04-General Simulation Principles
date: 2022-02-08
description: 随机模拟的基本原则
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍随机模拟的基本原则。
{: .block-preface }


## Steps in a Simulation Study

现实中一个完整的仿真过程一般包括以下步骤：

<div align=center>
<img src="https://i.imgur.com/qlzcwBN.png" width="80%">
<img src="https://i.imgur.com/uLXKj1r.png" width="80%">
<img src="https://i.imgur.com/iRuSQRU.png" width="80%">
</div>

## Some Definitions

在本门课中我们会使用如下的定义：

<div align=center>
<img src="https://i.imgur.com/S192US3.png" width="80%">
<img src="https://i.imgur.com/YXAf08z.png" width="80%">
</div>

## Time-Advance Mechanisms

进行仿真时我们需要对时刻表进行模拟，模拟的方法包括使用固定的时间间隔或是根据event来推进时刻表。

<div align=center>
<img src="https://i.imgur.com/SwBLGrn.png" width="80%">
</div>

使用FEL来推进时刻表时需要注意相邻事件之间系统状态不能发生改变。

<div align=center>
<img src="https://i.imgur.com/IstYDL7.png" width="80%">
<img src="https://i.imgur.com/2enSNfs.png" width="80%">
<img src="https://i.imgur.com/NwMydm7.png" width="80%">
</div>

使用FEL进行排队问题仿真的案例如下：

<div align=center>
<img src="https://i.imgur.com/tKUPvB1.png" width="80%">
<img src="https://i.imgur.com/FKNeR2J.png" width="80%">
</div>

## Two Modeling Approaches

因此，基于事件驱动的模拟程序标准流程如下：

<div align=center>
<img src="https://i.imgur.com/yBQu7UC.png" width="80%">
<img src="https://i.imgur.com/esurWLJ.png" width="80%">
<img src="https://i.imgur.com/1YcU2P6.png" width="80%">
</div>

本课程主要会使用一些仿真软件通过process-interaction的方式进行模拟。在这种模式下我们主要关注每个entity的逻辑而由软件帮助我们完成具体的调度，这样会使仿真过程更加清晰。

<div align=center>
<img src="https://i.imgur.com/Diks0Pl.png" width="80%">
<img src="https://i.imgur.com/D15lMPP.png" width="80%">
</div>

## Simulation Languages

本节课最后介绍了一些常用的仿真环境。

<div align=center>
<img src="https://i.imgur.com/lDXQzhV.png" width="80%">
</div>

## Reference

- [General Simulation Principles](https://studio.edx.org/assets/courseware/v1/51e20b9c9b80a3ec03f2c490424627ee/asset-v1:GTx+ISYE6644x+2T2019+type@asset+block/Module04-GeneralPrinciples_180527.pdf)