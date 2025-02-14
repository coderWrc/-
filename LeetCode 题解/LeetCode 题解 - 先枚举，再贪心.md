# 先枚举，再贪心
[1. 行相等的最少多米诺旋转](#行相等的最少多米诺旋转)  
[2. 拿出最少数目的魔法豆](#拿出最少数目的魔法豆)  


# [行相等的最少多米诺旋转](https://leetcode.cn/problems/minimum-domino-rotations-for-equal-row/description/)
[top](#先枚举再贪心)
```
class Solution 
{
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) 
    {
        auto min_rot = [&](int target) -> int 
        {
            int to_top = 0, to_bottom = 0;
            for (int i = 0; i < tops.size(); i++) 
            {
                int x = tops[i], y = bottoms[i];
                if (x != target && y != target) return INT_MAX;
                if (x != target) to_top++; // 把 y 旋转到 top
                if (y != target) to_bottom++; // 把 x 旋转到 bottom
            }
            return min(to_top, to_bottom);
        };
        int ans = min((min_rot(tops[0])), min_rot(bottoms[0]));
        return ans == INT_MAX ? -1 : ans;
    }
};
```
# [拿出最少数目的魔法豆](https://leetcode.cn/problems/removing-minimum-number-of-magic-beans/description/)
[top](#先枚举再贪心)  
码一：
```
class Solution 
{
public:
    long long minimumRemoval(vector<int>& beans) 
    {
        ranges::sort(beans);
        long long sum = 0, mx = 0;
        int n = beans.size();
        for (int i = 0; i < n; i++)
        {
            sum += beans[i];
            mx = max(mx, (long long) (n - i) * beans[i]);
        }
        return sum - mx;
    }
};
```
码二：
```
class Solution 
{
public:
    long long minimumRemoval(vector<int>& beans)
    {
        vector<int> cnt(1e5 + 1); // cnt[i] 表示有多少袋里面含 i 颗魔法豆
        // 魔法豆总数    剩余豆子的最大值    魔法豆袋数
        long long sum = 0, mx = 0, n = beans.size();
        for (int& x : beans)
        {
            cnt[x]++;
            sum += x;
        }
        // 从小到大遍历
        for (int i = 1; i < cnt.size(); i++)
        {
            if (cnt[i]) // 存在含 i 颗魔法豆的袋子
            {
                mx = max(mx, n * i);
                n -= cnt[i]; 
            }
        }
        return sum - mx;
    }
};
```
