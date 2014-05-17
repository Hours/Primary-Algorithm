Primary-Algorithm
=================
Get accurent results

DP( Dynamic Programming )：
----------------------
### LCS( Longest Common Subsequence )
		dp[i][j]：s1...si和t1...tj对应的LCS长度
			s[i+1] = t[j+1]，在s1...si和t1...tj对应的LCS末尾追加s[i+1]
			s1...si和t1...tj+1的LCS
			s1...si+1和t1...tj的LCS 
		dp[i+1][j+1] = max(dp[i][j]+1,dp[i][j+1],dp[i+1][j] )     (si+1=tj+1)
                        max( dp[i][j+1],dp[i+1][j] )               (others)
```
	void solve(){   
		for( int i = 0; i < n; i++ )
			for( int j = 0; j < m; j++ )
				if( s[i] == t[j] )
					dp[i+1][j+1] = dp[i][j]+1;
				else
					dp[i+1][j+1] = max( dp[i][j+1], dp[i+1][j] );
		cout << dp[n][m];
	}
```

### KP( Knapsack problem )
__0-1背包:__
```
	有n个重量和价值分别为wi,vi的物品。从这些物品中挑选出总重量不超过W的物品，求所有挑选方案中价值总和的最大值
		dp[i+1][j]：从前i个物品中选出总重量不超过j的物品时总价值的最大值
		dp[0][j] = 0
		dp[i+1][j] = dp[i][j] (j<w[i])
			max(dp[i][j], dp[i][j-w[i]]+v[i]) (others)
		void solve(){
			for( int i = 0; i < n; i++ ){
				for( int j = 0; j <= W; j++ ){
					if( j < w[i] )
						dp[i+1][j] = dp[i][j];
					else
						dp[i+1][j] = max(dp[i][j], dp[i][j-w[i]]+v[i]);
				}
			}
			cout << dp[n][W];
		}
		——————————————————————————
		void solve(){
		  memset( dp, 0, sizeof(dp) );
			for( int i = 0; i < n; i++ ){
				for( int j = W; j >= w[i]; j-- )
					dp[j] = max(dp[j], dp[j-w[i]]+v[i]);
			cout << dp[W];
		｝
		注：根据限制条件不同，时间复杂度O(nW)可能会有超时现象。换个角度，重量限制下的价值最大O(nW)〈=〉同一价值下的重量最小O(nV)。
```
__完全背包:__
```
	有n个重量和价值分别为wi,vi的物品。从这些物品中挑选出总重量不超过W的物品，求所有挑选方案中价值总和的最大值。在这里每种物品可以挑选任意多种。
		dp[i+1][j]：从前i个物品中选出总重量不超过j的物品时总价值的最大值
		dp[0][j] = 0
		dp[i+1][j] = max{ dp[i][j-k*w[i]]+k*v[i] | k>=0 }
		void solve(){
			memset( dp, 0, sizeof(dp) );
			for( int i = 0; i < n; i++ )
				for( int j = w[i]; j <= W; j++ )
					dp[j] = max( dp[j], dp[j-w[i]]+v[i] );
			cout << dp[W];
		}
```
__多重部分和（限定个数的背包问题）:__
```
		有n种不同大小的数字ai，每种各mi个。判断是否可以从这些数字之中选出若干使它们的和恰好为K。
		
		dp[i+1][j]：用前i中数字是否能加和成j
		dp[i+1][j] = (0<=k<=mi && k*ai<=j时存在使dp[i][j-k*ai]为真的k)
		void solve(){
			dp[0][0] = true;
			for( int i = 0; i < n; i++ )
				for( int i = 0; i < n; i++ )
					for( int k = 0; k < m[i]; && k*a[i] <= j; k++ ){
						dp[i+1][j] |= dp[i][j-k*a[i]];
					}
			if( dp[n][K] )
				cout << "Yes\n";
			else
				cout << "No\n";
		}
		注：时间复杂度O(KΣimi)
		dp[i+1][j]：用前i种数加和得到j时第i种数最多能剩余多少个
			     mi		(dp[i][j] >= 0 )
		dp[i+1][j] = -1		(j<ai || dp[i+1][j-ai] <= 0 )
			     dp[i+1][j=ai]+1	(others)
		void solve(){
			memset( dp, -1, sizeof(dp) );
			dp[0] = 0;
			for( int i = 0; i < n; i++ )
				for( int j = 0; j <= K; j++ )
					if(dp[j] >= 0)
						dp[j] = m[i];
					else if( j < a[i] || dp[j-a[i]] <= 0 )
						dp[j] = -1;
					else
						dp[j] = dp[j-a[i]] - 1;
			if( dp[K] >= 0 )
				cout << "Yes\n";
			else
				cout << "No\n";
		}
		注：时间复杂度O(nK)
```
### LIS( Longest Increasing Subsequence )
```
		求出序列中最长上升子序列
		
		dp[i]:以ai为末尾的最长上升子序列的长度
		在满足j<i并且aj < ai的以aj为结尾的上升子列末尾，追加上ai后得到的子序列
		dp[i] = max{1,dp[j]+1 | j<i && aj < ai }
		void solve(){
			int res = 0;
			for( int i = 0; i < n; i++ ){
				dp[i] = 1;
				for( int j = 0; j < i; j++ )
					if( a[j] < a[i] )
						dp[i] = max( dp[i], dp[j]+1 );
				res = max(res, dp[i]);
			}
			cout << res <<endl;
		}
 		注：时间复杂度O(n^2)
 		void solve(){
 			fill( dp, dp+n, INF );
 			for( int i = 0; i < n; i++ )
 				*lower_bound(dp, dp+n, a[i]) = a[i];
 			cout << lower_bound(dp, dp+n, INF)-dp <<endl;
 		}
 		注：时间复杂度O(nlogn)
```
### 划分数
		有n个无区别的物品，将它们划分成不超过m组，求划分方法数。
		这样的划分被称作n的m划分，特别地，m = n时称作n的划分数。
		
		dp[i][j] = j的i划分的总数
		*如果对于每个i都有ai>0，那么ai-1就对应了n-m的m划分；
		*如果存在ai=0，那么就对应了n的m-1划分，复杂度
		dp[i][j] = dp[i][j-i] + dp[i-1][j]
		void solve(){
			dp[0][0] = 1;
			for( int i = 0; i <= m; i++ )
				for( int j = 0; j <= n; j++ )
					if( j-i >= 0 )
						dp[i][j] = dp[i-1][j] + dp[i][j-1];
					else
						dp[i][j] = dp[i-1][j];
			cout << dp[m][n] << endl;
		}
		注：时间复杂度O(nm)
### 多重集组合数
		有n种物品，第i种物品有ai个。不同种类的物品可以相互区分但相同种类的无法区分。从这些物品中取出m个的话，有多少种取法？求出方案数。
 
