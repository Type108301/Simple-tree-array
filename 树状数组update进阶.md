单点赋值
* 加法
	```cpp
	void update(int x,int k){
		int delta=k-a[x];
		a[x]=k;
		while(x<=n){
			tree[x]+=delta;
			x+=lowbit(x);
		}
	}
	```
* 乘法
	```cpp
	void update(int x,int k){
		int delta=k/a[x];
		a[x]=k;
		while(x<=n){
			tree[x]*=delta;
			x+=lowbit(x);
		}
	}
	```
* 异或
	```cpp
	void update(int x,int k){
		int delta=k^a[x];
		a[x]=k;
		while(x<=n){
			tree[x]^=delta;
			x+=lowbit(x);
		}
	}
	```
* 关于DFS序
	* 如果不是赋值（比如x处加k）就传入`dfn[x]`
		```cpp
		update(dfn[x],k);
		```
	* 如果是赋值就不能传入，`update`处要改动一下
		```cpp
		void update(int x,int k){
			int delta=k-a[x];
			a[x]=k;
			x=dfn[x]; //转到dfs序位置
			while(x<=n){
				tree[x]+=delta;
				x+=lowbit(x);
			}
		}
		
		update(x,k);
		```