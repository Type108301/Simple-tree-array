#### 1. 是什么
就是对树做一次 DFS，**进入节点时给它一个编号**，最后整棵树会被拍成一段连续的数组下标。
#### 2. 核心性质
- 每个节点的**子树**，在 DFS 序里一定是**一段连续区间**
- 区间：`[dfn[u], dfn[u] + size[u] - 1]`
#### 3. 能干什么
- 子树修改 / 子树查询
- 单点修改 + 子树查询
- 子树加、子树求和、子树异或和…
#### 4. 缺点
**只能做子树，不能做路径**
比如：查询从 u 到 v 这条路径的和，DFS 序做不到。
#### 例子(单点修改 + 子树异或和查询)
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 5;   // 数据范围：最多1e5个节点
vector<int> G[N];        // 邻接表：存储整棵树
int a[N], tree[N];      // a[]=节点原值，tree[]=树状数组
int dfn[N], sz[N];      // dfn[]=DFS序编号，sz[]=子树大小
int cnt;                // DFS序编号计数器
int n, m;               // n=节点数，m=操作数

// 树状数组核心：lowbit操作
int lowbit(int x) {
	return x & -x;
}

// 单点修改：把节点x的值改成k
void update(int x, int k) {
	k ^= a[x];          // 计算需要异或的差值（新值^旧值）
	a[x] ^= k;          // 更新原数组a[x]
	x = dfn[x];         // 把节点编号 → 转成DFS序的线性下标
	// 树状数组向上更新
	while (x <= n) {
		tree[x] ^= k;
		x += lowbit(x);
	}
}

// 查询前缀异或和：[1, x]
int query(int x) {
	int res = 0;
	while (x) {
		res ^= tree[x];
		x -= lowbit(x);
	}
	return res;
}

// 查询区间 [l, r] 的异或和
int range(int l, int r) {
	return query(r) ^ query(l - 1);
}

// DFS序：给树拍平成线性数组
void dfs(int u, int fa) {
	dfn[u] = ++cnt;     // 给当前节点u分配DFS序编号
	sz[u] = 1;          // 子树大小初始为1（自己）
	// 遍历所有子节点
	for (int v : G[u]) {
		if (v != fa) {  // 不回走父节点
			dfs(v, u);
			sz[u] += sz[v]; // 累加子树大小
		}
	}
}

int main() {
	cin >> n >> m;
	// 读入每个节点的初始值
	for (int i = 1; i <= n; i++) cin >> a[i];
	// 读入树的边，建树
	for (int i = 1; i < n; i++) {
		int u, v; cin >> u >> v;
		G[u].push_back(v);
		G[v].push_back(u);
	}

	// 初始化DFS序，根节点为1
	dfs(1, 0);

	// 初始化树状数组
	for (int i = 1; i <= n; i++) {
		update(i, a[i]);
	}

	// 处理m个操作
	while (m--) {
		int op, x;
		cin >> op >> x;
		if (op == 1) {
			// 操作1：单点修改，把x改成y
			int y; cin >> y;
			update(x, y);
		} else {
			// 操作2：查询x的子树异或和
			// 子树 = 连续区间 [dfn[x], dfn[x]+sz[x]-1]
			cout << range(dfn[x], dfn[x] + sz[x] - 1) << '\n';
		}
	}
	return 0;
}
```