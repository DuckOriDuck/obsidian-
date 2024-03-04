# 크루스칼 알고리즘
간선을 오름차순 정렬해서 cycle이 발생하지 않게 제일 작은것부터 더해준다
간선의 크기대로 더해진다
### [프로그래머스 섬 연결하기](https://school.programmers.co.kr/learn/courses/30/lessons/42861)
```C++
#include <string>
#include <vector>
#include <queue>

using namespace std;
struct Edge{
    int s;
    int e;
    int w;
    Edge(int start, int end, int weight){
        s= start;
        e = end;
        w = weight;
    }
    Edge(vector<int> &v){
        s=v[0];
        e=v[1];
        w=v[2];
    }
};

struct cmp{
    bool operator ()(Edge a, Edge b){
        return a.w>b.w;
    }
};

int solution(int n, vector<vector<int>> costs) {
    int answer = 0;
    priority_queue<Edge, vector<Edge>, cmp> pq;
    vector<bool> connected =vector<bool>(n,false);
    
    for(auto i: costs){
        pq.push(Edge(i));
    }
    
    while(!pq.empty()){
        Edge e = pq.top();
        pq.pop();
        if(connected[e.s]&&connected[e.e])
            continue;
        else{
            connected[e.s]=true;
            connected[e.e]=true;
            answer+=e.w;
        }
    }    
    
    return answer;
}


```
### 틀렸다고 한다.
이렇게 해버리면, 이미 그룹화가 된 2개의 집단을 합칠수가 없으니 union-find 방식을 통해서 더해주고+  cycle이 안생기게 해줘야 한다(+그냥 union find 쓰면 제대로 된다.)
### 정답 코드
```C++
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<int> union_v;

struct Edge{
    int s;
    int e;
    int w;
    Edge(int start, int end, int weight){
        s= start;
        e = end;
        w = weight;
    }
    Edge(vector<int> &v){
        s=v[0];
        e=v[1];
        w=v[2];
    }
};

struct cmp{
    bool operator ()(Edge a, Edge b){
        return a.w > b.w;
    }
};

int find_parent(int a){
    while(union_v[a]!=a)
        a = union_v[a];
    return a;
}

void join_union(int a,int b){
    int tmp1 = find_parent(a);
    int tmp2 = find_parent(b);

    if(tmp1<tmp2)union_v[tmp2]=tmp1;
    else union_v[tmp1]=tmp2;
}

bool if_union(int a, int b){
    return (find_parent(a)==find_parent(b));
}

int solution(int n, vector<vector<int>> costs) {
    int answer = 0;
    
    
    priority_queue<Edge, vector<Edge>, cmp> pq;
    
    union_v = vector<int>(n+1,0);
    for(int i =0;i<=n;i++){
        union_v[i]=i;
    }
    
    for(auto i: costs){
        pq.push(Edge(i));
    }
    
    while(!pq.empty()){
        Edge e = pq.top();
        pq.pop();
        
        if(if_union(e.s,e.e))
            continue;
        else{
            join_union(e.s, e.e);
            answer+=e.w;
        }
    }    
    
    return answer;
}
```
PQ와 uinion-find를 아주 잘 구현해놓은 모습이다. 

# 프림 알고리즘:
정점을 선택해나가는 방식. 선택해둔 정점에서 연결 할 수 있는 간선이 가장 작은것부터 골라나간다.