---
title: Derangement of An Array
tags: 
- Algorithm
categories: Algorithm
---
Suppose that there are n people who are numbered 1, 2, ..., n. Let there be n hats also numbered 1, 2, ..., n. We have to find the number of ways in which no one gets the hat having same number as their number. Let us assume that the first person takes hat i. There are n − 1 ways for the first person to make such a choice. There are now two possibilities, depending on whether or not person i takes hat 1 in return:
1.  Person i does not take the hat 1. This case is equivalent to solving the problem with n − 1 persons and n − 1 hats: each of the remaining n − 1 people has precisely 1 forbidden choice from among the remaining n − 1 hats (i's forbidden choice is hat 1).
2.  Person i takes the hat 1. Now the problem reduces to n − 2 persons and n − 2 hats.

```
class Solution {
public:
	int findDerangement(int n) {
		int64_t mod = pow(10, 9) + 7;
		vector<int64_t> buf(n + 1, 0);
		buf[1] = 0, buf[2] = 1;
		for (int i = 3; i <= n; i++)
			buf[i] = ((int64_t)(i - 1) * (buf[i - 1] + buf[i - 2])) % mod;
		return (int)buf[n];
	}
};
```