---
title: '埃氏筛复杂度分析'
date: 2023-10-28
permalink: /posts/2023/10/eratosthenes-sieve/
lang: zh
ref: eratosthenes-sieve
tags:
  - 数学
  - 算法
---

埃氏筛代码：

````c++
bool vis[N];
void Eratosthenes() {
    for (int i = 2; i <= N; i++) {
        if (vis[i]) continue; // 合数就下一个
        for (int j = i; j <= N / i; j++) vis[i * j] = true;
    }
}
````

**约定：为了方便表述，规定本文之后出现的所有 $p$ 都是素数**

我们的目标是求出：
$$
\sum_{p\le \sqrt N} \left(\frac Np -p\right)
$$
上面的和式分为两部分，$\sum\limits_{p\le \sqrt N} \dfrac{N}{p}$ 与 $\sum\limits_{p\le \sqrt N}p$

下面的推导基于**素数定理**：设 $\pi (x)$ 表示不大于 $x$ 的数中有多少个素数，则 $\pi(x) \sim \dfrac{x}{\ln x}$

则我们知道 $x$ 为素数的概率为 $\pi(x) - \pi(x-1) \approx \dfrac 1{\ln x}$（这里实际上是期望，但由于只有一个数 $x$，因此期望等于概率），这样，只要乘上概率，我们就能将对 $p$ 的求和转化为连续的 $x$ 的求和了！

#### 先算第一部分

$$
\sum_{p\le \sqrt N} \frac Np =\sum_{x=2}^{\sqrt N} \frac{N}{x\ln x}=N\sum_{x=2}^{\sqrt{N}} \frac{1}{x\ln x}
$$

接下来就要求这个含有 $x$ 的和式，根据套路，我们可以使用**积分近似**。
$$
\sum_{x=2}^{\sqrt N} \frac1{x\ln x} \approx \int_{2}^ \sqrt N \frac{1}{x\ln x} \mathrm{d}x
$$
这是个经典的换元法解决的积分式。不妨设 $u = \ln x$，两边求导得 $\mathrm{d} u = \dfrac 1x \mathrm{d} x$，发现这一项正好出现在积分式中，直接代入：
$$
\int \frac 1{x\ln x} \mathrm{d} x=\int \frac 1u \mathrm{d}u= \ln u +C=\ln\ln x+C
$$
于是，将这个定积分代回到之前的式子中，第一部分的近似值为 $O(N\ln\ln \sqrt N) =O(N\ln\ln N)$

#### 再算第二部分

经过我的尝试，我使用**算两次**方法来解决这个问题。

第一种计算方式与第一部分的相同：
$$
\sum_{p\le \sqrt N} p = \sum_{x=2}^{\sqrt{N}} \frac x {\ln x}
$$
第二种计算方式是将每个 $p$ 拆分成 $\sum_x [1\le x\le p]$ 的形式，计算贡献（类似阿贝尔变换）：
$$
\begin{aligned}
\sum_{p\le \sqrt N}p &= \sum_{x=0}^{\sqrt N}\left(\pi(\sqrt N) - \pi(x)\right)= (\sqrt N + 1)\times  \pi (\sqrt N) - \sum_{x=2}^{\sqrt{N}}\pi(x)
\\&= (\sqrt N + 1)\times \frac {2\sqrt N}{\ln N} -\sum_{x=2}^{\sqrt N} \frac x{\ln x}
\end{aligned}
$$
两种计算方法联立，可以解得第二部分的近似值 $O(\dfrac N {\ln N})$
$$
\sum_{x = 2}^{\sqrt N} \frac x {\ln x} \approx \frac{N}{\ln N}
$$

#### 合并两部分

所以埃氏筛法的总复杂度为第一部分减去第二部分：
$$
O(N\ln\ln N-\frac N{\ln N}) = O(N\ln\ln N)
$$
得证！
