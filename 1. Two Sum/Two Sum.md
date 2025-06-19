# Problem 1: Two Sum 两数之和 (EASY)

---

给定一个整数数组nums和一个整数目标值target,请你在该数组中找出和为目标值target的那两个整数,并返回数组下标. 

可以假设每种输入只会对应一个答案,不能使用两次相同的元素. 

---

# 解题思路: 

## 1. Brute Force 枚举 

枚举数组中的每一个数x,然后在数组中寻找是否存在target - x. ⚠️: 要注意使用遍历整个数组的方式时,每一个位于x之前的元素都已经与x匹配过,不需要再进行匹配. 

```python
class Solution:
  def twoSum(self, nums: List[int], target: int) -> List[int]:
    n = len(nums)
    for i in range(n):
      for j in range(i + 1, n):
        if nums[i] + nums[j] == target:
          return [i, j]
    return []
```

## 2. Two-pass Hash 两次遍历哈希表 

首先把数组中的每个元素的值作为key, 对应的index作为value存入到哈希表里面. 这一步的目的是为了在查找补数(target - nums[i])的时候可以使用哈希表快速定位index.
对于数组nums中的每个元素nums[i], 我们计算补数complement = target - nums[i], 然后在哈希表里面对这个补数进行查找, 并且不是当前元素本身. 
⚠️: 不能使用同一个元素两次,所以要加入条件hashmap[complement] != i.

```python
class Solution:
  def twoSum(self, nums: List[int], target: int) -> List[int]:
    hashmap = {}
    for i in range(len(nums)):
      hashmap[nums[i]] = i
    for i in range(len(nums)):
      complement = target - nums[i]
      if complement in hashmap and hashmap[complement] != i:
        return [i, hashmap[complement]]
    return []
```

## 3. One-pass Hash 一次遍历哈希表

再进行遍历数组操作的同时, 对数组中的元素进行当前元素是否已之前某数配对能够得到目标值的判断. 如果可以就立即返回这两个数的索引. 

使用一个哈希表来记录已经遍历过的数和索引. 

遍历每一个元素nums[i]的时候: 

1. 计算该元素的补数complement = target - nums[i]
2. 检查这个补数是否已经在哈希表中存在:

   如果存在的话,就返回这个数的索引.

   如果不存在的话,把当前的元素和索引加入哈希表,供后续的查找使用.

```python
class Solution:
  def twoSum(self, nums: List[int], target: int) -> List[int]:
    hashmap = {}
    for i in range(len(nums)):
      complement = target - nums[i]
      if complement in hashmap:
        return [i, hashmap[complement]]
      hashmap[nums[i]] = i
    return []
```

