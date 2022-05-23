---
title: "LeetCode Contest #294"
summary: "整理LeetCode第294次周赛的题目 6077. 巫师的总力量和"
date: 2022-05-23
tags: ["LeetCode"]
author: "Letian Jiang"
draft: false
katex: true
---

整理LeetCode第294次周赛的题目 [6077. 巫师的总力量和](https://leetcode.cn/problems/sum-of-total-strength-of-wizards/)

思路是使用单调栈来计算每个元素的作为最小值的区间范围，然后根据前缀和计算每个元素对于答案的贡献，本题的特别之处在于要使用二阶前缀和。



1. 一次遍历计算每个元素作为最小值的区间

```cpp
int n = nums.size();
vector<int> left(n, - 1), right(n, n);
stack<int> st;
for (int i = 0; i < n; i++) {
    while (!st.empty() && nums[st.top()] >= nums[i]) {
        int j = st.top();
        st.pop();
        right[j] = i;
    }
    if (!st.empty()) {
        left[i] = st.top();
    }
    st.push(i);
}

for (int i = 0; i < n; i++) {
	cout << left[i] + 1 << ' ' << right[i] - 1;
}
```



2. 计算二阶前缀和

每个元素 nums[i] 作为最小值的区间 [l, r] 内的所有子数组之和，可以用求和公式表达
$$\sum_{x=l}^i \sum_{y=i}^r sum(nums[x...y])$$

对这个公式进行推导
$$
\begin{aligned}
& \sum_{x=l}^i \sum_{y=i}^r sum(nums[x...y]) \\\\\\
= & \sum_{x=l}^i \sum_{y=i}^r psum[y] - psum[x-1] \\\\\\
= & (i - l + 1) \sum_{y=i}^r psum[y] - (r - i + 1) \sum_{x=l}^i psum[x - 1] \\\\\\
= & (i - l + 1) \sum_{y=i}^r psum[y] - (r - i + 1) \sum_{x=l-1}^{i-1} psum[x] \\\\\\
= & (i - l + 1) (ppsum[r] - ppsum[i-1]) - (r - i + 1) (ppsum[i - 1] - ppsum[l - 2])
\end{aligned}
$$
其中psum为nums的前缀和，ppsum为psum的前缀和

```cpp
int mod = 1e9 + 7;
vector<int> psum = strength;
for (int i = 1; i < n; i++) {
    psum[i] = (psum[i] + psum[i - 1]) % mod;
}

vector<int> ppsum = psum;
for (int i = 1; i < n; i++) {
    ppsum[i] = (ppsum[i] + ppsum[i - 1]) % mod;
}
```

另外通过前缀和计算区间和时，可以使用匿名函数来屏蔽关于下标越界的细节

```cpp
auto f = [&](int l, int r) {
    if (r < 0) return 0;
    if (l - 1 < 0) return ppsum[r];
    return (ppsum[r] - ppsum[l - 1] + mod) % mod;
};
```

最后对每个元素的贡献求和

```cpp
int ans = 0;
for (int i = 0; i < n; i++) {
    int l = left[i] + 1;
    int r = right[i] - 1;
    int sleft = (long long)(i - l + 1) * f(i, r) % mod;
    int sright = (long long)(r - i + 1) * f(l - 1, i - 1) % mod;
    long long cur = (sleft - sright + mod) % mod; 
    ans = (ans + cur * strength[i]) % mod;
}
return ans;
```

3. 类似的题目有：

* [907. 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)
* [1856. 子数组最小乘积的最大值](https://leetcode.cn/problems/maximum-subarray-min-product/)





