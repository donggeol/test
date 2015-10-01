#include<stdio.h>
#include<algorithm>

#define Nmax 110

int N,M,An,T;
int A[Nmax],D[Nmax][Nmax],V[Nmax],Answer[Nmax];

void dfs(int x)
{
	int y,ch;
	if(x == N){
		An = 1;
		return;
	}
	for(int i = 2; i < M; i++){
		if(V[i] != 0) continue;
		V[i] = x;
		y = A[i] - Answer[0];
		for(int j = 1; j < x; j++){
			ch = 0;
			for(int k = 2; k < M; k++){
				if(V[k] != 0) continue;
				if(A[k] == y + Answer[j]){
					V[k] = x;
					ch = 1;
					break;
				}
			}
			if(ch == 0) break;
		}
		if(ch == 0){
			for(int k = 2; k < M; k++){
				if(V[k] == x) V[k] = 0;
			}
			continue;
		}
		Answer[x] = y;
		dfs(x + 1);
		if(An == 1) return;
		for(int k = 2; k < M; k++){
			if(V[k] == x) V[k] = 0;
		}
	}
}

int main()
{
	An = -1;
	while (scanf ("%d", &N) == 1) {
		An = 0;
		M = N * (N - 1) / 2;
		for(int i=0;i<M;i++) V[i] = 0;
		for(int i = 0; i < M; i++) scanf("%d",&A[i]);
		std :: sort(A , A+M);
		D[0][1] = A[0];
		D[0][2] = A[1];
		for(int i = 2; i < M; i++){
			Answer[0] = (A[0] + A[1] + A[i]) / 2 - A[i];
			Answer[1] = (A[0] + A[1] + A[i]) / 2 - A[1];
			Answer[2] = (A[0] + A[1] + A[i]) / 2 - A[0];
			if(Answer[0] + Answer[1] != A[0] || Answer[0] + Answer[2] != A[1] || Answer[1] + Answer[2] != A[i]) continue;
			V[0] = V[1] = V[i] = 1;
			dfs(3);
			V[0] = V[1] = V[i] = 0;
			if(An == 1) break;
		}
		if(An == 0) printf("Impossible");
		else{
			for(int i = 0;i < N;i++){
				if(i) printf(" %d",Answer[i]);
				else  printf("%d",Answer[i]);
			}
		}
		printf("\n");
	}
	return 0;
}