[백준 1208번](https://www.acmicpc.net/problem/1208)
[참고 블로그](https://hyeo-noo.tistory.com/128)
부분수열의 모든 합을 구하는 재귀함수가 인상적이어서 기록했다.
```C++
#include <iostream>
#include <vector>
#include <map>
#include <algorithm>
using namespace std;

vector<int> v;
map<int, int> sum;
int N, S;
long long ans;

void right(int prior, int m) {
	if (m == N) {
		sum[prior]++;
		return;
	}
	right(prior + v[m], m + 1);
	right(prior, m + 1);
}

void left(int prior, int s) {
	if (s == N / 2) {
		ans += sum[S - prior];
		return;
	}

	left(prior + v[s], s + 1);
	left(prior, s + 1);
}

int main() {
	
	cin >> N>>S;
	ans = 0;
	v = vector<int>(N+1, 0);
	for (int i = 0; i <N; i++) {
		cin >> v[i];
	}

	int mid = N / 2;
	right(0, mid);
	left(0, 0);

	if (S==0) cout << ans - 1;
	else cout << ans;


}
```