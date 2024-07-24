---
layout: post
title: OMSCS-CV课程笔记11-Tracking
date: 2021-10-19
description: 目标跟踪
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍目标跟踪的相关内容。
{: .block-preface }


## Introduction to Tracking

**目标跟踪(tracking)**是视频处理和分析的基本内容之一，我们希望能对视频中的物体运动进行跟踪，从而得到它的运动轨迹。目标跟踪的主要难点如下：

1. 在很多场景中无法计算光流；
2. 当物体运动很快时会产生比较大的位移；
3. 观测的误差容易发生累计；
4. 物体运动过程中存在遮挡的情况。

为了处理这些问题，一个基本的思路是考虑物体的**动力学模型(dynamic model)**。这样当物体发生运动时利用动力学模型可以对它可能出现的位置进行预测，从而缩小搜索范围并降低观测误差。在这种情况下目标跟踪和逐帧的检测是有一定差别的：目标检测是在每一帧图像上独立地进行检测(红色框)，相邻图像之间没有任何联系；而跟踪则需要根据动力学模型在前一帧的基础上估计物体的当前位置，然后结合当前帧的观测结果进行更新从而确定物体的实际位置(蓝色框)。

<div align=center>
<img src="https://i.imgur.com/GDDsNlQ.png" width="90%">
</div>

<div align=center>
<img src="https://i.imgur.com/lW0cqpC.png" width="90%">
</div>

因此，目标跟踪的目标是利用动力学模型来缩减搜索空间并结合观测结果来减小误差，进而得到光滑的物体运动轨迹。为了便于讨论我们这里对目标跟踪的场景进行一些限制：

1. 物体不会在场景中突然消失或是突然出现；
2. 相机的位置和姿态不会发生突变；
3. 物体和场景只会发生缓慢的运动。

## Parametric Models

### Tracking as Inference

当我们已知动力学模型的时候，目标跟踪可以建模成一个概率推断问题。假设系统的真实状态为$$X$$，观测到的状态为$$Y$$，在任意时刻$$t$$我们可以利用前一时刻的状态和动力学模型来计算系统的真实状态$$X_t = f(X_{t-1})$$。需要说明的是系统状态$$X$$往往是不可知的，因此它也被称为是隐状态(hidden state)。我们的目标是结合$$t$$时刻的观测来更新当前时刻的真实状态。从概率分布的角度上讲，这样的过程实际上就是利用先验概率(动力学模型)和似然函数(观测)来估计系统的后验概率(当前时刻真实状态)。

<div align=center>
<img src="https://i.imgur.com/cTZ5c3q.png" width="30%">
<img src="https://i.imgur.com/MZFyhof.png" width="61%">
</div>

因此，目标跟踪可以分为2步：

1. 利用过去时刻的观测值来估计系统当前状态，称为**预测(prediction)**；
2. 利用当前时刻的观测值来更新系统当前状态，称为**修正(correction)**。

预测和修正过程的数学形式为：

$$
P(X_t \vert Y_0 = y_0, \dots , Y_{t-1} = y_{t-1})
$$

$$
P(X_t \vert Y_0 = y_0, \dots , Y_{t-1} = y_{t-1}, Y_t = y_t)
$$

我们进一步对问题进行简化。假设动力学模型和观测模型满足一阶马尔科夫性，系统当前时刻的状态仅与前一时刻有关而且任意时刻的观测仅与该时刻的系统状态有关：

$$
P(X_t \vert X_0, \dots, X_{t-1}) = P(X_t \vert X_{t-1})
$$

$$
P(Y_t \vert X_0, Y_0, \dots, X_{t-1}, Y_{t-1}, X_t) = P(Y_t \vert X_t)
$$

此时系统在时间上的演化可以用下图所示的概率图来表示：

<div align=center>
<img src="https://i.imgur.com/jBWnEiR.png" width="50%">
</div>

这样我们就可以通过迭代的方式来对系统状态进行估计。具体而言，预测过程可以表示为已知$$P(X_{t-1} \vert y_0, \dots, y_{t-1})$$条件下估计$$P(X_t \vert y_0, \dots, y_{t-1})$$：

$$
\begin{aligned}
P(X_t \vert y_0, \dots, y_{t-1}) &= \int P(X_t, X_{t-1} \vert y_0, \dots, y_{t-1}) d X_{t-1} \\
&= \int P(X_t \vert X_{t-1}, y_0, \dots, y_{t-1}) P(X_{t-1} \vert y_0, \dots, y_{t-1}) d X_{t-1} d X_{t-1} \\
&= \int P(X_t \vert X_{t-1}) P(X_{t-1} \vert y_0, \dots, y_{t-1}) d X_{t-1}
\end{aligned}
$$

类似地，修正的过程则是利用$$P(X_t \vert y_0, \dots, y_{t-1})$$和$$y_t$$来计算$$P(X_t \vert y_0, \dots, y_t)$$：

$$
\begin{aligned}
P(X_t \vert y_0, \dots, y_t) &= \frac{P(y_t \vert X_t, y_0, \dots, y_{t-1}) P(X_t \vert y_0, \dots, y_{t-1})}{P(y_t \vert y_0, \dots, y_{t-1})} \\
&= \frac{P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1})}{P(y_t \vert y_0, \dots, y_{t-1})} \\
&= \frac{P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1})}{\int P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1}) d X_t} \\
&\propto P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1})
\end{aligned}
$$

这样我们就可以结合动力模型和观测模型来对系统状态进行估计：

<div align=center>
<img src="https://i.imgur.com/WdtLSBh.png" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/QJ8Zozi.png" width="62%">
</div>

### The Kalman Filter

对于线性高斯系统我们可以使用**Kalman滤波(Kalman filter)**来完成状态估计的任务。我们假设系统在$$t$$时刻的动力学模型为：

$$
X_t = D_t X_{t-1} + \varepsilon_{d_t}
$$

$$
\varepsilon_{d_t} \sim N(0, \Sigma_{d_t})
$$

观测模型为：

$$
y_t = M_t X_t + \varepsilon_{m_t}
$$

$$
\varepsilon_{m_t} \sim N(0, \Sigma_{m_t})
$$

以一维状态估计为例，系统的动力学模型和观测模型可以表示为：

$$
x_t = d \cdot x_{t-1} + \varepsilon_{d_t}
$$

$$
y_t = m \cdot x_t + \varepsilon_{m_t}
$$

其中$$\varepsilon_{d_t}$$和$$\varepsilon_{m_t}$$分别表示均值为0方差为$$\sigma_d^2$$和$$\sigma_m^2$$的高斯噪声。

系统在$$t$$时刻的状态服从正态分布$$X_t \sim N(\mu_t^-, (\sigma_t^-)^2)$$。利用动力学模型可以得到预测过程的状态更新公式：

$$
\mu_t^- = d \cdot \mu_{t-1}^+
$$

$$
(\sigma_t^-)^2 = \sigma_d^2 + (d \cdot \sigma_{t-1}^+)^2
$$

其中$$\mu_{t-1}^+$$和$$(\sigma_{t-1}^+)^2$$分别表示上一时刻系统状态的均值和方差。

利用观测方程，$$t$$时刻的观测量可以表示为$$Y_t \sim N(m \cdot x_t, \sigma_m^2)$$。结合修正过程的状态估计公式：

$$
P(X_t \vert y_0, \dots, y_t) 
= \frac{P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1})}{\int P(y_t \vert X_t) P(X_t \vert y_0, \dots, y_{t-1}) d X_t}
$$

可以得到修正过程的状态更新公式：

$$
\mu_t^+ = \frac{\mu_t^- \sigma_m^2 + m y_t (\sigma_t^-)^2}{\sigma_m^2 + m^2 (\sigma_t^-)^2}
$$

$$
(\sigma_t^+)^2 = \frac{\sigma_m^2 (\sigma_t^-)^2}{\sigma_m^2 + m^2 (\sigma_t^-)^2}
$$

从上式中不难发现更新后的均值和方差取决于预测模型的误差$$(\sigma_t^-)^2$$以及观测模型误差$$\sigma_m^2$$。当预测模型完全正确时($$(\sigma_t^-)^2 = 0$$)，系统更新后的状态等于预测阶段的状态；而当观测模型完全正确时($$\sigma_m^2 = 0$$)，系统更新后的状态等于观测到的状态。

记$$a = \frac{\sigma_m^2}{m^2}$$，$$b = (\sigma_t^-)^2$$。状态更新公式可以化简为：

$$
\begin{aligned}
\mu_t^+ &= \frac{\mu_t^- \sigma_m^2 + m y_t (\sigma_t^-)^2}{\sigma_m^2 + m^2 (\sigma_t^-)^2} \\
&= \frac{\frac{\mu_t^- \sigma_m^2}{m^2} + \frac{y_t (\sigma_t^-)^2}{m}}{\frac{\sigma_m^2}{m^2} + (\sigma_t^-)^2} \\
&= \frac{a \mu_t^- + b \frac{y_t}{m}}{a + b} \\
&= \frac{(a + b) \mu_t^- + b (\frac{y_t}{m} - \mu_t^-)}{a + b} \\
&= \mu_t^- + \frac{b}{a + b} (\frac{y_t}{m} - \mu_t^-) \\
&= \mu_t^- + k (\frac{y_t}{m} - \mu_t^-)
\end{aligned}
$$

上式说明系统真实状态最有可能的位置等于预测值$$\mu_t^-$$与残差$$(\frac{y_t}{m} - \mu_t^-)$$加权求和，加权系数$$k = \frac{b}{a + b}$$称为**Kalman增益(Kalman gain)**。

对于多维状态的情况，Kalman滤波的矩阵形式为：

- 预测过程：

$$
x_t^- = D_t x_{t-1}^+
$$

$$
\Sigma_t^- = D_t \Sigma_{t-1}^+ D_t^T + \Sigma_{d_t}
$$

- 修正过程：

$$
K_t = \Sigma_t^- M_t^T (M_t \Sigma_t^- M_t^T + \Sigma_{m_t})^{-1}
$$

$$
x_t^+ = x_t^- + K_t (y_t - M_t x_t^-)
$$

$$
\Sigma_t^+ = (I - K_t M_t) \Sigma_t^-
$$

迭代使用上面的几个公式即可完成对系统状态的估计。

<div align=center>
<img src="https://i.imgur.com/ZoFjxBY.png" width="70%">
</div>

Kalman滤波非常简洁而且高效，但需要注意的是它的使用前提是线性高斯系统。当系统的动力学模型不是线性方程或者噪声不满足正态分布假定时，使用Kalman滤波则不能得到正确的结果。

## Non-Parametric Models

### Bayes Filters

在实际应用中绝大多数的系统是非线性非高斯的，因此严格来说我们不能使用Kalman滤波来对系统状态进行估计。在这种情况下我们需要利用Bayes滤波来进行状态估计。假设系统状态的先验为$$p(x)$$，动力学模型为$$p(x_t \vert u_t, x_{t-1})$$，观测模型为$$p(z \vert x)$$，那么在给定观测和控制序列$$\{ u_1, z_2, ... \}$$的条件下我们希望对系统当前的状态$$x_t$$进行估计：

$$
P(x_t \vert u_1, z_2, ..., u_{t-1}, z_t)
$$

类似于Kalman滤波的假设，我们可以利用如下所示的概率图对系统进行建模：

<div align=center>
<img src="https://i.imgur.com/qF4GWO9.png" width="40%">
</div>

因此结合Bayes公式和马尔科夫性可以得到：

$$
\begin{aligned}
P(x_t \vert u_1, z_2, ..., u_{t-1}, z_t) &= \eta P(z_t \vert x_t, u_1, z_2, ..., u_{t-1}) P(x_t \vert u_1, z_2, ..., u_{t-1}) \\
&= \eta P(z_t \vert x_t) P(x_t \vert u_1, z_2, ..., u_{t-1}) \\
&= \eta P(z_t \vert x_t) \int P(x_t, x_{t-1} \vert u_1, z_2, ..., u_{t-1}) d x_{t-1} \\
&= \eta P(z_t \vert x_t) \int P(x_t \vert u_1, z_2, ..., u_{t-1}, x_{t-1}) P(x_{t-1} \vert u_1, z_2, ..., u_{t-1}) d x_{t-1} \\
&= \eta P(z_t \vert x_t) \int P(x_t \vert u_{t-1}, x_{t-1}) P(x_{t-1} \vert u_1, z_2, ..., u_{t-1}) d x_{t-1}
\end{aligned}
$$

记$$Bel(x_t) = P(x_t \vert u_1, z_2, ..., u_{t-1}, z_t)$$，我们可以整理得到递推公式：

$$
Bel(x_t) = \eta P(z_t \vert x_t) \int P(x_t \vert u_{t-1}, x_{t-1}) Bel(x_{t-1}) d x_{t-1}
$$

使用上式递推进行状态估计的算法称为Bayes滤波器。实际上Kalman滤波器正是线性高斯系统的Bayes滤波器，但对于非线性非高斯的系统则一般无法显式计算出Bayes滤波器的结果。

<div align=center>
<img src="https://i.imgur.com/DVZJBpr.png" width="60%">
</div>

### Particle Filters

**粒子滤波(particle filter)**是求解非线性非高斯系统的Bayes滤波器的经典方法，它的核心思想是利用大量的粒子(样本)和它们对应的权重(概率)来表示系统的状态分布。假设我们已经有$$t-1$$时刻系统的状态分布，首先需要从这个先验分布中采样出$$N$$个粒子，然后将它们输入到动力学模型中得到$$N$$个预测状态，并且利用观测值$$z_t$$来更新粒子的(未归一化)权重$$P(z_t \vert x_t)$$作为$$t$$时刻的系统状态。上述过程可以用下图来进行表示：

<div align=center>
<img src="https://i.imgur.com/jDz7UNC.png" width="60%">
</div>

粒子滤波在机器人定位中有经典的应用，在初始时刻我们可以将粒子均匀分布在地图上：

<div align=center>
<img src="https://i.imgur.com/R5ZiFmV.png" width="40%">
</div>

每当我们有了新的观测值时，利用粒子滤波来更新机器人当前的状态：

<div align=center>
<img src="https://i.imgur.com/mtkvb21.png" width="40%">
<img src="https://i.imgur.com/gYQDNDf.png" width="40%">
</div>

随着迭代的进行，机器人的位置会迅速收敛到真实的位置：

<div align=center>
<img src="https://i.imgur.com/fCHwDnY.png" width="40%">
<img src="https://i.imgur.com/9wpXNVy.png" width="40%">
</div>

在实际使用粒子滤波时还有很多技巧来提高算法的运行效率：

- 当粒子的权重分布比较均匀时我们可以省略掉重采样的过程；
- 如果某些粒子的权重明显大于其他粒子，我们可以在动力学模型和观测方程中提高噪声的比例来获得更光滑的估计结果；
- 我们可以引入系统状态的先验来加速粒子滤波收敛的过程，如使用Kalman滤波来初始化系统状态；
- 当粒子滤波运行失败时还需要一些重启算法的策略。

## Reference

- [Wikipedia: Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter)
- Chapter 3: Linear Gaussian Estimation, [State Estimation for Robotics](http://asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser17.pdf#page=53)
- Chapter 4: Nonlinear Non-Gaussian Estimation, [State Estimation for Robotics](http://asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser17.pdf#page=107)