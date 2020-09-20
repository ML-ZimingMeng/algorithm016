#### 方法一：dp

Javascript 代码：

```javascript
var nthUglyNumber = function(n) {
    const dp = new Array(n).fill(1);
    let a = 0, b = 0, c = 0;
    for (let i = 1; i < n; i++) {
        dp[i] = Math.min(dp[a] * 2, Math.min(dp[b] * 3, dp[c] * 5));
        if (dp[i] === dp[a] * 2) a++;
        if (dp[i] === dp[b] * 3) b++;
        if (dp[i] === dp[c] * 5) c++;
    }
    return dp[n - 1];
};
```

Python 代码：

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        dp = [1 for _ in range(n)]
        idx_a = 0
        idx_b = 0
        idx_c = 0
        for i in range(1,n):
            dp[i] = min(dp[idx_a] * 2, dp[idx_b] * 3, dp[idx_c] * 5)
            if dp[i] == dp[idx_a] * 2:
                idx_a += 1
            if dp[i] == dp[idx_b] * 3:
                idx_b += 1
            if dp[i] == dp[idx_c] * 5:
                dp[i] == dp[idx_c] * 5
                idx_c += 1
        return dp[-1]
```

#### 复杂度分析

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$