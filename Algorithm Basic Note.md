# Algorithm Basic Note

## 贪心算法

- 局部最优推出全局最优
- 无固定套路

## 回溯算法

- 递归函数下，纯暴力搜索，用于解决有限个`for`循环解决不了的问题
- 常见：排列、组合、切割子串、子集、棋盘等
- 抽象化：n叉树
- <font color=red>基本模板</font>:

```cpp
void backtracking(<parameters>){
    if (<sth> == <sth>){
        <push_back>;
        return;
    }
    <other sentences>;
    for (<sth>){
        <push_back>;	// 插入
        backtracking(<parameters>);	// 递归
        <pop_back>;	// 回溯
    }
}
```

### 组合问题

```cpp
// C++实现
// 例题：给定两个整数 n 和 k，
// 返回范围 [1, n] 中所有可能的 k 个数的组合
class Solution {
private:
	vector<vector<int>> res;
	vector<int> path;
	void backtracking(int n, int k, int startIndex) {
		if (path.size() == k) {
			// 找到一组结果
			res.emplace_back(path);
			return;
		}
		for (int i = startIndex; i <= n - (k - path.size()) + 1; ++i) {
			path.emplace_back(i);	// 处理节点
			backtracking(n, k, i + 1);	// 递归
			path.pop_back();	// 回溯撤销
			// 实际上 res 每存入一组结果
             // 就执行了 k 次 path.pop_back()
		}
	}
public:
	vector<vector<int>> combine(int n, int k) {
		backtracking(n, k, 1);
		return res;
	}
};
```


### 切割问题

```cpp
// C++实现
// 例题：给一个字符串 s，将 s 分割成一些子串，
// 使每个子串都是回文串，返回 s 所有可能的分割方案
class Solution {
private:
    vector<vector<string>> res;
    vector<string> path;
    bool isPalindrome(const string& s) {
        for (int i = 0; i <= s.length() / 2; ++i)
            if (s[i] != s[s.length() - 1 - i])
                return false;
        return true;
    }
    void backtracking(const string& s,int startIndex) {
        if (startIndex >= s.size()) {
            res.emplace_back(path);
            return;
        }
        for (int i = startIndex; i < s.size(); ++i) {
            string str = s.substr(startIndex, i - startIndex + 1);
            if (isPalindrome(str))
                path.emplace_back(str);
            else continue;
            backtracking(s, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return res;
    }
};
```

### 集合问题

```cpp
// C++实现
// 例题：给一个整数数组 nums ，数组中的元素互不相同
// 返回该数组所有可能的子集（幂集）
class Solution {
private:
	vector<vector<int>> res;
	vector<int> path;
	void backtracking(vector<int>& nums, int startIndex) {
		res.emplace_back(path);
		for (int i = startIndex; i < nums.size(); ++i) {
			path.emplace_back(nums[i]);
			backtracking(nums, i + 1);
			path.pop_back();
		}
	}
public:
	vector<vector<int>> subsets(vector<int>& nums) {
		backtracking(nums, 0);
		return res;
	}
};
```

### 子序列问题

- 树层去重与树枝去重

```cpp
// C++实现
// 例题：给一个整数数组 nums ，找出并返回所有该数组中不同的
// 递增子序列，递增子序列中至少有两个元素你可以按任意顺序返回答案
// 数组中可能含有重复元素，如出现两个整数
// 相等，也可以视作递增序列的一种特殊情况。
// 关键：树层去重，树枝不去重
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;
    void backtrackinig(vector<int>& nums, int startIndex) {
        if (path.size() > 1)
           res.emplace_back(path);
        unordered_set<int> uset;
        for (int i = startIndex; i < nums.size(); ++i) {
            if ((!path.empty() && nums[i] < path.back())
                || uset.find(nums[i])!=uset.end())
                continue;
            uset.insert(nums[i]);
            path.emplace_back(nums[i]);
            backtrackinig(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        backtrackinig(nums, 0);
        return res;
    }
};
```

### 排列问题

```cpp
// C++实现
// 例题：给定一个不含重复数字的数组 nums
// 返回其所有可能的全排列 
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            res.emplace_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); ++i) {
            if (used[i] == true) continue;
            used[i] = true;
            path.emplace_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return res;
    }
};
```

### 棋盘问题

#### N皇后问题

```cpp
// C++实现
// 例题：给一个整数 n ,要求将 n 个皇后放置在 n×n 的棋盘上，
// 并且使皇后彼此之间不能相互攻击
// 返回所有不同的 n 皇后问题的解决方案
// 该方案中 'Q' 和 '.' 分别代表了皇后和空位
class Solution {
    vector<vector<string>> res;

    bool isValid(int row, int col, vector<string>& chessboard,int n) {
        // check col
        for (int i = 0; i < row; ++i)
            if (chessboard[i][col] == 'Q')
                return false;
        // check incline
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) 
            if (chessboard[i][j] == 'Q')
                return false;
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; --i, ++j)
            if (chessboard[i][j] == 'Q')
                return false;
        return true;
    }

    void backtracking(int n,int row,vector<string>& chessboard) {
        if (row == n) {
            res.emplace_back(chessboard);
            return;
        }
        for (int col = 0; col < n; ++col) {
            if (isValid(row, col, chessboard, n)) {
                chessboard[row][col] = 'Q';
                backtracking(n, row + 1, chessboard);
                chessboard[row][col] = '.';
            }
        }
    }
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<string> chessboard(n, string(n, '.'));
        backtracking(n, 0, chessboard);
        return res;
    }
};
```

#### 数独问题

```cpp
// C++实现
// 例题：编写一个程序，通过填充空格来解决数独问题。
// 数独的解法需 遵循如下规则：
// 数字 1-9 在每一行只能出现一次
// 数字 1-9 在每一列只能出现一次
// 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次
// 数独部分空格内已填入了数字，空白格用 '.' 表示
class Solution {
private:
    bool isValid(int row, int col, char val, vector<vector<char>>& board) {
        // check row
        for (int i = 0; i < 9; ++i)
            if (board[row][i] == val)
                return false;
        // check col
        for (int i = 0; i < 9; ++i)
            if (board[i][col] == val)
                return false;
        // check square
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; ++i)
            for (int j = startCol; j < startCol + 3; ++j)
                if (board[i][j] == val)
                    return false;
        return true;
    }
    
    // 只用得出一个结果，核心算法直接用bool
    bool backtracking(vector<vector<char>>& board) {
        for (int i = 0; i < 9; ++i) {
            for (int j = 0; j < 9; ++j) {
                if (board[i][j] == '.') {
                    for (char k = '1'; k <= '9'; ++k) {
                        if (isValid(i, j, k, board)) {
                            board[i][j] = k;
                            // 找到一组符合条件的解立即返回
                            if (backtracking(board))
                                return true;
                            board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};
```


## 动态规划

- <font color=red>关键点</font>：动规五部曲
  - 确定dp数组及其下标的含义
  - 确定递推公式
  - dp数组如何初始化
  - 确定遍历顺序
  - 举例推导dp数组


### 背包问题

- 三种常见背包
  - $0-1$背包：有$n$种物品，每样只有$1$个
  - $n$重背包：有$n$种物品，每样物品的个数各不相同
  - 完全背包：有$n$种物品，每样物品都有无限个

#### $0-1$背包问题

```cpp
// C++实现
// 例题：小明的行李空间为 N，研究材料有 M 件
// 求出背包所能携带的最大价值，其中每种研究材料只能选择一次，
// 并且只有选与不选两种选择，不能进行切割
// 对每个传入的 materials[i]
// materials[i][0]表示所占空间，materials[i][1]表示
class Solution {
public:
    int maxValue(int capacity, vector<vector<int>>& materials) {
        // dp数组两个索引i和j分别代表物品序列和容量序列
        vector<vector<int>> dp(materials.size(),
            vector<int>(capacity + 1, 0));

        // 在空间容量大于等于第一个物品所占空间的情况下
        // 设定对应的dp[0][j]初始值为第一件物品的价值
        for (int j = materials[0][0]; j <= capacity; ++j)
            dp[0][j] = materials[0][1];

        for (int i = 1; i < materials.size(); ++i)
            for (int j = 0; j <= capacity; ++j)
                // 若装不下，与不装这个物品的情况相同
                if (j < materials[i][0])
                    dp[i][j] = dp[i - 1][j];
                // 否则更新为不装这个物品的最大值与
                // 装这个物品的最大值这两者的最大值
                else
                    dp[i][j] = max(dp[i - 1][j], 
                        dp[i - 1][j - materials[i][0]] + materials[i][1]);

        return dp[materials.size() - 1][capacity];
    }
};
```

- 对上述例题的另一种解法：滚动数组
  - 将二维dp数组转化为一维dp数组
  - 弊端：不能通过回溯得到选择方案，只能得到最优结果
```cpp
// C++实现
class Solution {
public:
    int maxValue(int capacity, vector<vector<int>>& materials) {
        vector<int> dp(capacity + 1);
        for (int i = 1; i < materials.size(); ++i)
            for (int j = capacity; j >= materials[i][0]; --j)
                // 倒序遍历j，保证物品只放入一次
                // 一个数据仅由该数据的左上方数据生成
                // 倒序遍历在还没有生成某个数据前保护了上一层所需数据
                dp[j] = max(dp[j], 
                    dp[j - materials[i][0]] + materials[i][1]);
        return dp[capacity];
    }
};
```

#### $n$重背包问题

- 朴素方法：将物品件数展开，看成是$n$种不同的物品，从而将问题转换为$0-1$背包问题
- 二进制优化：若某种物品有$n$件，可将其作如下分组：
$$
1,2,2^2,2^3,\dots,2^{[\log_{2} n]},n+1-2^{[\log_{2} n]}.
$$
	  于是，对 $\forall i \in [1,n],i\in \mathrm{N_+}$，都可以通过有限次加法运算用上述分组将 $i$ 表示出来

```cpp
// C++实现
// 例题：有 n 种物品，items[i][0]表示物品重量
// items[i][1]表示物品价值，items[i][2]表示物品件数
// 现有一个容量为 V 的背包
class Solution {
public:
	int maxValue(int V, vector<vector<int>>& items) {
		// 二进制优化法
		vector<vector<int>> openItems;
		vector<int> tmp(2);
		for (int i = 0; i < items.size(); ++i) {
			for (int j = 1; j <= items[i][2]; j <<= 1) {
				tmp[0] = items[i][0] * j;
				tmp[1] = items[i][1] * j;
				openItems.emplace_back(tmp);
				items[i][2] -= j;
			}
			if (items[i][2]) {
				tmp[0] = items[i][0] * items[i][2];
				tmp[1] = items[i][1] * items[i][2];
				openItems.emplace_back(tmp);
			}
		}
		// dp滚动数组求解0-1背包
		vector<int> dp(V + 1);
		for (int i = 1; i < openItems.size(); ++i)
			for (int j = V; j >= openItems[i][0]; --j)
				dp[j] = max(dp[j], dp[j - openItems[i][0]] + openItems[i][1]);
		return dp[V];
	}
};
```

#### 完全背包问题

- 二维dp数组的状态转移方程：
  $$
  \mathrm{dp}[i][j]=\max(\mathrm{dp}[i][j-\mathrm{w}[i]]+\mathrm{c}[i],\mathrm{dp}[i-1][j])\\
  (0\le k\le \frac{j}{\mathrm{w}[i]})
  $$
  特别地，不放入物品可以看作放入$k=0$件物品

  - 与$0-1$背包二维转移方程的联系：二者在公式上仅在$\mathrm{dp}[i-1][j-\mathrm{w}[i]$与$\mathrm{dp}[i][j-\mathrm{w}[i]]$上有差别

```cpp
// C++实现
// 例题：有 n 种物品，每种物品有无限个
// items[i][0]表示物品重量，items[i][1]表示物品价值
// 现有一个容量为 V 的背包

// 方法一：二维dp数组
class Solution {
public:
	int maxValue(int V, vector<vector<int>>& items) {
		vector<vector<int>> dp(items.size(), vector<int>(V + 1, 0));
		for (int j = items[0][0]; j <= V; ++j)
			dp[0][j] = items[0][1];
		for (int i = 1; i < items.size(); ++i)
			for (int j = 0; j <= V; ++j)
				if (j < items[i][0])
					dp[i][j] = dp[i - 1][j];
				else
					dp[i][j] = max(dp[i - 1][j],
						dp[i][j - items[i][0]] + items[i][1]);
		return dp[items.size() - 1][V];
	}
};
// 方法二：一维dp数组
class Solution {
public:
	int maxValue(int V, vector<vector<int>>& items) {
		vector<int> dp(V+1,0);
		for (int i = 1; i < items.size(); ++i)
			for (int j = items[i][0]; j <= V; ++j)
				// 注意与0-1背包不同点，正序遍历j
				// 0-1背包需要上一行在当前遍历元素前面的元素的信息
				// 完全背包需要当前行在当前遍历元素前面的元素的信息
				dp[j] = max(dp[j], dp[j - items[i][0]] + items[i][1]);
		return dp[V];
	}
};
```

- 当仅要求得出最大装满量，两层`for`循环可颠倒
- 当要求得出装满背包的方法，递推公式一般为`dp[i] += dp[i - nums[j]]`
- 当需要考虑排列数还是组合数时，遍历顺序有讲究
  - 排列：先遍历背包，后遍历物品
  - 组合：先遍历物品，后遍历背包

### 打家劫舍问题

```cpp
// C++实现
// 例题：你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定
// 的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗
// 系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
// 给定一个代表每个房屋存放金额的非负整数数组，
// 计算不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额

// 方法一：一维dp数组
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        switch (n) {
        case 0:
            return 0;
        case 1:
            return nums[0];
        }
        vector<int> dp(n);
        dp[0] = nums[0], dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < n; ++i)
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        return dp[n - 1];
    }
};
// 方法二：双变量代替数组
// ...
int pre = 0, cur = nums.front();
	for (int i = 1; i < nums.size(); ++i) {
        pre = max(pre + nums[i], cur);
        swap(pre, cur);
    }
return cur;
```

- 与二叉树的结合
```cpp
// C++实现
// 例题：某地区只有一个入口 root ，除root外，每个房子有且只有
// 一个"父"房子与之相连，若两个直接相连的房子在同一天晚上被打劫，
// 房屋会自动报警
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode() :val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) :val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, 
        TreeNode* right) :val(x), left(left), right(right) {}
};

class Solution {
private:
    // 定义长度为2的数组，0：不偷，1：偷
    vector<int> robTree(TreeNode* cur) {
        if (cur == nullptr) return vector<int>{0, 0};
        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        // 偷cur，不能偷左右节点
        int val1 = cur->val + left[0] + right[0];
        // 不偷cur,可褕可不偷左右节点
        int val2 = max(left[0], left[1]) + max(right[0] , right[1]);
        return vector<int>{ val2,val1 };
    }
public:
    int rob(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->left == nullptr && root->right == nullptr) return root->val;
        vector<int> res = robTree(root);
        return max(res[0], res[1]);
    }
};
```

### 股票问题

```cpp
// C++实现
// 例题：给定一个数组 prices ，它的第 i 个元素 prices[i] 表示
// 一支给定股票第 i 天的价格。你只能选择某一天买入这只股票，
// 并选择在未来的某一个不同的日子卖出该股票
// 设计一个算法来计算你所能获取的最大利润。
// 返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 

// 方法一：二维dp数组
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (!prices.size()) return 0;
      // 二维dp数组，dp[i][0]:持有，dp[i][1]：不持有
      // 持有：前一天已经持有 or 这一天买入
      // dp[i][0] = max(dp[i-1][0],-prices[i])
      // 不持有：前一天也不持有 or 这一天卖出
      // dp[i][1] = max(dp[i-1][1],dp[i-1][0] + prices[i])
        vector<vector<int>> dp(prices.size(), vector<int>(2));
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.size(); ++i) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp.back()[1];
    }
};
// 方法二：滚动数组
// 可以观察到，dp[i]只依赖dp[i-1]
        int len = prices.size();
        vector<vector<int>> dp(2, vector<int>(2));
        dp[0][0] = -prices[0];
        for (int i = 1; i < len; i++) {
            dp[i % 2][0] = max(dp[(i - 1) % 2][0], -prices[i]);
            dp[i % 2][1] = max(dp[(i - 1) % 2][1],
                prices[i] + dp[(i - 1) % 2][0]);
        }
        return dp[(len - 1) % 2][1];
```

### 子序列问题

- 一个父数组

```cpp
// C++实现
// 例题：给一个整数数组 nums ，返回其中最长的严格递增子序列长度
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) return n;
        vector<int> dp(n, 1);
        int res{};
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j)
                if (nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j] + 1);
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

- 两个父数组

  - 子数组(元素顺序可变)


```cpp
// C++实现
// 例题：给两个整数数组 nums1 和 nums2 
// 返回两个数组中公共的最长的子数组的长度

// 方法一：二维dp数组
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int la = nums1.size(), lb = nums2.size();
        if (la == 0 || lb == 0) return 0;
        // 下标从1开始而不从0开始，可避免
        // 因对dp[0][j]和dp[i][0]初始化造成的不便
        // 否则应多写两次单层for循环确定下标0处的初始值
        vector<vector<int>> dp(la + 1, vector<int>(lb + 1));
        int res{};
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (nums1[i - 1] == nums2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }
};
// 方法二：一维滚动dp数组
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int la = nums1.size(), lb = nums2.size();
        if (la == 0 || lb == 0) return 0;
        int res{};
        vector<int> dp(lb + 1);
        // 后一个元素由前一个元素生成，注意要倒序遍历
        for (int i = 1; i <= la; ++i) {
            for (int j = lb; j > 0; --j) {
                if (nums1[i - 1] == nums2[j - 1])
                    dp[j] = dp[j - 1] + 1;
                else dp[j] = 0;
                res = max(dp[j], res);
            }
        }
        return res;
    }
};
```

  - 子序列(元素顺序不可变)


```cpp
// C++实现
// 例题：给定两个字符串 text1 和 text2
// 返回这两个字符串的最长公共子序列的长度
// 如果不存在公共子序列 ，返回 0

// 方法一：二维dp数组
class Solution {
public:
    int longestCommonSubsequence(string s, string ss) {
        int la = s.size(), lb = ss.size();
        if (la == 0 || lb == 0) return 0;
        vector<vector<int>> dp(la + 1, vector<int>(lb + 1));
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (s[i - 1] == ss[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[la][lb];
    }
};
// 方法二：一维滚动dp数组
class Solution {
public:
    int longestCommonSubsequence(string s, string ss) {
        int la = s.size(), lb = ss.size();
        if (la == 0 || lb == 0) return 0;
        vector<vector<int>> dp(2, vector<int>(lb + 1));
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (s[i - 1] == ss[j - 1])
                    dp[i % 2][j] = dp[(i - 1) % 2][j - 1] + 1;
                else
                    dp[i % 2][j] = max(dp[(i - 1) % 2][j], dp[i % 2][j - 1]);
            }
        }
        return dp[la % 2][lb];
    }
};
```

### 编辑距离问题

- 仅删除：

```cpp
// C++实现
// 例题：给定两个单词 word1 和 word2 
// 返回使得 word1 和  word2 相同所需的最小步数
// 每步可以删除任意一个字符串中的一个字符

// 方法一：直接遍历，不同就删除这个元素
class Solution {
public:
    int minDistance(string word1, string word2) {
        int la = word1.size(), lb = word2.size();
        vector<vector<int>> dp(la + 1, vector<int>(lb + 1));
        for (int i = 0; i <= la; ++i) dp[i][0] = i;
        for (int j = 0; j <= lb; ++j) dp[0][j] = j;
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (word1[i - 1] == word2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1];
                else
                    // 不同则删除其中一个元素
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
            }
        }
        return dp[la][lb];
    }
};
// 方法二：先求出两个string的公共子序列，再将剩余部分删除即可
class Solution {
public:
    int minDistance(string word1, string word2) {
        int la = word1.size(), lb = word2.size();
        vector<vector<int>> dp(la + 1, vector<int>(lb + 1));
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (word1[i - 1] == word2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return la + lb - 2 * dp[la][lb];
    }
};
```

- 插入、删除、替换混合操作

```cpp
// C++实现
// 例题：给两个单词 word1 和 word2，
// 返回将 word1 转换成 word2 所使用的最少操作数
// 你可以对一个单词进行如下三种操作：
// 插入一个字符、删除一个字符、替换一个字符
class Solution {
public:
    int minDistance(string word1, string word2) {
        int la = word1.size(), lb = word2.size();
        vector<vector<int>> dp(la + 1, vector<int>(lb + 1));
        for (int i = 0; i <= la; ++i) dp[i][0] = i;
        for (int j = 0; j <= lb; ++j) dp[0][j] = j;
        for (int i = 1; i <= la; ++i) {
            for (int j = 1; j <= lb; ++j) {
                if (word1[i - 1] == word2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1];
                else
                    // 增与删实质一样
                    // 替换：由dp[i-1][j-1]+1得出
                    dp[i][j] = min({ dp[i - 1][j - 1],dp[i - 1][j],dp[i][j - 1] }) + 1;
            }
        }
        return dp[la][lb];
    }
};
```

## 单调栈

- 思想：利用栈的LIFO原理，空间换时间（时间复杂度 $O(n)$）
- 单增栈，单减栈
- 应用：给一个数组，求解左(右)侧第一个大于元素x或小于x的位置(元素)
  - 找大于x的数用单减栈，目的是让`res`顺利出栈；反之同理
- 主要步骤（以单增栈举例）：
  - 对数组从左往右开始遍历，如果遍历到的元素`arr[i]`大于等于栈顶元素`stk.top()`，直接将`i`入栈
  - 如果`arr[i] < stk.top()`，不断弹出栈顶元素直到满足上面情况，此时栈顶元素就是数组中左侧第一个比`arr[i]`小的元素

```cpp
// C++实现
// 例题：给定一个整数数组 temperatures 表示每天的温度
// 返回一个数组 res,其中 res[i] 是指对于第 i 天
// 下一个更高温度出现在几天后.如果气温在这之后
// 都不会升高，在该位置用 0 来代替
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& t) {
        stack<int> stk;	// dan
        vector<int> res(t.size());
        stk.emplace(0);
        for (int i = 1; i < t.size(); ++i) {
            if (t[i] <= t[stk.top()]) stk.emplace(i);
            else {
                while (!stk.empty() && t[i] > t[stk.top()]) {
                    res[stk.top()] = i - stk.top();
                    stk.pop();  // 不符合单调栈的元素出栈
                }
                stk.emplace(i); // 直到符合第一种情况
            }
        }
        return res;
    }
};
// 精简版：
//...
            while (!stk.empty() && t[i] > t[stk.top()]) {
                res[stk.top()] = i - stk.top();
                stk.pop();
            }
            stk.emplace(i);
```

## 并查集

Union-Find 算法

- 描述：若两个元素$p,q$是相连的，则满足自反性、对称性、传递性三条性质。当且仅当两个元素相连时同属一个等价类，要求过滤每一个等价类中无意义的元素对。

- 应用：网络、变量名等价性、数学集合等

- 算法相关：

  - 用一个以触点为索引的数组`id[]`作为基本数据结构来表示所有分量
  - 成本模型：数组的访问次数(访问任意数组 元素的次数，无论读写)

- 算法实现：
  ```cpp
  // C++算法实现
  class UF {
  private:
      int* id = NULL; // 分量id（以触点为索引）
      int cnt;  // 分量数量
      int len;  // 数组大小
  public:
      UF(int cnt) :cnt(cnt),len(cnt) { 
          id = new int[cnt]; 
          // 初始状态下，每个触点都构成了一个只含有自己的分量
          for (int i = 0; i < cnt; i++)
              id[i] = i;  
      }
      ~UF() { delete[] id; }
      int count() { return cnt; }
      bool connnected(int p, int q) { return Find(p) == Find(q); }
  
      int Find(int p);
      void Union(int p, int q);
  
      int quickFind(int p);
      void quickUnion(int p, int q);
  };
  
  int UF::Find(int p) { return id[p]; }
  void UF::Union(int p, int q) {
      int pID = Find(p);
      int qID = Find(q);
      // 如果p和q已经在同一个分量，不操作
      if (pID == qID) return;
      // 将p的分量重命名为q的分量
      for (int i = 0; i < len; i++)
          if (id[i] == pID)
              id[i] = qID;
      cnt--;
  }
  
  int UF::quickFind(int p) {
      //找出分量名称
      while (p != id[p])
          p = id[p];
      return p;
  }
  void UF::quickUnion(int p, int q) {
      // 解决常规Union()方法在数据量大时运行慢的问题
      // 树形结构
      // 将p和q的根节点统一
      int pRoot = quickFind(p);
      int qRoot = quickFind(q);
      if (pRoot == qRoot) return;
      id[pRoot] = qRoot;
      cnt--;
  }
  ```

  上面算法（即使是`quickUnion()`方法）仍然存在问题，即最坏情况下运行时间仍为平方级别。于是，我们可以做出如下改动：每次将较小的树连接到较大的树上面，这项改动需要添加一个数组和一些代码来记录树的节点数。这个改进算法称为***加权quick-union算法***

  ```cpp
  // C++实现 加权quick-union算法
  typedef class WeightedQuickUnionUF {
  private:
      int cnt;
      int* id = NULL; // 父链接数组（由触点索引）
      int* size = NULL;   // （由触点索引的）各个根节点所对应的分量的大小
  public:
      WeightedQuickUnionUF(int cnt) :cnt(cnt) {
          id = new int[cnt];
          for (int i = 0; i < cnt; i++)
              id[i] = i;
          size = new int[cnt];
          for (int i = 0; i < cnt; i++)
              size[i] = 1;
      }
      ~WeightedQuickUnionUF() { delete[] id, size; }
      int count() { return cnt; }
      bool connected(int p, int q) { return Find(p) == Find(q); }
      int Find(int p) {
          // 跟随链接找到根节点
          while (p != id[p])
              p = id[p];
          return p;
      }
      void Union(int p, int q) {
          int i = Find(p), j = Find(q);
          if (i == j)  return;
          // 将小树的根节点连接到大叔的根节点
          if (size[i] < size[j]) {
              id[i] = j;
              size[j] += size[i];
          }
          else {
              id[j] = i;
              size[i] += size[j];
          }
          cnt--;
      }
  } weighQU;
  ```

- 最优算法：压缩路径的加权quick-union算法
  为`Find()`添加一个循环，在路径上遇到的所有节点都直接链接到根节点，这样得到一个几乎完全扁平化的树

  ```cpp
  // C++实现
  class CmpWayWeighQU {
  private:
      int cnt, len;
      int* parent, * size;
  public:
      CmpWayWeighQU(int cnt) :cnt(cnt),len(cnt) {
          parent = new int[cnt];
          for (int i = 0; i < cnt; i++)
              parent[i] = i;
          size = new int[cnt];
          for (int i = 0; i < cnt; i++)
              size[i] = 1;
      }
      ~CmpWayWeighQU() { delete[] parent, size; }
      int count() { return cnt; }
      bool connected(int p, int q) { return Find(p) == Find(q); }
  
      void validate(int p) {
          // 验证p是一个有效的索引
          int n = len;
          if (p < 0 || p >= n)
              throw "error";
      }
      int Find(int p) {
          validate(p);
          int root = p;
          while (root != parent[root])
              root = parent[root];
          while (p != root) {
              int temp = parent[p];
              parent[p] = root;
              p = temp;
          }
          return root;
      }
      void Union(int p, int q) {
          int rootP = Find(p), rootQ = Find(q);
          if (rootP == rootQ)   return;
          if (size[rootP] < size[rootQ]) {
              parent[rootP] = rootQ;
              size[rootQ] += size[rootP];
          }
          else {
              parent[rootQ] = rootP;
              size[rootP] += size[rootQ];
          }
          cnt--;
      }
  };
  ```

