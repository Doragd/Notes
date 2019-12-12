# One AC a Day Keep the Failure Away
!!! failure 人的一切痛苦,本质上都是对自己无能的愤怒
    你看看你！因为不坚持刷算法题失去了多少机会！
    没能够去想去的地方！没能够拿到喜欢的实习！失败！自卑！不敢尝试！觉得自己没有准备好！一次次退缩！都是因为你的懒惰！畏惧！不能坚持！

!!! tip
    * 悟已往之不谏，知来者之可追
    * 驽马十驾，功在不舍
    * 贵有恒,何须三更起五更眠; 最无益,只怕一日曝十日寒。

!!! example 要求
    * 每天至少AC一道题，不求多！！
    * 每周复习一次本周的题目

!!! success 复习记录
    

## Array
### [27. Remove Element](https://leetcode.com/problems/remove-element/)

#### 分析

就地删除一个数组中和给定值相同的数字，并返回新的数组的长度。关键是就地！可以联想到**数组就地删除的话，一般是用后面的来覆盖前面的。** 需要一个变量用来计数，然后遍历原数组，如果当前的值和给定值不同，就把当前值覆盖计数变量的位置，并将计数变量加1。

#### 代码
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int cnt = 0;
        for(int i=0;i<nums.size();i++){
            if(nums[i] != val) nums[cnt++] = nums[i];
        }
        return cnt;
    }
};
```

#### 复杂度
空间: $O(1)$ 时间: $O(n)$

