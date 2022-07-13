# LeetCode101

## 两数之和

```cpp
class Solution {
 public:
  vector<int> twoSum(vector<int>& nums, int target) {
    int n = nums.size();
    std::unordered_map<int, int> has;

    // 一遍哈希表
    // 改进：在进行迭代并将元素插入到表中的同时，
    // 我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。
    // 如果它存在，那我们已经找到了对应解，并立即将其返回。
    for (int i = 0; i < n; ++i) {
      if (has.find(target - nums[i]) != has.end()) {
        return {has[target - nums[i]], i};
      }
      has[nums[i]] = i;
    }

    return {};
  }
};
```



### 变种

求满足特定值的所有可能，不能重复

```cpp
int solve(vector<int> nums, int target) {
  std::unordered_map<int, int> mp;
  int ans = 0;

  // 一遍hash，如果找到组合，对应位置设置成-1,表示已经使用了
  for (int i = 0; i < nums.size(); ++i) {
    if (mp[target - nums[i]] == 1) {
      ++ans;
      mp[target - nums[i]] = -1;
      mp[nums[i]] = -1;
      printf("nums[i]: %d, target - nums[i]: %d\n", nums[i], target - nums[i]);
    }

    // 如果hash对应记录的不是-1,则建立hash
    if (mp[nums[i]] != -1) {
      mp[nums[i]] = 1;
    }
  }

  return ans;
}

int main(int argc, char const *argv[]) {
  vector<int> nums = {1, 1, 1, 1, 5, 5, 5, 3, 3, 3, 3};

  printf("%d\n", solve(nums, 6));

  return 0;
}
```









## 三数之和

```cpp
class Solution {
 public:
  vector<vector<int>> threeSum(vector<int>& nums) {
    int n = nums.size();
    if (n < 3) {  // 特判
      return {};
    }

    vector<vector<int>> ans;
    std::sort(nums.begin(), nums.end());

    // 固定第一个数，转化为求两数之和
    for (int i = 0; i < n; ++i) {
      // 如果第一个数大于0，后面的都是大于0的数，不可能相加为0了
      if (nums[i] > 0) {
        return ans;
      }
      // 如果上一次已经处理过，则跳过
      if (i > 0 && nums[i] == nums[i - 1]) {
        continue;
      }

      // 双指针：在nums[i]后面区间寻找和为0-nums[i]的另外两个数
      int left = i + 1;
      int right = n - 1;
      while (left < right) {
        if (nums[left] + nums[right] > -nums[i]) {
          --right;  // 两数之和太大，右指针左移
        } else if (nums[left] + nums[right] < -nums[i]) {
          ++left;  // 两数之和太小，左指针右移
        } else {
          // 找到第一个和为0的三元组，添加到结果中，并继续寻找
          ans.push_back({nums[i], nums[left], nums[right]});
          ++left;
          --right;

          // 第二个数和第三个数也不能重复选取
          while (left < right && nums[left] == nums[left - 1]) ++left;
          while (left < right && nums[right] == nums[right + 1]) --right;
        }
      }
    }

    return ans;
  }
};
```



## [2. Add Two Numbers](https://leetcode-cn.com/problems/add-two-numbers/)

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
       ListNode* head = new ListNode(-1), *cur = head;
       int t = 0;

       while(l1 || l2 || t) {
           if(l1) t += l1->val, l1 = l1->next;
           if(l2) t += l2->val, l2 = l2->next;
           cur = cur->next = new ListNode(t % 10);

           t /= 10;
       }

       return head->next;

    }
};
```





## 玩转双指针



### 滑动窗口

https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/yi-ge-mo-ban-miao-sha-10dao-zhong-deng-n-sb0x/



 

#### [3. Longest Substring Without Repeating Characters](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```cpp
class Solution {
 public:
  int lengthOfLongestSubstring(string s) {
    // 1. 定义需要维护的变量
    int ans = 0, n = s.length();
    std::unordered_map<char, int> mp;

    // 2. 定义窗口的首尾端，然后从右端开始滑动窗口
    for (int left = 0, right = 0; right < n; ++right) {
      // 3. 滑动的时候需要维护变量
      mp[s[right]]++;

      // 4. 根据题意，题目的窗口长度可变，这个时候一般涉及到窗口是否合法问题
      // 需要用while去不断移动窗口指针，直到剔除掉非法元素值，使得窗口再次合法
      /**
       * 当mp[s[right]] > 1时候，
       * 说明s[right]为重复元素，需要移动left，直到去除掉左边和s[right]相同的元素
       */
      while (mp[s[right]] > 1) {
        mp[s[left++]]--;
      }

      // 更新最大长度
      ans = max(ans, right - left + 1);
    }

    return ans;
  }
};
```





## 深入浅出动态规划

### 基本动态规划：一维





### 基本动态规划：二维



### 子序列问题

#### [300. Longest Increasing Subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```cpp
class Solution {
 public:
  int lengthOfLIS(vector<int>& nums) {
    int maxlen = 0, n = nums.size();

    if (n <= 1) return n;
    vector<int> dp(n, 1);

    // dp[i] 表示dp[i] 可以表示已i结尾的，最长子序列长度
    for (int i = 0; i < n; ++i) {
      // 如果其之前的某个位置j所对应的数字小于位置i所对应的数字
      // 则我们可以获得一个以i结尾的、长度为dp[j]+1的子序列
      for (int j = 0; j < i; ++j) {
        if (nums[i] > nums[j]) {
          dp[i] = max(dp[i], dp[j] + 1);
        }
      }

      maxlen = max(maxlen, dp[i]);
    }
  }
};
```



#### **BM66** **最长公共子串**

```cpp
class Solution {
 public:
  /**
   * longest common substring
   * @param str1 string字符串 the string
   * @param str2 string字符串 the string
   * @return string字符串
   */
  string LCS(string str1, string str2) {
    int len1 = str1.length(), len2 = str2.length();

    vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
    int maxLen = 0, pos = 0;

    for (int i = 1; i <= len1; ++i) {
      for (int j = 1; j <= len2; ++j) {
        // 如果两位相同
        if (str1[i - 1] == str2[j - 1]) {
          dp[i][j] = dp[i - 1][j - 1] + 1;

          if (dp[i][j] > maxLen) {
            maxLen = dp[i][j];
            pos = i - 1;
          }
        }
      }
    }

    return str1.substr(pos - maxLen + 1, maxLen);
  }
};
```



#### 最长公共子序列

```cpp
class Solution {
 public:
  int longestCommonSubsequence(string text1, string text2) {
    int n = text1.length(), m = text2.length();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));

    for (int i = 1; i <= n; ++i) {
      for (int j = 1; j <= m; ++j) {
        if (text1[i - 1] == text2[j - 1]) {
          dp[i][j] = dp[i - 1][j - 1] + 1;
        } else {
          dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
      }
    }

    return dp[n][m];
  }
};
```













## 指针三剑客--链表

两个小技巧：

* 尽量处理当前节点的下一个节点
* 建立一个虚拟节点dummy

| 题号 | 题目                        | 题解          | 难度等级 |
| ---- | --------------------------- | ------------- | -------- |
| 19   | 删除链表的倒数第N个节点     | 两种实现+图解 | 中等     |
| 21   | 合并两个有序链表            | 两种实现+图解 | 简单     |
| 23   | 合并K个升序链表             | 四种实现+图解 | 困难     |
| 24   | 两两交换链表中的节点        | 三种实现+图解 | 中等     |
| 25   | K 个一组翻转链表            | 两种实现+图解 | 困难     |
| 61   | 旋转链表                    | 两种实现+图解 | 中等     |
| 82   | 删除排序链表中的重复元素 II | 三种实现+图解 | 中等     |
| 83   | 删除排序链表中的重复元素    | 两种实现+图解 | 简单     |
| 141  | 二叉树展开为链表            | 四种实现+图解 | 中等     |
| 138  | 复制带随机指针的链表        | 两种实现+图解 | 中等     |
| 141  | 环形链表                    | 两种实现+图解 | 简单     |
| 160  | 相交链表                    | 两种实现+图解 | 简单     |
| 203  | 移除链表元素                | 两种实现+图解 | 简单     |
| 206  | 反转链表                    | 两种实现+图解 | 简单     |
| 234  | 回文链表                    | 图解          | 简单     |
| 237  | 删除链表中的节点            | 图解          | 简单     |
| 876  | 链表的中间结点              | 图解          | 简单     |



#### [206. Reverse Linked List](https://leetcode-cn.com/problems/reverse-linked-list/)

```cpp
class Solution {
 public:
  // 递归版本
  ListNode *reverseList(ListNode *head, ListNode *prev = nullptr) {
    if (head == nullptr) return prev;

    ListNode *next = head->next;
    head->next = prev;
    return reverseList(next, head);
  }

  // 非递归版本
  ListNode *reverseList(ListNode *head) {
    ListNode *prev = nullptr, *next;
    while (head) {
      // 临时保存一下head的下一个节点
      next = head->next;
      // 反转指向
      head->next = prev;

      // 向前移动一步
      prev = head;
      head = next;
    }

    return prev;
  }
};
```



#### [19. Remove Nth Node From End of List](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(-1, head), *p = dummy;

        // 先走n步
        while(n--) {
            head = head->next;
        }

        // 两指针再一起走，当head为nullptr时候，p指向的就是待删除结点
        while(head) {
            p = p->next;
            head = head->next;
        }

        p->next = p->next->next;
        return dummy->next;

    }
};
```



#### [25. Reverse Nodes in k-Group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

递归版本

```cpp
class Solution {
 public:
  ListNode* reverseKGroup(ListNode* head, int k) {
    ListNode *cur = head, *tmp;

    int i = 0;
    for (; i < k && head != nullptr; ++i) {
      head = head->next;
    }
    if (i < k) return cur;

    ListNode* prev = reverseKGroup(head, k);

    for (int i = 0; i < k; ++i) {
      // 保存下一个节点
      tmp = cur->next;
      // 反转
      cur->next = prev;
      // 移动
      prev = cur;
      cur = tmp;
    }

    return prev;
  }
};
```

非递归版本

```cpp
class Solution {
 public:
  ListNode* reverseKGroup(ListNode* head, int k) {
    // 增加一个虚拟头结点
    ListNode* dummy = new ListNode(-1, head);

    // prev 指向上一组的末尾，初始化执行dummy
    // last 指向每组的末尾，反转前是末尾，反转后是开头
    ListNode *prev = dummy, *last = dummy;

    while (last != nullptr) {
      for (int i = 0; i < k && last != nullptr; ++i) {
        last = last->next;
      }

      // 如果last为空，表示这一组小于k个，因此不用反转
      if (last == nullptr) break;

      // 让上一组的末尾指向本组的开头，反转后last就是开头
      prev->next = last, prev = head;

      while (head != last) {
        ListNode* tmp = head->next;
        head->next = last->next;
        last->next = head;
        head = tmp;
      }

      last = prev;
      head = last->next;
    }

    return dummy->next;
  }
};

```







## 更复杂的数据结构

### 并查集



### 复合数据结构

#### [146. LRU Cache](https://leetcode-cn.com/problems/lru-cache/)

```cpp
// 使用双链表加hash表模拟LRU
class LRUCache {
 private:
  int capacity;                   // 容量
  std::list<pair<int, int>> lst;  // 模拟LRU队列
  // 记录key在链表中的位置
  std::unordered_map<int, list<pair<int, int>>::iterator> mp;

 public:
  LRUCache(int capacity) : capacity(capacity) {}

  int get(int key) {
    // 如果在哈希表中有记录，就将此记录移动到链表最后，并返回value
    if (mp.find(key) != mp.end()) {
      lst.splice(lst.end(), lst, mp[key]);
      return (*mp[key]).second;
    }

    return -1;
  }

  void put(int key, int value) {
    // 如果此时哈希表中有同名记录，就更新value，并将记录移动到最后
    if (mp.find(key) != mp.end()) {
      mp[key]->second = value;
      lst.splice(lst.end(), lst, mp[key]);
    } else {
      // 如果容量已满，就删除第一元素
      if (lst.size() == capacity) {
        mp.erase((*lst.begin()).first);
        lst.erase(lst.begin());
      }

      // 将新纪录插入到链表最后，并在哈希表中记录
      lst.push_back({key, value});
      mp[key] = --lst.end();
    }
  }
};
```









## 其他

### 在C ++中找到先增加然后减少的数组中的最大元素

假设我们有一个数组，它最初是增加然后减少。我们必须在数组中找到最大值。因此，如果数组元素类似于A = [8、10、20、80、100、250、450、100、3、2、1]，则输出将为500。

我们可以使用二进制搜索来解决这个问题。有三个条件-

- 当mid大于两个相邻元素时，mid是最大值
- 如果mid大于下一个元素，但小于上一个元素，则max位于mid的左侧。
- 如果mid元素小于下一个元素，但大于前一个元素，则max位于mid的右侧。

```cpp
#include <iostream>
using namespace std;
int getMaxElement(int array[], int left, int right) {
  if (left == right) return array[left];
  if ((right == left + 1) && array[left] >= array[right]) return array[left];
  if ((right == left + 1) && array[left] < array[right]) return array[right];
    
  int mid = (left + right) / 2;
  if (array[mid] > array[mid + 1] && array[mid] > array[mid - 1])
    return array[mid];
  if (array[mid] > array[mid + 1] && array[mid] < array[mid - 1])
    return getMaxElement(array, left, mid - 1);
  else
    return getMaxElement(array, mid + 1, right);
}

int main() {
  int array[] = {8, 10, 20, 80, 100, 250, 450, 100, 3, 2, 1};
  int n = sizeof(array) / sizeof(array[0]);
  cout << "The maximum element is: " << getMaxElement(array, 0, n - 1);
}
```



