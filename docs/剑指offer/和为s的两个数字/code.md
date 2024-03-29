# 和为s的两个数字

思路：给定了一个target，我们打印结束的条件肯定是找到target/2这个数字(也就是找到1到target中间的两个数字)，因为如果target为偶数，那么在target/2就结束了，并且如果target为奇数，也会在target/2结束，所以我们设置一个起始的值从1开始(因为是正数)，同时设置一个结束的值到2(因为连续)，并使用一个sum变量记录start到end之间的和。

步骤如下：

1. 如果sum > target，那么肯定大了，sum-=small，同时我们将start右移1个单位
2. 如果sum < target，那么值小了，我们需要将end右移一个单位，同时sum += end
3. 如果sum == target，满足条件，我们组成start到end的一个切片，并将其放入结果中。之后因为后面还可能有满足条件的记录，因此我们将big++，不断重复如上步骤，直到small <= (target + 1) / 2


## 剑指offer上的题解

### 错误代码
~~错误代码~~ 找到一个就会返回结果

```go
func findContinuousSequence(target int) [][]int {
	if target <= 0 {
		return nil
	}

	ret := [][]int{}

	//序列的起始值和结束值，以及序列的和
	startVal, endVal, sum := 1, target, (target+1)*target/2
	fmt.Println(startVal, endVal, sum)

	for startVal < endVal {
		//大于目标值, 去掉左边的值的元素
		if sum > target {
			startVal, sum = startVal+1, sum-startVal
			fmt.Println("缩小值到目标值：", startVal, endVal, sum)
		} else if sum < target { //小于目标值，加上后边的元素
			endVal, sum = endVal+1, sum+endVal
			fmt.Println("放大值到目标值：", startVal, endVal, sum)
		} else { //等于目标值
			// 将startVal到endVal所有元素全部加入
			temp := make([]int, endVal-startVal+1)
			for i, val := 0, startVal; val <= endVal; i, val = i+1, val+1 {
				temp[i] = val
			}
			ret = append(ret, temp)
			fmt.Println("值到目标值：", startVal, endVal, sum)
		}
	}

	return ret
}
```

### 自己参照剑指offer思路写的代码

```go
//参照剑指offer思路代码写的
func findContinuousSequence2(target int) [][]int {
	if target <= 1 {
		return nil
	}

	ret := [][]int{}

	//序列的起始值和结束值，以及序列的和
	startVal, endVal, sum := 1, 2, 3
	fmt.Println(startVal, endVal, sum)
	middleVal := (target + 1) >> 1
	for startVal <= middleVal {
		if sum == target {
			temp := make([]int, endVal-startVal+1)
			for i, val := 0, startVal; val <= endVal; i, val = i+1, val+1 {
				temp[i] = val
			}
			ret = append(ret, temp)
			endVal += 1
			sum += endVal
		} else if sum < target {
			// 将startVal到endVal所有元素全部加入
			endVal += 1
			sum += endVal
		} else {
			sum -= startVal
			startVal += 1
		}
	}
	return ret
}
```


## LeetCode上的题解
### 方法三：类似于两数之和，使用map

```go
//方法一：map
func twoSum(nums []int, target int) []int {

	hmap := make(map[int]int)

	for i := 0; i < len(nums); i++ {
		if index, ok := hmap[target-nums[i]]; ok {
			return []int{nums[index], nums[i]}
		}
		hmap[nums[i]] = i
	}

	return nil
}
```

### 【推荐】方法四：使用双指针

!> 题目参考：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/

!> 因为题目中说了数组是有序的

```go
//方法二：双指针
func twoSum(nums []int, target int) []int {
	l, r := 0, len(nums)-1
	for l < r{
		if nums[l] + nums[r] > target {
			r--
		} else if nums[l] + nums[r] < target {
			l++
		} else {
			return []int{nums[l], nums[r]}
		}
	}
	return nil
}
```

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int l = 0, r = nums.size()-1;

        while(l < r)
        {
            if (nums[l] + nums[r] == target) 
                return {nums[l], nums[r]};
            else if (nums[l] + nums[r] < target)
                l++;
            else
                r--;
        }
        return {};
    }
};
```