# Data Structure Note

## 三种常见的基础数据类型

### Bag 包

不支持从中删除元素的集合数据类型，它的目的就是帮助用例收集元素并迭代遍历所有收集到的元素，元素的处理顺序不重要

### Queue 队列

符合先进先出(FIFO)原则，元素的处理顺序就是它们被添加到队列中的顺序，使用目的是在用集合保存元素的同时保存它们的相对顺序

#### 优先队列

- 顶部元素为最大元素

### Stack 栈

符合后进先出(LIFO)原则

```cpp
// C++算法
// 链表实现下压堆栈
template <class T> class Stack
{
private:
	int n;
	struct Node {
		T val;
		Node* next = NULL;
	};
	Node* head = NULL;
public:
	Stack(int n, T val) :n(n) {
		Node* newNode = new Node(val);
		head = newNode;
	}
	Stack(int n) :n(n) {
		Node* newNode = new Node(0);
		head = newNode;
	}
	~Stack() {
		while (head) {
			Node* temp = head;
			head = head->next;
			delete temp;
		}
	}
	int getSize() {
		Node* pt = head;
		int cnt = 0;
		while (pt->next) {
			pt = pt->next;
			cnt++;
		}
		return cnt;
	}
	Node* getHead() { return head; }
	bool isEmpty() { return head == NULL; }
	void push_back(T val);
	void pop();
};

template <class T>
void Stack<T>::push_back(T val) {
	int cnt = getSize();
	if (cnt >= n) {
		cout << "#### Stack is filled! ####" << endl;
		return;
	}
	Node* newNode = new Node(val);
	if (!head || !(head->next))
		head->next = newNode;
	else {
		Node* pt = head;
		while (pt->next)
			pt = pt->next;
		pt->next = newNode;
	}
}

template <class T>
void Stack<T>::pop() {
	if (!head)
		cout << "#### Stack is empty! ####" << endl;
	else if (!head->next)
		head = NULL;
	else {
		Node* pt = head;
		while (pt->next->next)
			pt = pt->next;
		pt->next = NULL;
	}
}
```

#### 栈的应用举例：Dijkstra双栈算术表达式求值算法

- 构建两个栈分别存储操作数和运算符

- 接受一个输入的字符串(算术表达式)，对字符进行判断后分别入栈

- ***忽略左括号***

- 遇到右括号时，弹出一个运算符，弹出所需数量的操作数，并将运算符和操作数的运算结果压入操作数栈

  ```cpp
  // C++算法实现
  class Solution {
  public:
      int precedence(char op) {
          // 运算符优先级
          switch (op) {
          case '+':
              return 1;
          case '-':
              return 1;
          case '*':
              return 2;
          case '/':
              return 2;
          }
      }
  
      int applyOp(int a, int b, char op) {
          // 运算符操作
          switch (op) {
          case '+':
              return a + b;
          case '-':
              return a - b;
          case '*':
              return a * b;
          case '/':
              try {
                  if (b == 0)
                      throw "ERROR";
                  return a / b;
              }
              catch (const char* s) {
                  cout << s << endl;
                  return 0;
              }
          }
      }
  
      int evaluate(const string& expre) {
          stack<int> values;
          stack<char> ops;
  
          for (size_t i = 0; i < expre.length(); i++) {
              if (isspace(expre[i]) || expre[i] == '(')
                  // 忽略空格与左括号
                  continue;
  
              if (isdigit(expre[i])) {
                  int val = 0;
                  while (i < expre.length() && isdigit(expre[i])) {
                      val = (val * 10) + (expre[i] - '0');
                      i++;
                  }
                  values.push(val);
                  i--;
              }
              else if (expre[i] == ')') {
                  while (!ops.empty()) {
                      int val2 = values.top();
                      values.pop();
  
                      int val1 = values.top();
                      values.pop();
  
                      char op = ops.top();
                      ops.pop();
  
                      values.push(applyOp(val1, val2, op));
                  }
                  if (!ops.empty()) {
                      ops.pop();
                  }
              }
              else {
                  while (!ops.empty() && precedence(ops.top()) >= precedence(expre[i])) {
                      // 比较运算符栈顶元素优先级与当前运算符优先级
                      // 如果栈顶操作符的优先级更高，则先计算
                      // 确保栈顶操作符优先级小于当前运算符优先级或栈为空
                      int val2 = values.top();
                      values.pop();
  
                      int val1 = values.top();
                      values.pop();
  
                      char op = ops.top();
                      ops.pop();
  
                      values.push(applyOp(val1, val2, op));
                  }
                  ops.push(expre[i]);
              }
          }
  
          while (!ops.empty()) {
              // 开始操作栈内剩余元素
              int val2 = values.top();
              values.pop();
  
              int val1 = values.top();
              values.pop();
  
              char op = ops.top();
              ops.pop();
  
              values.push(applyOp(val1, val2, op));
          }
  
          return values.top();
      }
  };
  ```

## Tree 树

### 二叉树

- 常见种类：满二叉树、完全二叉树、二叉搜索树、平衡二叉搜索树
- 存储方式：链式存储、顺序存储
  - 链式：指针
  - 顺序：数组，若父节点的数组下标为`i`，则左孩子下标为`2*i+1`，右孩子下标为`2*i+2`
- 遍历方式：深度优先搜索(DFS)、广度优先搜索(BFS)
  - DFS：前序遍历、中序遍历、后序遍历(递归法、迭代法)
    - 助记：前中后指中间节点的遍历顺序，即：前序=中左右，中序=左中右，后序=左右中
  - BFS：层次遍历(迭代法)

#### 二叉树的遍历

##### 递归遍历

- 前序遍历
```cpp
// C++实现
void traversal(TreeNode* cur, vector<int>& vals) {
    if (cur == nullptr) return;
    vals.emplace_back(cur->val);    // 中
    traversal(cur->left, vals);     // 左
    traversal(cur->right, vals);    // 右
}
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    traversal(root, res);
    return res;
}
```

- 中序遍历与后序遍历同理

##### 迭代遍历

- 借助栈实现

- 前序遍历
```cpp
// C++实现
vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	stack<TreeNode*> st;
	if (!root) return res;
	st.emplace(root);
	while (!st.empty()) {
		TreeNode* node = st.top();	// 中
		st.pop();
		res.emplace_back(node->val);
		if (node->right) st.emplace(node->right);	// 右
		if (node->left) st.emplace(node->left);		// 左
	}
	return res;
}
```

- 中序遍历
  - 借助一个指针记录访问过的节点
```cpp
// C++实现
vector<int> inorderTraversal(TreeNode* root) {
	vector<int> res;
	stack<TreeNode*> st;
	TreeNode* cur = root;
	while (cur || !st.empty()) {
		// 先一路向左访问到最底层
		// 并将对应数据入栈
		if (cur) {
			st.emplace(cur);
			cur = cur->left;
		}
		// 到达最底层后，栈顶元素出栈,记录中节点
		// 再向右走
		else {
			cur = st.top();
			st.pop();
			res.emplace_back(cur->val);
			cur = cur->right;
		}
	}
	return res;
}
```

- 后序遍历
  后序遍历只需在前序遍历的基础上，将左右孩子入栈顺序调换并最后将数组颠倒即可，即：中左右 -> 中右左 -> 左右中

##### 层序遍历

- 递归法
```cpp
// C++实现
void order(TreeNode* cur, vector<vector<int>>& res, int depth) {
	if (!cur) return;
	if (res.size() == depth) res.emplace_back(vector<int>());
	res[depth].emplace_back(cur->val);
	order(cur->left, res, depth + 1);
	order(cur->right, res, depth + 1);
}
vector<vector<int>> levelOrder(TreeNode* root) {
	vector<vector<int>> res;
	order(root, res, 0);
	return res;
}
```
- 迭代法：借助队列实现
```cpp
// C++实现
vector<vector<int>> levelOrder(TreeNode* root) {
	queue<TreeNode*> que;
	if (root) que.emplace(root);
	vector<vector<int>> res;
	while (!que.empty()) {
		int size = que.size();
		vector<int> vec;
		for (int i = 0; i < size; ++i) {
			TreeNode* node = que.front();
			que.pop();
			vec.emplace_back(node->val);
			if (node->left) que.emplace(node->left);
			if (node->right) que.emplace(node->right);
		}
		res.emplace_back(vec);
	}
	return res;
}
```

##### 复原二叉树

在已知前序(后序)与中序的情况下可以复原出唯一二叉树，其中前序(后序)确定根，中序确定左右子树

- 关键点：确定每次迭代左右子树的root对应的下标

- 已知前序与中序
```cpp
// C++实现
TreeNode* buildTree(vector<int>& preOrder, vector<int>& inOrder, int preStart, int inStart, int inEnd) {
	if (preStart >= preOrder.size() || inStart > inEnd) return nullptr;
	TreeNode* root = new TreeNode(preOrder[preStart]);
	int inIndex = find(inOrder.begin(), inOrder.end(), root->val) - inOrder.begin();
	// 左子树下标从 preStart+1 开始
	root->left = buildTree(preOrder, inOrder, preStart + 1, inStart, inIndex - 1);
	// 左子树包含 inIndex-inStart+1 个元素
	// 于是右子树下标从 (preStart+1)+(inIndex-inStart) 开始
	root->right = buildTree(preOrder, inOrder, preStart + inIndex - inStart + 1, inIndex + 1, inEnd);
	return root;
}
TreeNode* recoverTree(vector<int>& preOrder, vector<int>& inOrder) {
	return buildTree(preOrder, inOrder, 0, 0, inOrder.size() - 1);
}
```
- 已知后序与中序
```cpp
// C++实现
TreeNode* buildTree(vector<int>& postOrder, int postStart, int postEnd, vector<int>& inOrder, int inStart, int inEnd) {
    if (postStart > postEnd || inStart > inEnd) return nullptr;
    TreeNode* root = new TreeNode(postOrder[postEnd]);
    int inIndex = find(inOrder.begin(), inOrder.end(), root->val) - inOrder.begin();
    int leftSize = inIndex - inStart;
    root->left = buildTree(postOrder, postStart, postStart + leftSize - 1, inOrder, inStart, inIndex - 1);
    root->right = buildTree(postOrder, postStart + leftSize, postEnd - 1, inOrder, inIndex + 1, inEnd);
    return root;
}
TreeNode* recoverTree(vector<int>& postorder, vector<int>& inorder) {
    return buildTree(postorder, 0, postorder.size() - 1, inorder, 0, inorder.size() - 1);
}
```

#### 二叉堆

- 定义：当一棵二叉树的每个节点都大于等于它的两个子节点时，称为**堆有序**

- 核心代码：父子节点调整
  ```cpp
  // C++实现
  void swim(int k) {
  	// 自底向上
  	while (k > 1 && pq[k / 2] < pq[k]) {
  		swap(pq[k / 2], pq[k]);
  		k /= 2;
  	}
  }
  void sink(int k) {
  	// 自顶向下
  	while (k * 2 + 1 <= pq.size()) {
  		int j = 2 * k;
  		if (j + 1 <= pq.size() && pq[j] < pq[j + 1]) ++j;
  		if (pq[k] > pq[j]) break;
  		swap(pq[k], pq[j]);
  		k = j;
  	}
  }
  ```
- 插入元素：先将元素放到数组最后，再对数组调整
- 删除顶部元素：先将顶部元素与最后一个元素换位，去除最大元素后再对数组做调整

### 二叉查找树(BST)

- 定义：每个节点包含一对键值对`key -> value`的有序二叉树
- 基本构造
```cpp
// C++实现
struct TreeMap {
	int key;
	int val;
	TreeMap* left;
	TreeMap* right;
	TreeMap() :key(0), val(0), left(nullptr), right(nullptr) {}
	TreeMap(int key, int val) :key(key), val(val), left(nullptr), right(nullptr) {}
	TreeMap(int key, int val, TreeMap* left, TreeMap* right) :key(key), val(val), left(left), right(right) {}
};
TreeMap* push(TreeMap* root, int key, int val) {
	if (!root) { return new TreeMap(key, val); }
	if (root->key == key) root->val = val;
	else if (root->key < key) root->right = push(root->right, key, val);
	else root->left = push(root->left, key, val);
	return root;
}
int find(TreeMap* root, int target_key) {
	if (root->key == target_key) return root->val;
	else if (target_key < root->key)
		return !root->left ? 0 : find(root->left, target_key);
	else return !root->right ? 0 : find(root->right, target_key);
}
TreeMap* findMin(TreeMap* root) { return !root->left ? root : findMin(root->left); }
TreeMap* remove(TreeMap* root, int key) {
	if (!root) return nullptr;
    if (key == root->key) {
        if (!root->left && !root->right) {
            // 若左右节点均空，直接删除后返回
			delete root;
			return nullptr;
		}
		else if (!root->left && root->right) {
            // 若左节点空而右节点非空，取右节点代替根节点
			TreeMap* temp = root;
			root = root->right;
			delete temp;
		}
		else if (root->left && !root->right) {
            // 若左节点非空而右节点空，取左节点代替根节点
			TreeMap* temp = root;
			root = root->left;
			delete temp;
		}
		else {
            // 若左右节点均非空，找到前驱节点或后继结点后交换
            // 回到其他情况，递归删除
			TreeMap* minNode = findMin(root->right);
			root->key = minNode->key;
			root->val = minNode->val;
			root->right = remove(root->right, minNode->key);
		}
    }
	else if (key < root->key) root->left = remove(root->left, key);
	else if (key > root->key) root->right = remove(root->right, key);
	return root;
}
```

### 平衡查找树

- 我们称一颗平衡查找树中任意一个节点为 **n- 节点**，如果这个节点含有 $n-1$ 个键和 $n$ 条链接

#### 2-3 查找树
- 由2- 节点、3- 节点和空节点组成
  - 2- 节点的左链接都指向键比当前节点小的节点，右链接都指向键比当前节点大的节点
  - 3- 节点的左右链接与2- 节点相同，中节点指向键位于该节点的两个键之间的节点

- 为保持平衡性，所有空链接到根节点的距离之差绝对值应不超过1，完美平衡的2-3查找树所有空链接到根节点的距离应当是相同的
- 插入新的键值对 
  - 先进行一次未命中的查找，到达最后位置
  - 若结束于 2- 节点，将 2- 节点替换为 3- 节点，将这对新键值对保存在其中
  - 若结束于 3- 节点，分成下面几种情况讨论：
    - 向一棵只含有一个3-节点的树中插入新键：先临时将新键存入该节点，使之成为一个4-节点，然后将其转换为一棵由3个2-节点组成的2-3树(2-3树的生长)
    - 向一个父节点为2-节点的3-节点中插入新键：3-节点变4-节点，再将其拆分，上升部分传给父节点，于是该节点于其父节点都成为了3-节点
    - 向一个父节点为3-节点的3-节点中插入新键：同上递归
    - 分解根节点：同第一种情况
  - 显然，树中的任何地方都可以进行变换，不局限于树的底部

为了简化实现2-3查找树的代码，需要引入红黑二叉查找树的概念
#### 红黑树

- 两种链接：
  - ==黑链接==：普通链接
  - ==红链接==：将两个2-节点连起来的链接
    - 3-节点可以表示为一条左斜的红链接
- 等价定义：
  - 红黑树可看成含有红黑链接且满足下面条件的二叉查找树：
    - 红链接均为左链接
    - 没有任何一个节点同时与两条红链接相连
    - 任意空链接到根节点的路径上的黑链接数量相同
  - 将红链接拉成水平，则红黑树也可以看成是只有2- 节点的2-3查找树
- 基本构造：
  - 一个节点的颜色指其父节点指向它的链接的颜色
    - 每个节点最开始都是黑色的
  - ==根节点总是黑色的==
  - 空节点定义为黑色节点
 ```cpp
  // C++实现
 enum color { BLACK, RED };
 struct RBTreeNode {
 	int key;
 	int val;
 	bool color;
 	RBTreeNode* left;
 	RBTreeNode* right;
 	RBTreeNode() :key(0), val(0), color(BLACK), left(nullptr), right(nullptr) {}
 	RBTreeNode(int key, int val) :key(key), val(val), color(BLACK), left(nullptr), right(nullptr) {}
 	RBTreeNode(int key, int val, bool color) :key(key), val(val), color(color), left(nullptr), right(nullptr) {}
 	RBTreeNode(int key, int val, RBTreeNode* left, RBTreeNode* right) :key(key), val(val), color(BLACK), left(left), right(right) {}
 };
 
 bool empty(RBTreeNode* root) { return !root; }
 bool isRed(RBTreeNode* root) { return root->color == RED; }
 int size(RBTreeNode* root) { return !root ? 0 : 1 + size(root->left) + size(root->right); }
 RBTreeNode* minNode(RBTreeNode* root) { return !root->left ? root : minNode(root->left); }
 RBTreeNode* maxNode(RBTreeNode* root) { return !root->right ? root : maxNode(root->right); }
 int get(RBTreeNode* root, int key) {
 	RBTreeNode* pt = root;
 	while (pt) {
 		if (pt->key == key) return pt->val;
 		else if (pt->key > key) pt = pt->left;
 		else if (pt->key < key) pt = pt->right;
 	}
 	return 0;
 }
 ```

- 旋转链接
	- 旋转重置只需最相反操作即可

```cpp
// 旋转链接
// 将红色的右链接转换为左链接
RBTreeNode* rotateLeft(RBTreeNode* root) {
	// 取当前节点的右节点作为新的头节点
	RBTreeNode* tmp = root->right;
	root->right = tmp->left;
	// 新节点的颜色继承原节点的颜色
	tmp->color = root->color;
	// 原节点的颜色变为红色
	root->color = RED;
	return tmp;
}
// 将红色的左链接转换为右链接
RBTreeNode* rotateRight(RBTreeNode* root) {
	// 取当前节点的左节点作为新的头节点
	RBTreeNode* tmp = root->left;
	root->left = tmp->right;
	tmp->color = root->color;
	root->color = RED;
	return tmp;
}
```

- 颜色转换：将一个节点的两个红色子节点都变黑，同时将该节点变红

```cpp
// 颜色转换
void flipColors(RBTreeNode* root) {
	root->color = RED;
	root->left->color = BLACK;
	root->right->color = BLACK;
}
```

- 调节红黑树平衡

```cpp
// 调节红黑树平衡
RBTreeNode* balance(RBTreeNode* root) {
	if (root->right->color == RED) root = rotateLeft(root);
	if (root->right->color == RED && root->left->color != RED)    root = rotateLeft(root);
	if (root->left->color == RED && root->left->left->color == RED) root = rotateRight(root);
	if (root->left->color == RED && root->right->color == RED)     flipColors(root);
	return root;
}
```

- 插入新键
  - 插入的新节点最开始都是红的
  - 向单个2-节点插入新键：
    - 向左插入：插入后设定新节点颜色为红色
    - 向右插入：插入设颜色后旋转
  - 向树底部的2-节点插入新键：插入设颜色后旋转
  - 向一棵双键树(即一个3-节点中插入新键)：
    - 新键大于原树中的两个键：插入初始颜色为红色的新节点，再将两个子节点的颜色都变为黑色
    - 新键小于原树中的两个键：插入初始颜色为红色的新节点后旋转，回到第一种情况处理
    - 新键介于两键之间：同情况二

```cpp
// 插入新键
RBTreeNode* putHelper(RBTreeNode* root, int key, int val) {
	if (!root) return new RBTreeNode(key, val, RED);
	if (root->key > key) root->left = putHelper(root->left, key, val);
	else if (root->key < key) root->right = putHelper(root->right, key, val);
	else root->val = val;
	// 对插入后的部分做旋转操作
	if (root->right->color == RED && root->left->color != RED) root = rotateLeft(root);
	if (root->left->color == RED && root->left->left->color == RED) root = rotateRight(root);
	if (root->left->color == RED && root->right->color == RED) flipColors(root);
	return root;
}
void put(RBTreeNode* root, int key, int val) {
	root = putHelper(root, key, val);
	root->color = BLACK;
}
```

- 删除：
  - 删除最小的键：
```cpp
// 删除最小键
RBTreeNode* moveRedLeft(RBTreeNode* root) {
	// 假设节点root为红色,root->left和root->right都是黑色
	// 将root->left或root->left的子节点之一变红
	flipColors(root);
	if (root->right->left->color == RED) {
		root->right = rotateRight(root->right);
		root = rotateLeft(root);
	}
	return root;
}
RBTreeNode* deleteMinHelper(RBTreeNode* root) {
	if (!root->left) return nullptr;
	if (root->left->color != RED && root->left->left->color != RED)
		root = moveRedLeft(root);
	root->left = deleteMinHelper(root->left);
	return balance(root);
}
void deleteMin(RBTreeNode* root) {
	if (root->left->color != RED && root->right->color != RED)
		root->color = RED;
	root = deleteMinHelper(root);
	if (root) root->color = BLACK;
}
```
  - 删除最大的键：
```cpp
// 删除最大键
RBTreeNode* moveRedRight(RBTreeNode* root) {
	// 假设节点root为红色,root->left和root->right都是黑色
	// 将root->right或root->right的子节点之一变红
	flipColors(root);
	if (root->left->left->color == RED)
		root = rotateRight(root);
	return root;
}
RBTreeNode* deleteMaxHelper(RBTreeNode* root) {
	if (root->left->color == RED) root = rotateRight(root);
	if (!root->right) return nullptr;
	if (root->right->color != RED && root->right->left->color != RED)
		root = moveRedRight(root);
	root->right = deleteMaxHelper(root->right);
	return balance(root);
}
void deleteMax(RBTreeNode* root) {
	if (root->left->color != RED && root->right->color != RED)
		root->color = RED;
	root = deleteMaxHelper(root);
	if (root) root->color = BLACK;
}
```
  - 删除任意键：
```cpp
// 删除任意键
RBTreeNode* deleteHelper(RBTreeNode* root, int key) {
	if (root->key > key) {
		if (root->left->color != RED && root->left->left->color != RED)
			root = moveRedLeft(root);
		root->left = deleteHelper(root->left, key);
	}
	else {
		if (root->left->color == RED) root = rotateRight(root);
		if (root->key == key && !root->right) return nullptr;
		if (root->right->color != RED && root->right->left->color != RED)
			root = moveRedRight(root);
		if (root->key == key) {
			root->val = get(root->right, minNode(root->right)->key);
			root->key = minNode(root->right)->key;
			root->right = deleteMinHelper(root->right);
		}
		else root->right = deleteHelper(root->right, key);
	}
	return balance(root);
}
void deleteKey(RBTreeNode* root, int key) {
	if (root->left->color != RED && root->right->color != RED)
		root->color = RED;
	root = deleteHelper(root, key);
	if (root) root->color = BLACK;
}
```

## 哈希表

- 用数组构建无序符号表，将键作为数组索引，对应的索引的值就是与键相连的值
- 空间换时间
- 