Primary-Algorithm
=================
Get accurent results

Max-flow
-----------------
### BFS
    bool EK_BFS(){
            queue<int> node;
            bool visit[maxm];
            memset( visit, false, sizeof(visit) );
            memset( bmap, 0, sizeof(bmap) );
            int cur;
            node.push( source );
            visit[source] = true;
            while( !node.empty() ){
                    cur = node.front();
                    node.pop();
                    for( int i = 1; i <= n; i++ ){
                            if( !visit[i] && flow[i][cur] ){
                                    visit[i] = true;
                                    node.push(i);
                                    bmap[i] = cur;
                                    if( i == destination )
                                            return true;
                            }
                    }
            }
            return false;
    }
