# LeetCode



### [807. 保持城市天际线](https://leetcode.cn/problems/max-increase-to-keep-city-skyline/)

```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int n = grid[0].size();
        int ans = 0;

        // 记录行和列的最大值
        vector<int> col(n, 0);
        vector<int> row(n, 0);
        // 遍历所给矩阵，寻找行和列的最大值
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                row[i] = std::max(row[i], grid[i][j]);
                col[j] = std::max(col[j], grid[i][j]);
            }
        }
        // 遍历所给矩阵，[i][j]建筑物的增长只要不超过row[i]和col[j]就行
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                ans += std::min((row[i] - grid[i][j]), (col[j] - grid[i][j]));
            }
        }

        return ans;
    }
};
```



### [535. TinyURL 的加密与解密](https://leetcode.cn/problems/encode-and-decode-tinyurl/)

```cpp
class Solution
{
private:
    // 短网址到长网址的映射
    unordered_map<string, string> dataBase;
    // 用于生成短网址的字母表
    string alfabet = "0123456789aAbBcCdDeEfFgGhHiIjJkKlLmMnNoOpPqQrRsStTuUvVwWxXyYzZ";

public:
    string encode(string longUrl)
    {
        // 打乱字母表
        std::mt19937 rd(std::random_device{}());
        std::shuffle(alfabet.begin(), alfabet.end(), rd);

        // 取字母表的前8个字符
        string key(alfabet.begin(), alfabet.begin() + 8);
        dataBase[key] = longUrl;
        return "http://tinyurl.com/" + key;
    }

    string decode(string shortUrl)
    {
        // 根据短网址查询长网址
        return dataBase[shortUrl.substr(shortUrl.rfind('/') + 1)];
    }
};
```





### [剑指 Offer II 004. 只出现一次的数字 ](https://leetcode.cn/problems/WGki4K/)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        std::unordered_map<int, int> mp;
        // 统计元素出现次数
        for(const auto& v : nums) {
            mp[v]++;
        }

        // 找到出现次数等于1的元素
        for(const auto& v : mp) {
            if(v.second == 1) {
                return v.first;
            }
        }
        return 0;
    }
};
```





### [剑指 Offer II 008. 和大于等于 target 的最短子数组](https://leetcode.cn/problems/2VG8Kg/)

```cpp
class Solution
{
public:
	int minSubArrayLen(int target, vector<int>& nums)
	{
		int ans = 100010, n = nums.size();
		int sum = 0;

        // 利用滑动窗口
		for (int left = 0, right = 0; right < n;)
		{
            // 先向右滑动，增大窗口
			while (sum < target && right < n)
			{
				sum += nums[right++];
			}

            // 如果一直到最后，都没有满足要求，直接返回0
			if (right == n && left == 0 && sum < target)
			{
				return 0;
			}

            // 如果找到一个符合要求的子数组
			if (right <= n)
			{
                // 向左滑动，直到不满足条件
				while (sum >= target)
				{
					sum -= nums[left++];
				}
                // 求子数组长度
				ans = std::min(ans, right - left + 1);
			}
		}

		return ans;
	}
};
```



### [剑指 Offer II 009. 乘积小于 K 的子数组](https://leetcode.cn/problems/ZVAVXX/)

```cpp
class Solution
{
public:
	int numSubarrayProductLessThanK(vector<int>& nums, int k)
	{
		if (k <= 1) return 0;

		int  ans = 0, sum = 1, n = nums.size();

		for (int left = 0, right = 0; right < n; ++right)
		{
			sum *= nums[right];

			while (left <= right && sum >= k)
			{
				sum /= nums[left++];
			}
			ans += right - left + 1;
		}

		return ans;
	}
};
```



### [剑指 Offer II 010. 和为 k 的子数组](https://leetcode.cn/problems/QTMn0o/)

```cpp
class Solution
{
public:
	int subarraySum(vector<int>& nums, int k)
	{
		unordered_map<int, int> hash;
		hash[0] = 1;
		int pre = 0, ans = 0;
		for (int i = 0; i < nums.size(); i++)
		{
			pre += nums[i];
			if (hash.find(pre - k) != hash.end()) {
                ans += hash[pre - k];
            }
            
			hash[pre] += 1;
		}
		return ans;
	}
};
```





### [8. 字符串转换整数 (atoi)](https://leetcode.cn/problems/string-to-integer-atoi/)

```cpp
class Solution {
public:
    int myAtoi(string s) {
	int i = 0;

	while (s[i] == ' ') i++;
    string str = s.substr(i);

    try
	{
		return std::stoi(str);
	}
	catch (const std::invalid_argument&)
	{
		return 0;
	}
    catch (const std::out_of_range&) {
        return str[0] == '-' ? -2147483648 : 2147483647;
    }

    }
};
```







## 映驰100题

### [LCP 24. 数字游戏](https://leetcode.cn/problems/5TxKeK/)

```cpp
class Solution
{
public:
	/*
	优先队列来找数据流的中位数操作：
		小顶堆的顶 > 大顶堆的顶
		每次优先加入小顶堆
		如果两边的大小差超过1时，小顶堆的顶弹出到大顶堆
	*/
	vector<int> numsGame(vector<int>& nums)
	{
		const int mod = 1e9 + 7;
		int n = nums.size();
		int64_t right_sum = 0;		// 保存小顶堆中的元素之和
		int64_t left_sum = 0;		// 保存大顶堆中的元素之和
		int64_t mid; // 中位数
		vector<int> ans(n, 0);
		priority_queue<int, vector<int>, std::greater<int>> right_small_heap;	// 小顶堆
		priority_queue<int, vector<int>, std::less<int>> left_big_heap;		// 大顶堆

		for (int i = 0; i < n; ++i)
		{
			// nums 减去一个递增序列，这里是优化后的写法，不然需要单独写一个for循环
			nums[i] -= i;
			// 如果nums[0:i]有奇数个元素
			if (i % 2 == 0)
			{
				// 先nums[i]放到右边的小顶堆
				right_small_heap.push(nums[i]);
				right_sum += nums[i];

				// 然后将右边小顶堆的最小元素放到左边的大顶堆
				left_big_heap.push(right_small_heap.top());
				left_sum += right_small_heap.top();
				right_sum -= right_small_heap.top();
				right_small_heap.pop();

				mid = left_big_heap.top();
				// 如果要使中位数左右两端的元素都和中位数相等，则需要操作次数为
				// left_big_heap.size() * mid - left_sum + right_sum - right_small_heap.size()*mid
				// 因为是奇数个元素，所以left_big_heap.size() - right_small_heap.size() = 1
				// 所以简化为 right_sum - left_sum + mid
				ans[i] = (right_sum - left_sum + mid + mod) % mod;
			}
			// 如果nums[0:i]有偶数个元素
			else
			{
				// 先将nums[i]放到左边的大顶堆
				left_big_heap.push(nums[i]);
				left_sum += nums[i];

				// 然后将左边大顶堆的最大元素放到右边的小顶堆
				right_small_heap.push(left_big_heap.top());
				right_sum += left_big_heap.top();
				left_sum -= left_big_heap.top();
				left_big_heap.pop();

				// 如果要使中位数左右两端的元素都和中位数相等，则需要操作次数为
				// left_big_heap.size() * mid - left_sum + right_sum - right_small_heap.size()*mid
				// 因为是偶数个元素，所以left_big_heap.size() - right_small_heap.size() = 0
				ans[i] = (right_sum - left_sum + mod) % mod;
			}
		}

		return ans;
	}
};
```

