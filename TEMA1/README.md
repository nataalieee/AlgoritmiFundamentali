## Tema 1 Lab AF - Pogar Denisa Natalia 251 

### Problem 1 - sate

https://www.infoarena.ro/job_detail/3371221?action=view-source

<img width="50%" height="396" alt="image" src="https://github.com/user-attachments/assets/1a14f2f8-fc1e-44ef-a7e6-6ce6ea37f57a" />

```cpp
#include <bits/stdc++.h>
#define nmax 30005
using namespace std;

ifstream f("sate.in");
ofstream g("sate.out");


int n,m,x,y,l,r,w;
int dist[nmax];
bool viz[nmax];
vector<pair<int,int>> adj[nmax];
queue<int> q;

void bfs(int nod) {
    q.push(nod);
    viz[nod]=1;
    while (q.size()) {
        int nod=q.front();
        q.pop();

        for (int j=0;j<adj[nod].size();j++) {
            int vecin=adj[nod][j].second;
            int new_cost=adj[nod][j].first;
            if (!viz[vecin]) {
                dist[vecin]=dist[nod]+new_cost;
                q.push(vecin);
                viz[vecin]=1;
            }
        }
    }
}
int main() {
    f>>n>>m>>x>>y;
    for (int i=1;i<=m;i++) {
        f>>l>>r>>w;
        adj[l].push_back({w,r});
        adj[r].push_back({-w,l});
    }
    
    dist[x]=0;
    bfs(x);
    g<<dist[y];
    return 0;
}
```
### Problem 2 - drumuri 

https://www.infoarena.ro/job_detail/3371573?action=view-source

<img width="50%" height="432" alt="image" src="https://github.com/user-attachments/assets/ec1ef0f7-e8d1-4053-bb0f-d24b79ee4860" />

```cpp
#include <bits/stdc++.h>
#define nmax 10005
using namespace std;

ifstream f("drumuri.in");
ofstream g("drumuri.out");

typedef pair<int,int> pii;
int n,m,l,r;
int lvl[nmax];
bool viz[nmax];
vector<int> adj[nmax];

int dfs(int nod, int tata) {
    viz[nod]=1;
    lvl[nod]=lvl[tata]+1;
    vector<int> unused_edges;

    for (auto vecin:adj[nod]) {
        if (vecin==tata) continue;
        if (viz[vecin] && lvl[vecin]<lvl[nod]) {
            unused_edges.push_back(vecin);
        }
        else if (!viz[vecin]) {
            int nod_ramas=dfs(vecin,nod);
            if (nod_ramas>0) {
                unused_edges.push_back(nod_ramas);
            }
        }
    }
    if (tata!=0) unused_edges.push_back(tata);

    for (int i=0;i+1<unused_edges.size();i+=2) {
        g<<unused_edges[i]<<" "<<nod<<' '<<unused_edges[i+1]<<endl;
    }
    if (unused_edges.size()%2==1) {
        return nod;
    }
    return 0;

}
int main() {
    f>>n>>m;
    for (int i=1;i<=m;i++) {
        f>>l>>r;
        adj[l].push_back(r);
        adj[r].push_back(l);
    }
    if (m%2) {
        g<<0<<'\n';
        return 0;
    }
    g<<1<<'\n';
    dfs(1,0);


    return 0;
}
```

### Problem 3 - pamant 

https://www.infoarena.ro/job_detail/3371333?action=view-source

<img width="50%" height="452" alt="image" src="https://github.com/user-attachments/assets/32622140-6be1-4dbf-8f02-51ab98432631" />

```cpp
#include <bits/stdc++.h>
#define nmax 100005
using namespace std;
ifstream f("pamant.in");
ofstream g("pamant.out");

int n,m,x,y,lvl[nmax], low[nmax],timp;
bool viz[nmax];
vector<int> adj[nmax],coduri_echipe;
set<int>pcte_art;
void dfs(int nod, int tata, int radacina) {
    viz[nod]=1;
    timp++;
    lvl[nod]=low[nod]=timp;
    int fii_rad=0;

    for (int j=0;j<adj[nod].size();j++) {
        int vecin=adj[nod][j];
        if (vecin==tata) continue;
        if (viz[vecin]==1) { //muchie de intoarcere
            low[nod]=min(lvl[vecin], low[nod]);
        }
        else if (!viz[vecin]) { //muchie de arbore
            if (nod==radacina) fii_rad++;

            dfs(vecin,nod,radacina);
            low[nod]=min(low[vecin], low[nod]);

            if (low[vecin]>=lvl[nod] && nod!=radacina) {
                pcte_art.insert(nod);
            }
        }
    }
    if (nod == radacina && fii_rad >= 2) {
        pcte_art.insert(nod);
    }
}
int main() {
    f>>n>>m;
    for (int i=1;i<=m;i++) {
        f>>x>>y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    for (int i=1; i<=n; i++) {
        if (!viz[i]) {
            coduri_echipe.push_back(i);
            dfs(i, 0,i);
        }
    }
    g<<coduri_echipe.size()<<'\n';
    for (auto cod:coduri_echipe) {
        g<<cod<<' ';
    }
    g<<'\n';
    g<<pcte_art.size()<<'\n';
    for (auto pct: pcte_art) {
        g<<pct<<' ';
    }
    return 0;
}
```

### Problem 4 - ctc 

https://www.infoarena.ro/job_detail/3371088?action=view-source

<img width="50%" height="435" alt="image" src="https://github.com/user-attachments/assets/baeb917a-bf30-4ccb-be97-d0a7b73a04ca" />

```cpp
#include <bits/stdc++.h>
#define nmax 100005
using namespace std;
ifstream f("ctc.in");
ofstream g("ctc.out");

vector<int> adj[nmax],gt[nmax],cc[nmax];
stack <int> s;
int st,dr,n,m,nr_cc;
int viz[nmax];
void dfs(int nod) {
    viz[nod]=1;
    for (int j=0;j<adj[nod].size();j++)
        if (!viz[adj[nod][j]]) dfs(adj[nod][j]);
    s.push(nod);
}
void dfs_gt(int nod, int cnt) {
    cc[cnt-1].push_back(nod);
    viz[nod]=2;
    for (int j=0;j<gt[nod].size();j++) {
        if (viz[gt[nod][j]]==1) dfs_gt(gt[nod][j],cnt);
    }
}
int main() {
    f>>n>>m;

    for (int i=1;i<=m;i++) {
        f>>st>>dr;
        adj[st].push_back(dr);
        gt[dr].push_back(st);
    }

    for (int i=1;i<=n;i++) if (!viz[i]) dfs(i);
    while (s.size()) {
        int varf=s.top(); s.pop();
        
        if (viz[varf]==1) {
            nr_cc++; dfs_gt(varf,nr_cc);
        }
    }
    g<<nr_cc<<'\n';
    for (int i=0;i<nr_cc;i++) {
        for (int j=0;j<cc[i].size();j++)
            g<<cc[i][j]<<' ';
        g<<'\n';
    }
    return 0;
}
```
### Problem 5 - biconex 

https://www.infoarena.ro/job_detail/3371330?action=view-source

<img width="50%" height="487" alt="image" src="https://github.com/user-attachments/assets/8d5209ec-7d95-4203-9b01-84af86903794" />

```cpp
#include <bits/stdc++.h>
#define nmax 100005
using namespace std;
ifstream f("biconex.in");
ofstream g("biconex.out");

int n,m,x,y,lvl[nmax], low[nmax],nr_comp,timp;
bool viz[nmax];
stack<int> s;
vector<int> adj[nmax],cbc[nmax];

void dfs(int nod, int tata) {
    viz[nod]=1;
    timp++;
    lvl[nod]=low[nod]=timp;
    s.push(nod);

    for (int j=0;j<adj[nod].size();j++) {
        int vecin=adj[nod][j];
        if (vecin==tata) continue;
        if (viz[vecin]==1) {
            low[nod]=min(lvl[vecin], low[nod]);
        }
        else if (!viz[vecin]) {
            dfs(vecin,nod);
            low[nod]=min(low[vecin], low[nod]);

            if (low[vecin]>=lvl[nod]) {
                cbc[nr_comp].push_back(nod);
                while (s.top()!=vecin) {
                    cbc[nr_comp].push_back(s.top());
                    s.pop();
                }
                cbc[nr_comp].push_back(s.top()); s.pop();
                nr_comp++;
            }
        }
    }
}
int main() {
    f>>n>>m;
    for (int i=1;i<=m;i++) {
        f>>x>>y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }
    for (int i=1; i<=n; i++) {
        if (!viz[i]) {
            dfs(i, 0);
        }
    }
    g<<nr_comp<<'\n';
    for (int i=0;i<nr_comp;i++) {
        for (int j=0;j<cbc[i].size();j++)
            g<<cbc[i][j]<<' ';
        g<<'\n';
    }
    return 0;
}
```

### Problem 6 - easygraph 

https://www.infoarena.ro/job_detail/3373719?action=view-source

<img width="50%" height="518" alt="image" src="https://github.com/user-attachments/assets/ec4f4ccb-6890-4bf5-b8f1-e90dd67d430e" />

```cpp
#include <bits/stdc++.h>
#define nmax 15005
#define int long long
using namespace std;
ifstream f("easygraph.in");
ofstream g("easygraph.out");

int n,m,x,y,t,val[nmax],dp[nmax];
bool viz[nmax];
vector<int> adj[nmax];
void dfs(int nod) {
    viz[nod]=1;
    for (auto vecin:adj[nod]) {
        if (!viz[vecin]) dfs(vecin);
    }
    if (adj[nod].empty()) {
        dp[nod]=val[nod]; return;
    }
    int best_fiu=0;
    for (auto vecin:adj[nod]) {
        best_fiu=max(best_fiu,dp[vecin]);
    }
    dp[nod]=max(dp[nod],val[nod]+best_fiu);

}
signed main() {
    f>>t;
    for (int i=1;i<=t;i++) {
        f>>n>>m;
        for (int j=1;j<=n;j++) {
            f>>val[j];
        }
        for (int j=1; j<=m; j++) {
            f>>x>>y;
            adj[x].push_back(y);
        }
        for (int j=1;j<=n;j++) {
            dp[j]=LONG_LONG_MIN;
        }
        for (int j=1;j<=n;j++) {
            if (!viz[j]) dfs(j);
        }
        int rasp=dp[1];
        for (int j=2;j<=n;j++) {
            rasp=max(rasp, dp[j]);
        }
        g<<rasp<<'\n';
        memset(viz,0,sizeof viz);
        for (int j=1;j<=n;j++)  adj[j].clear();
    }
    return 0;
}
```

### Problem 7 - alice2 

https://www.infoarena.ro/job_detail/3371811?action=view-source

<img width="50%" height="553" alt="image" src="https://github.com/user-attachments/assets/fc3463f5-4cec-4a3a-9920-edd869ed06dd" />

```cpp
#include <bits/stdc++.h>
#define nmax 100005
using namespace std;
ifstream f("alice2.in");
ofstream g("alice2.out");

int n,m,x,y,t;
string s;
vector<int> adj[nmax];
int dist[nmax];
bool viz[nmax];
queue<int> q;
unordered_map<char,int>last;
void bfs(int nod) {
    q.push(nod);
    dist[nod]=0;
    viz[nod]=1;
    while (q.size()) {
        int nod=q.front();
        q.pop();
        for (auto vecin:adj[nod]) {
            if (!viz[vecin]) {
                q.push(vecin);
                dist[vecin]=dist[nod]+1;
                viz[vecin]=1;
            }
        }
    }
}
int main() {
    f>>s;
    n=s.size();
    for (int lit=s.size()-1;lit>=0;lit--) {
        if (last[s[lit]]!=0) {
            adj[lit+1].push_back(last[s[lit]]);
            adj[last[s[lit]]].push_back(lit+1);
        }
        last[s[lit]]=lit+1;
    }
    for (int i=1;i<n;i++) {
        adj[i].push_back(i+1);
        adj[i+1].push_back(i);
    }
    //for (int i=1;i<=n;i++) dist[i]=INT_MAX;
    bfs(1);
    g<<dist[n];
    return 0;
}
```

### Problem 8 - autobuze 

https://www.infoarena.ro/job_detail/3373737?action=view-source

<img width="50%" height="536" alt="image" src="https://github.com/user-attachments/assets/5f5daecf-6ec5-4fa4-ad6c-70b61b511335" />

```cpp
#include <bits/stdc++.h>
#define nmax 50005
using namespace std;
ifstream f("autobuze.in");
ofstream g("autobuze.out");

int arr[nmax];
map<int,int> parent;
bitset<10000003> found;
int n, nr_cc;

int findroot(int nod) {
    if (parent[nod]==0) return nod;
    int rad=findroot(parent[nod]);
    parent[nod]=rad;
    return rad;
}

int main() {
    f>>n;
    nr_cc=n;
    int val_max=0;

    for (int i=1; i<=n; i++) {
        f>>arr[i];
        found[arr[i]]=1;
        if (arr[i]>val_max) {
            val_max=arr[i];
        }
    }
    
    for (int i=1; i<=n; i++) {
        int crt=arr[i];
        for (int j=2*crt;j<=val_max;j+=crt) {
            if (found[j]) {
                int rad_crt=findroot(crt);
                int rad=findroot(j);
                if (rad_crt!=rad) {
                    nr_cc--;
                    parent[rad]=rad_crt;
                }
            }

        }
    }

    g<<nr_cc<<'\n';

    return 0;
}
```
