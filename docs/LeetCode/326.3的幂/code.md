# [326. 3的幂](https://leetcode-cn.com/problems/power-of-three/)

## 方法一：判断是否可以除尽3

思路：不断除以3，最后如果余数为0说明是3的幂次方



## 方法二：将n转换为3进制后只有1个1

思路：将n转换为3进制之后，1在开头并且后面全为0



## 方法三：打表法，3的幂在int32范围内只有19个

思路：将19个范围内的3的幂列出来，直接放在map中，如果是对应的数字直接返回true，否则返回false



## 方法四：参考leetcode官方题解最后一种解法

思路：因为3是质数，且3^19是位于范围内的最大的3的幂，因此如果n>0且3^19%n==0则一定是3的幂


> 执行用时：32 ms, 在所有 Go 提交中击败了66.67%的用户
> 		内存消耗：6.1 ms, 在所有 Go 提交中击败了32.61%的用户

```go
func isPowerOfThree(n int) bool {
	return n > 0 && int(math.Pow(3.0, 19.0)) % n == 0
}
```

