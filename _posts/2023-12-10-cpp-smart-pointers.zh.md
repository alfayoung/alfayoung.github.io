---
title: 'C++ 智能指针学习'
date: 2023-12-10
permalink: /posts/2023/12/cpp-smart-pointers/
lang: zh
ref: cpp-smart-pointers
tags:
  - c++
---

考虑以下代码：
```c++
#include <iostream>
#include <memory>
using namespace std;

shared_ptr<int> p;

void setp(const int& b) {
	p = make_shared<int>(b);
}

int main() {
	int c = 10;
	setp(c);
	cout << *p << endl;
	c = 20;
	cout << *p << endl;
	return 0;
}
```

你能说出它的输出是什么吗？

```c++
#include <iostream>
#include <memory>
using namespace std;

shared_ptr<int> p;

void setp(const shared_ptr<int>& b) {
	p = b;
}

int main() {
	int c = 10;
	shared_ptr<int> c_ptr = make_shared<int>(c);
	setp(c_ptr);
	cout << *p << endl;
	*c_ptr = 20;
	cout << *p << endl;
	return 0;
}
```

答案：
第一段代码：
```
10
10
```

第二段代码：
```
10
20
```

为什么会产生区别呢？原来 `make_shared<T>(x)` 等价于以 $x$ 为初值构造一个指针，而不是构造一个绑定到 $x$ 上的指针。
