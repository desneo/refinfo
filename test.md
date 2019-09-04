#include<stdio.h>
#include<stdlib.h>

#define MATRIXSIZE 20
#define WORDSIZE 101

int n = 0;
int m = 0;

char matrix[MATRIXSIZE][MATRIXSIZE] = {0};
char word[WORDSIZE] = {0};

int prom(int x, int y, int wordIndex, int restLen)
{	
	char ch = word[wordIndex];
	if (matrix[x][y] != ch) {
		//不相等时直接返回 
		return 0;
	} else if (matrix[x][y] == ch && restLen == 0) {
		// 匹配成功 
		return 1;
	} 
	
	// 如果不是最上边，则可以往上走
	if (y != n) {
		int res =  prom(x, y + 1, wordIndex + 1, restLen - 1);
		if (res == 1) {
			return 1;
		}
	}
	
	return 0;
}

int main()
{
	scanf("%d %d", &n, &m);	

	getchar();
	scanf("%s", word);
	
	// 获取矩阵
	for (int i = 0; i < n; i++) {
		getchar();
		scanf("%s", *(matrix + i));
	} 
	
	
	
	return 0;
}
