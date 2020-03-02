## One AC a Day Keep the Failure Away

!!! failure "人的一切痛苦,本质上都是对自己无能的愤怒"
    你看看你！因为不坚持刷算法题失去了多少机会！
    没能够去想去的地方！没能够拿到喜欢的实习！失败！自卑！不敢尝试！觉得自己没有准备好！一次次退缩！都是因为你的懒惰！畏惧！不能坚持！

!!! tip
    * 悟已往之不谏，知来者之可追
    * 驽马十驾，功在不舍
    * 贵有恒,何须三更起五更眠; 最无益,只怕一日曝十日寒。

!!! example "要求"
    * 每天至少AC一道题，不求多！！
    * 每周复习一次本周的题目
    * 不要死磕一道题！要学会不求甚解！！等到做题多了，再去总结共性！
    * 先掌握一种最容易理解的解法再说

!!! success "复习记录"
    

## Array
### [27. Remove Element](https://leetcode.com/problems/remove-element/)

!!! note "题意"
就地删除数组中和给定值相同的数字，返回新数组的长度。

!!! note "分析"
用后面的数来覆盖前面的数，可以达到就地删除目的。用指针$i$表示当前扫描到的位置，用指针$j$表示待覆盖的位置。首先我们手动算出最后的结果，看看待覆盖位置和扫描位置是怎么对应的。关键是想清楚指针怎么移动来实现这种对应关系。一开始两个指针都从0开始移动，当`nums[i] == val`时，$j$找到了待覆盖的位置不再移动，$i$继续移动，直到`nums[i] != val`, 此时执行覆盖`nums[j] = nums[i]` , 然后`j++` 。实际上，在还没找到待覆盖位置前，`i`和`j`都是同时移动的，`nums[j]=nums[i]`覆盖操作可以统一，不影响。

!!! note "代码"
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
!!! note "复杂度"
空间: $O(1)$ 时间: $O(n)$

### [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

!!! note "题意"
给定一个有序的数组就地删除重复项，使得每个数字最多出现1次，返回新数组长度

!!! note "分析"
强调有序是因为只有有序，重复项才可能集中在一起，而不是分散起来。快慢指针法：最开始时两个指针都指向第一个数字，如果两个指针指的数字相同，则快指针向前走一步，如果不同，则两个指针都向前走一步，(此时注意执行覆盖，手动模拟可以知道是慢指针后面一个被快指针覆盖，然后在两个指针+1，这个操作可以被统一成`nums[++j]=nums[i++]`)，这样当快指针走完整个数组后，慢指针当前的坐标加1就是数组中不同数字的个数。

!!! note "代码"
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 0, j = 0, n = nums.size();
        while(i<n){
            if(nums[i] == nums[j]) i++;
            else nums[++j] = nums[i++];
        }
        return nums.empty() ? 0:(j+1);
    }
};
```

!!! note "复杂度"
空间: $O(1)$ 时间: $O(n)$

### [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

!!! note "题意"

给定一个有序的数组就地删除重复项，使得每个数字最多出现2次，返回新数组长度

!!! note "分析"

设置一个`cnt` 表示最多还可以重复多少次，这里初始化`cnt=1`。如果出现一次重复(`nums[i] == nums[j]`)，则`cnt--`，此时`cnt==0`。下次再重复时，快指针就向前一步。遇到不重复的情况，因为数组是有序的，所以一定没有重复数了，`cnt`再次恢复为1。需要注意两点：1.初始化`j=0,i=1` 2. 无论`nums[i]`和`nums[j]`是否相等，我们都要移动两个指针，那移动两个指针时，为了对两个不等的情况进行处理，所以还要加赋值操作，`nums[++i]=nums[j++]`。这个操作是对相等的情况是不影响的，可以统一。

!!! note "代码"

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int j = 0, i = 1, cnt = 1, n = nums.size();
        while(i<n){
            if(nums[i] == nums[j] && cnt == 0) i++; //用掉一次机会了，还出现相等就快指针移动
            else{ //还有机会
                if(nums[i] == nums[j]) cnt--;
                else cnt = 1; //出现不相等的，由于是有序，不可能后面有重复的了，现在是判断新的数字了，所以恢复
                nums[++j] = nums[i++];
            }
        }
        return nums.empty() ? 0:(j+1);
    }
};
```

!!! note "复杂度"

空间: $O(1)$ 时间: $O(n)$

### [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

!!! note "题意"

就地旋转一个数组$k$次

!!! note "分析"

很容易想到的是利用同余的思想，用取模来求得第$i$个元素旋转$k$次后的位置。关键问题是如何实现就地，也就是不利用其他空间。
最好画图分析下，如下图所示
![分析](../images/leetcode-189.jpg)

!!! note "代码"

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size();  //注意不要忘记这步，因为k可能大于size
        int cnt = 0;
        for(int i=0; cnt < nums.size(); i++){ //学习一下这种写法!没想到的!
            int j = i;
            int s = nums[i];
            do{
                j = (j + k) % nums.size();
                int t = nums[j]; //先保存target的值
                nums[j] = s; //再赋值为新的
                s = t; //然后再更新source值为原来位置上的target值
                cnt++; //标记已处理的元素个数，不能超过nums.size
            }while(j != i);  //直到回到原点
        }
    }
};
```

!!! note "复杂度"

空间: $O(1)$ 时间: $O(n)$

### [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

!!! note "题意"
就地寻找乱序数组中未出现的最小正数

!!! note "分析"
手动模拟：对数组`[3,4,-1,1]`来说, 如果长度为$n=4$的数组不缺失正数的话，应该为`[1,2,3,4]` , 即`nums[i]==i+1`，此时应该返回`n+1=5`。如果把`[3,4,-1,1]` 中的元素移动到它应该在位置，即`[1,-1,3,4]` 可以很方便看出缺失的正数是`2` , 因为在此处`nums[i] != i+1`。

所以可以从头到尾扫描数组, 把正数调整到正确的位置, 即`nums[i] == nums[nums[i]-1]`。再扫描数组，找到第一个`nums[i] != i+1`, 返回`i+1`, 否则返回`n+1`。

!!! note "代码"
```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for(int i=0;i<n;i++){
            while(nums[i]>0 && nums[i]<=n && nums[nums[i]-1] != nums[i]){
                swap(nums[i], nums[nums[i]-1]);
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i] != i+1){
                return i+1;
            }
        }
        return n+1;
    }
};
```

!!! note "复杂度"
时间复杂度$O(n)$, 空间复杂度$O(1)$

### [299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/)

!!! note "题意"
有`secret`和`guess`两个数字字符串，统计`secret`中数字和位置都与`guess`相对应的数字个数`ansA`，以及`secret`中仅数字在`guess`存在但位置不同的数字个数`ansB`。

!!! note "分析"
从头到尾扫描`guess`，如果`secret[i]==guess[i]`, 则`ansA++`, 如果不同，则统计`guess`每个数字出现的次数。再扫描一次`secret`, 如果两个位置对应的数字不相同，而且`guess`中出现次数大于0的话，则`ansB++`。

还有一种别人的做法是如果不同的情况，分别统计`secret`和`guess`的数字出现次数。然后每个数字在两个字符串中的次数取最小值(因为没出现则为0，出现了则肯定是较小那个)再求和即为`ansB`。

!!! note "代码"
```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        vector<int> cnt(10);
        int ansA = 0, ansB = 0;
        for(int i=0;i<guess.length();i++){
            if(secret[i]==guess[i]){
                ansA++;
            }else{
                cnt[guess[i]-'0']++;
            }
        }
        
        for(int i=0;i<secret.length();i++){
            if(secret[i]!=guess[i] && cnt[secret[i]-'0']){
                ansB++;
                cnt[secret[i]-'0']--;
            }
        }
        return std::to_string(ansA) + "A" + std::to_string(ansB) + "B";
    }
};

```

!!! note "复杂度"
时间复杂度$O(n)$, 空间复杂度$O(1)$

### [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle)

!!! note "题意"
帕斯卡三角(杨辉三角)

!!! note "分析"
递推`tmp[j] = res[i-1][j-1] + res[i-1][j]`

!!! note "代码"
```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int> > res;
        for(int i=0; i<numRows; i++){
            vector<int> tmp(i+1);
            tmp[0] = tmp[tmp.size()-1] = 1;
            for(int j=1; j<tmp.size()-1; j++){
                tmp[j] = res[i-1][j-1] + res[i-1][j];
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```

### [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

!!! note "题意"
帕斯卡三角(杨辉三角)，要求空间 $O(k), k \leq 33$

!!! note "分析"
优化递推公式`tmp[j] = res[i-1][j-1] + res[i-1][j]`

采用滚动数组的技巧 `res[j] = res[j-1] + res[j]` 优化成一维

注意画图！看看值是怎么被覆盖的。

注意为了避免覆盖，要从后往前递推！！(我一开始做的时候是用了多余变量来保存之前的值，比较不优雅)

!!! note "代码"
```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> A(rowIndex+1, 0);
        A[0] = 1;
        for(int i=1; i<rowIndex+1; i++)
            for(int j=i; j>=1; j--)
                A[j] += A[j-1];
        return A;
    }
};

class Solution {
public:
    vector<int> getRow(int rowIndex) { 
        vector<int> res(rowIndex + 1);
        res[0] = 1;
        if(rowIndex > 0){
            res[0] = res[1] = 1;
        }
        for(int i=2; i<=rowIndex; i++){
            int last = res[0];
            for(int j=1; j<rowIndex; j++){
                int tmp = res[j];
                res[j] += last;
                last = tmp;
            }
            res[rowIndex] = 1;
        }
        return res;
    }
};
```



### [134. Gas Station](https://leetcode.com/problems/gas-station/)

!!! note "题意"

给N个加油站的加油量和从这个加油站到下一个的损耗，问是否存在一个起始位置，能够顺时针走完所有加油站

!!! note "分析"

* 我的思路：记录经过每个加油站的剩余量，并求和。小于0则一定没有解。大于0的话，一定有唯一解。从后往前遍历，用`sum`记录到当前位置的剩余量之和，最大的`sum`即为要找的位置。因为这个位置是顺时针走到尽头所储备的起始油量最多的点。
* 其他人的思路：本质一样，但更加直接，比我简洁多了TUT，因为其实可以正向遍历的。从前往后，记录剩余量并求和，如果小于0，则更新起始点位置为下一个加油点位置。走完一圈后，将当前油量与之前走过消耗的油量求和，小于0则无解，否则返回起始点位置。


!!! note "代码"

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        const int n = gas.size();
        int tot = 0, maxid = n-1, tmpsum = 0, maxsum = 0;
        for(int i=n-1;i>=0;i--){
            int res = (gas[i] - cost[i]);
            tot += res;
            if(i == n-1){
                maxsum = tmpsum = res;
            }else{
                tmpsum += res;
                if(tmpsum >= 0 && tmpsum > maxsum){
                    maxsum = tmpsum;
                    maxid = i;
                }
            }
        }
        if(tot < 0) return -1;
        return maxid;
            
    } 
};
/*别人代码*/
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int start(0),total(0),tank(0);
        for(int i=0;i<gas.size();i++) if((tank=tank+gas[i]-cost[i])<0) {start=i+1;total+=tank;tank=0;}
        return (total+tank<0)? -1:start;
    }
}; 
```

!!! note "复杂度"
时间复杂度$O(n)$, 空间复杂度$O(1)$


### [217/219/220 Contains Duplicate](https://leetcode.com/problems/contains-duplicate-iii/)

!!! note "题意"

`Contains Duplicate`的三个问题

* 问题1：数组中是否存在两个数重复
* 问题2：数组中是否存在两个下标不同，下标最多相差$k$，但值相同的数
* 问题3：数组中是否存在两个下标不同，下标最多相差$t$，值最多相差$k$的数


!!! note "分析"

* 问题1: 排序就好, $O(nlogn)$
* 问题2：使用`unordered_map`可以让查找插入删除时间平均在$O(1)$, 总共$O(n)$。这里是为了记录值的下标
* 问题3：使用`pair`数组记录值的下标并排序 (因为用`map`的话，重复值会被覆盖)，然后暴力$O(n^2)$

!!! note "代码"

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        if(nums.empty()) return false;
        sort(nums.begin(), nums.end());
        for(int i=0; i<nums.size()-1; i++){
            if(nums[i] == nums[i+1]) return true;
        }
        return false;
    }
};

class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for(int i=0; i<nums.size(); i++){
            if(mp.find(nums[i]) == mp.end()){
                mp[nums[i]] = i;
            }else{
                if(i - mp[nums[i]] <= k){
                    return true;
                }else{
                    mp[nums[i]] = i;
                }
            }
        }
        return false;
    }
};

class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        vector<pair<int, int> > rec;
        for(int i=0; i<nums.size(); i++){
            rec.push_back(std::make_pair(nums[i], i));
        }
        sort(rec.begin(), rec.end());
        for(int i=0; i<rec.size(); i++){
            for(int j=i+1; j<rec.size(); j++){
                if(rec[j].first <= t + rec[i].first){
                    if(abs(rec[i].second - rec[j].second) <= k){
                        return true;
                    }
                }else{
                    break;
                }
            }
            
        }
        return false;
    }
};
```

### [169. Majority Element](https://leetcode.com/problems/majority-element/)

!!! note "题意"

求众数，即出现次数大于$\lfloor len /2 \rfloor$ 的数

!!! note "分析"

排序之后，一定在`nums[len/2]`位置取到

!!! note "代码"

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        const int idx = nums.size() / 2;
        return nums[idx];
    }

};
```

### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water)

!!! note "题意"

求容器最大的装水量

![problem.jpg](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

!!! note "分析"

实际应该是双指针，装水量等于两指针的距离`j-i`乘以两者对应高度的较小值`min(height[i], height[j])`

从两边向中间搜索，因为距离在减小，所以要保证最小的那个高度要尽量高，所以每次移动指针的时候，要保留较高的那个，移动较小的那个

!!! note "代码"

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int i = 0, j = height.size()-1;
        int res = 0;
        while(i<j){
            res = max(res, min(height[i],height[j])*(j-i));
            height[i] < height[j] ? i++:j--;
        }
        return res;
    }
};
```

### [274. H指数](https://leetcode-cn.com/problems/h-index/solution/)

!!! note "题意"

在数组中至少有`h`个数大于等于`h`，剩下`n-h`个数小于等于`h`, 求这样的`h`

!!! note "分析"

题目给了一个样例，`[3,0,6,1,5]`。一开始想的是从大到小排序来看：对6来说，至少有1个大于等于6，对5来说，至少有2个大于等于5，对3来说，有3个大于等于3，这样答案即为3。但是我这样的做法并没有考虑这个`h`是否在数组里出现。如果出现的话，那么当然是`citations[i] == i+1` 。
但是新的一个样例是`[6,5,2,1,0],h=2` 我这样的做法就行不通了。
题解给了很好的解释，即说明了这个值很可能不存在于数组，那这个时候怎么办。

<img src="https://pic.leetcode-cn.com/Figures/274_H_index.svg">

如图所示的情况说明了，我们其实需要去找的是`citations[i] > i`，那么第0篇到第$i$篇都至少有$i+1$次引用，那么实际的h就为$i+1$

比如说：$h=3$

|index|0|1|2|3|4|
|-----|-|-|-|-|-|
|value|6|5|3|1|0|
|`citations[i] > i` | true|true|true|false|false|

比如说：$h=2$

|index|0|1|2|3|4|
|-----|-|-|-|-|-|
|value|6|5|2|1|0|
|`citations[i] > i` | true|true|false|false|false|

在代码实现的时候，先排序，然后出现第一个`citations[j] <= j` 时，`j`即为答案，因为此时是刚才说的`i+1`

!!! note "代码"

```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end(), greater<int>());
        for (int i = 0; i < citations.size(); i++) {
            if (citations[i] <= i) return i;
        }
        return citations.size();
    }
};
```

!!! note "复杂度"

时间$O(nlogn)$, 空间$O(1)$








