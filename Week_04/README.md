### 第四周作业情况

本周习题部分均已完成，采用 JavaScript 和 Python 两种语言书写，并简要分析了作题思路。

以下是题目【使用二分查找，寻找一个半有序数组 [4, 5, 6, 7, 0, 1, 2] 中间无序的地方说明：同学们可以将自己的思路、代码写在第 4 周的学习总结中】总结。

### 总结

LeetCode 中有很多 **旋转数组** 的问题，即对一个 **有序数组** 在某一个节点进行旋转，然后返回特定元素的下标、或者判断是否存在某个值等。 

什么是旋转数组呢？比如对于数组 `[1,2,3,4,5,6]` ，如果在 `3` 处进行旋转，旋转后的数组就是 `[4,5,6,1,2,3]`。

为什么一定要进行二分查找呢？$O(n)$ 直接遍历难道不好吗？

个人认为旋转数组类题目的出现就是为了考查二分查找的方法（个人意见，因为没找到旋转数组的实际应用），所以用 $O(N)$ 解法草草 AC 之后，还是有必要再用二分查找来做一遍的。

而在使用二分查找的过程中，我们主要面临以下问题：

- 知道应该分情况讨论，但情况太多无法总结出清晰的脉络；
- 强行套用二分查找模板，却不会修改细节，最后只好看答案。

下面我将自己的一些体会分享给大家，希望你在看完本文之后，自己能够独立写出这类问题的解决方案。我们先来看一个具体的例子。

### 33. 搜索旋转排序数组

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 `O(log n)` 级别。

**示例 1:**
```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```
#### 思路分析

*1. 判断 mid 与 left 对应元素的大小关系，从而判断哪边是有序的。*

我们发现数组中必定有一半是有序的，如何判断哪一半是有序的呢？这里选取 left（选 right 也可以），如果 `nums[mid] > nums[left]`，则左边有序；反之右边有序。这里先不证明了，可以对照图仔细想一下。

![](https://imgkr.cn-bj.ufileos.com/5e5af3c1-ead7-4431-859b-0bcaab696ce7.png)

*2. 判断 mid 对应元素与 target 的大小关系。*

假设左边是有序的，我们可以在有序的一边画一条线，这条线可以认为是柱状图的顶点：

![](https://imgkr.cn-bj.ufileos.com/fac6bde5-5bcf-461e-a01b-4f38f54d9a56.png)

a.如果 `target > nums[mid]`，target 只会存在于这种情况：

![](https://imgkr.cn-bj.ufileos.com/0bb2c1cf-4263-4be4-bece-82cc5ccfeccc.png)

即 target 一定会存在于右半区间内，所以 `left = mid + 1`；

b.相反，如果 `target < nums[mid]`，target 会存在以下两种情况：

![](https://imgkr.cn-bj.ufileos.com/42dded9d-4847-4377-acbe-37efff61489e.png)

也就是说，target 小于 mid 对应元素的时候，可能只是比 mid 稍小一点，但是仍然比 left 大，这时 target 位于左半区间；

或者，target 比 mid 对应的元素小很多，甚至比 left 对应的元素都小，这时 target 就位于右半区间了。

c.最后还有一个小尾巴：如果 `target == nums[mid]` 怎么办呢？由于没有重复元素，这时一定是 left 和 mid 指向同一个元素导致的，这时 target 会在哪里呢？毫无疑问，当然是 mid 的右侧，因为 left（mid） 已经是最左边的元素了。

而以上情况我们是假设左半区间是有序的，对于右半区间，我们仍然需要进行以上讨论，道理是一样的。

#### 参考代码

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target: # 符合条件
                return mid
            if nums[mid] > nums[left]:  # 左边是有序的
                if nums[mid] < target:
                    left = mid + 1
                elif nums[mid] > target: 
                    if target >= nums[left]:
                        right = mid - 1
                    else:
                        left = mid + 1
            elif nums[mid] < nums[left]: # 右边是有序的
                if nums[mid] > target:
                    right = mid - 1
                else:
                    if target < nums[left]:
                        left = mid + 1
                    else:
                        right = mid - 1
            else:
                left += 1
        return -1
```

#### 复杂度分析

- 时间复杂度：$O(log N)$
- 空间复杂度：$O(1)$

小结一下，我们的代码实现过程如下：

![](https://imgkr.cn-bj.ufileos.com/a48b7c6e-f3ba-488f-ba33-21d974878c55.png)

这个图只是帮助我们熟悉一下代码的框架，如果你能结合这个框架理解前面的讲解中每一种情况的缘由，那么你碰到旋转数组类的问题之后，都按照这个思路来推理，基本上就可以自己写出二分的代码。

下面是 [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/) 的代码，你会发现代码几乎没有任何变化。

```python 
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) >> 1
            
            if nums[mid] == target:
                return True
            if nums[l] < nums[mid]: # 左边是有序的
                if nums[mid] < target:
                    l = mid + 1
                else:
                    if target >= nums[l]:
                        r = mid - 1
                    else:
                        l = mid + 1
            elif nums[l] > nums[mid]: # 右边是有序的
                if target < nums[mid]:
                    r = mid - 1
                else:
                    if target <= nums[r]:
                        l = mid + 1
                    else:
                        r = mid - 1
            else:
                l += 1
        return False
```
而类似 [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/) 这样找最值的问题，继续运用上面的思想就行了。

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        l, r = 0, len(nums) - 1
        res = float('inf')

        while l <= r:
            mid = (l + r) >> 1
            res = min(res, nums[mid])
            if nums[mid] < nums[l]: # 右边是有序的
                res = min(res, nums[mid])
                r = mid - 1
            else:  # 左边是有序的，说明旋转点在右边
                res = min(res, nums[l])
                l = mid + 1
        return res
```
最后，[面试题 10.03. 搜索旋转数组](https://leetcode-cn.com/problems/search-rotate-array-lcci/) 属于压轴题，是在有重复元素的旋转数组中寻找 target 的下标。

如果你不了解前面的思路，直接做这道题的话，这里面的分类讨论绝对会让你怀疑人生。

事实上，由于他要返回具体的 target 下标，相当于第一道题解除了没有重复元素这个限制，所以代码只需在第一道题的基础上添加几个判断就可以了。

```python
class Solution:
    def search(self, arr: List[int], target: int) -> int:
        s, e = 0, len(arr) - 1
        res = float('inf')
        while s <= e:
            m = (s + e) >> 1
            if arr[m] == target:
                res = min(res, m)
                e = m - 1
            else:
                if arr[s] < arr[m]: # 左边有序的
                    if arr[m] < target:
                        s = m + 1
                    else:
                        if arr[s] <= target:
                            e = m - 1
                        else:
                            s = m + 1
                elif arr[s] > arr[m]: # 右边是有序的
                    if arr[m] > target:
                        e = m - 1
                    else:
                        if arr[e] > target:
                            s = m + 1
                        elif arr[e] < target:
                            e = m - 1
                        else:
                            if arr[s] == target: # 左边也有，去左边找
                                res = min(res, s)
                                break
                            else:
                                s = m + 1
                else:
                    s += 1
        return -1 if res == float('inf') else res
```

### 总结

如何去分类讨论，这是我们要真正理解的地方，而不是单纯的抄代码，抠细节，掌握了思想之后，下次在遇到的时候现推理就可以，而不会轻易忘掉。