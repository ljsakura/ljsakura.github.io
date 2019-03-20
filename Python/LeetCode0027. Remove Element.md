给定指定数值val，删除list 中的val， 并返回新的list的长度  
与26题相似，这里考虑的同样是遍历list ，定义辅助参数length，当发现list中某个数值不等于val时，将该数值赋值给list[length-1]，同时将length的长度加1，这样就变相将所有不为val的数值都放在了最前，最终截取length长度的list为新list

```python
class Solution(object):
    def removeElement(self, nums, val):
        """
        :typenums: List[int]
        :typeval: int
        :rtype: int
        """
        if not nums:
            return0
        if val not in nums:
            print nums  # 注意这里，如果用return，则会报错，显示TypeError: range() integer end argument expected, got list，但如果换做print则不再报错
        length = 0
        for i in range(0,len(nums)):
            if nums[i] != val:
                length += 1
                nums[length-1] = nums[i]
        nums = nums[:length]
        return length
```
