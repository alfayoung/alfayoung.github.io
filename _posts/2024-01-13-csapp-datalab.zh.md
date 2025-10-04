---
title: 'CSAPP-DataLab 题解'
date: 2024-01-13
permalink: /posts/2024/01/csapp-datalab/
lang: zh
ref: csapp-datalab
tags:
  - 二进制
  - 系统
---

- bitXor

异或操作可以看作是不进位加法，但是似乎对本问没有任何作用。

于是，还是只能按照异或的定义：按位，相同为 0，不同为 1

我们可以将上面的操作写作 `(x & ~y) | (~x | y)`，接下来要做的就是把或操作拆分成与和非操作。

考虑逻辑运算
$$
A \vee B = \neg \neg (A \vee B) = \neg (\neg A \wedge \neg B)
$$

于是，我们就可以用给定的运算符来获得

- isTmax

我们先考察最大值，即 `0x7fffffff` 有一个性质：`~x = x + 1`。

很遗憾，这不是充分条件，但好在有且只有一个反例 `0xffffffff`，左右两边都是 $0$，因此特判即可。

后续的部分已经完成，题解待补，先放代码。

```c
int bitXor(int x, int y) {
  return ~((~(x & ~y)) & (~(~x & y)));
}

int tmin(void) {
  return (1 << 31);
}

int isTmax(int x) {
  return !!(x + 1) & !(~x ^ (x + 1));
}

int allOddBits(int x) {
  return !(((x & 0xAA) & ((x >> 8) & 0xAA) & ((x >> 16) & 0xAA) & ((x >> 24) & 0xAA)) ^ 0xAA);
}

int negate(int x) {
  return ~x + 1;
}

int isAsciiDigit(int x) {
  return !(x >> 8) & !((x >> 4) ^ 0x03) & !(((x + 0x06) >> 4) ^ 0x03);
}

int conditional(int x, int y, int z) {
  int mask = !x;
  int sum = z + ~y + 1;
  mask = mask | (mask << 1);
  mask = mask | (mask << 2);
  mask = mask | (mask << 4);
  mask = mask | (mask << 8);
  mask = mask | (mask << 16);
  return (mask & sum) + y;
}

int isLessOrEqual(int x, int y) {
  int sgn_x = x >> 31 & 1, sgn_y = y >> 31 & 1;
  return (sgn_x & !sgn_y) | !((sgn_y & !sgn_x) | !((x + (~(y + 1) + 1)) >> 31 & 1));
}

int logicalNeg(int x) {
  int x1 = (x >> 16) | x;
  int x2 = (x1 >> 8) | x1;
  int x3 = (x2 >> 4) | x2;
  int x4 = (x3 >> 2) | x3;
  int x5 = (x4 >> 1) | x4;
  return (~x5 & 1);
}

int howManyBits(int x) {
  int ans = 0;

  x = (x >> 1) ^ x;

  ans = ans + ((!!(x >> 15)) << 4);

  ans = ans + ((!!(x >> (ans + 7))) << 3);

  ans = ans + ((!!(x >> (ans + 3))) << 2);

  ans = ans + ((!!(x >> (ans + 1))) << 1);

  ans = ans + (!!(x >> ans));

  return ans + 1;
}
```
