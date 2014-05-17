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
### LIS( Longest Increasing Subsequence )
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
